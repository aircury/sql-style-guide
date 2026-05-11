# Query clauses and constructs

## Combine multiple boolean conditions with `BETWEEN` and `IN()`

Use `BETWEEN` instead of combining multiple `AND` conditions to express a range. Use `IN()` instead of chaining `OR` clauses to match against a set of values. Both make the intent clearer and are easier to scan.

##### Example

Bad:
```sql
SELECT *
FROM enrolments
WHERE 
    cohort >= 2022
    AND cohort <= 2024
    AND (
        status = 'active'
        OR status = 'pending'
        OR status = 'deferred'
    )
```

Good:
```sql
SELECT *
FROM enrolments
WHERE 
    cohort BETWEEN 2022 AND 2024
    AND status IN ('active', 'pending', 'deferred')
```

#### Source breakdown

 - **For (1)**: Holywell.
 - **Against (0)**
 - **Not mentioned (8)**: Celko, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## CASE Statements

### Use `CASE` for conditional logic

Use `CASE` expressions for inline conditional logic. When the same expression is compared against multiple values, use the simple `CASE <expression> WHEN <value>` form rather than repeating the expression in every `WHEN` branch.

##### Example

Bad:
```sql
SELECT
    full_name,
    CASE
        WHEN grade_band = 'A' THEN 'Distinction'
        WHEN grade_band = 'B' THEN 'Merit'
        WHEN grade_band = 'C' THEN 'Pass'
        ELSE 'Fail'
    END AS grade_label
FROM assessments
```

Good:
```sql
SELECT
    full_name,
    CASE grade_band
        WHEN 'A' THEN 'Distinction'
        WHEN 'B' THEN 'Merit'
        WHEN 'C' THEN 'Pass'
        ELSE 'Fail'
    END AS grade_label
FROM assessments
```

#### Source breakdown

 - **For (3)**: Holywell, Celko, GitLab.
 - **Against (0)**
 - **Not mentioned (6)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, Mazur, Supabase.

### Use a multi line `CASE` structure

For `CASE` expressions that span multiple lines, indent each `WHEN` one level under `CASE`. Place `THEN` on the same line as its `WHEN`. Put `ELSE` at the same indentation level as `WHEN`. Close with `END` on its own line, aligned with `CASE`.


##### Example

Bad:
```sql
SELECT
    full_name,
    CASE WHEN grade >= 70 THEN 'Pass'
    WHEN grade >= 50 THEN 'Borderline'
    ELSE 'Fail' END AS result
FROM assessments
```

Good:
```sql
SELECT
    full_name,
    CASE
        WHEN grade >= 70 THEN 'Pass'
        WHEN grade >= 50 THEN 'Borderline'
        ELSE 'Fail'
    END AS result
FROM assessments
```

#### Source breakdown

 - **For (2)**: Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Supabase.

### A single short `WHEN`/`THEN` can stay on one line

When a `CASE` has only one `WHEN` branch and the `THEN` value is short, the entire expression can be written on a single line.

##### Example

```sql
SELECT
    full_name,
    CASE WHEN is_active IS true THEN details END AS details
FROM students
```

#### Source breakdown

 - **For (1)**: Brooklyn.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Mazur, Supabase.

### `CASE` inside a function call

When a `CASE` expression is nested inside a function call, place `CASE` and `END` each on their own lines, indented one level inside the function's parentheses.

##### Example

Bad:
```sql
SELECT
    count(CASE WHEN grade >= 50 THEN student_id ELSE NULL END) AS passing_students
FROM assessments
```

Good:
```sql
SELECT
    count(
        CASE
            WHEN grade >= 50 THEN student_id
            ELSE NULL
        END
    ) AS passing_students
FROM assessments
```

#### Source breakdown

 - **For (1)**: Brooklyn.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Mazur, Supabase.

## Prefer `UNION ALL` over `UNION`

`UNION ALL` is more performant than `UNION` because it skips the sort and deduplication step. Only use `UNION` when removing duplicate rows is genuinely part of the requirement.

##### Example

Bad:
```sql
SELECT 
    student_id, 
    course_id 
FROM enrolments 
WHERE status = 'active'
UNION
SELECT 
    student_id, 
    course_id 
FROM archived_enrolments 
WHERE status = 'active'
```

Good:
```sql
SELECT 
    student_id, 
    course_id 
FROM enrolments 
WHERE status = 'active'
UNION ALL
SELECT 
    student_id, 
    course_id 
FROM archived_enrolments 
WHERE status = 'active'
```

#### Source breakdown

 - **For (4)**: Holywell, Celko, Brooklyn, GitLab.
 - **Against (0)**
 - **Not mentioned (5)**: PostgreSQL, PG.ai, Mozilla, Mazur, Supabase.

## Use `SELECT DISTINCT` instead of `GROUP BY` on all columns when deduplicating

When the goal is to remove duplicate rows rather than aggregate, use `SELECT DISTINCT`. Using `GROUP BY` on every column in the select list to achieve the same result obscures intent.

##### Example

Bad:
```sql
SELECT
    student_id,
    course_id
FROM enrolments
GROUP BY
    student_id,
    course_id
```

Good:
```sql
SELECT DISTINCT
    student_id,
    course_id
FROM enrolments
```

#### Source breakdown

 - **For (1)**: Brooklyn.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Mazur, Supabase.

## Avoid `ORDER BY` unless necessary to produce the correct result

Do not add `ORDER BY` to a query just for convenience or aesthetics. Sorting is expensive and the results of most queries will be re-processed downstream (by application code, a window function, or another query) where the order is irrelevant or re-applied anyway.

##### Example

Bad:
```sql
SELECT
    student_id,
    count(course_id) AS total_courses
FROM enrolments
GROUP BY student_id
ORDER BY student_id
```

Good:
```sql
SELECT
    student_id,
    count(course_id) AS total_courses
FROM enrolments
GROUP BY student_id
```

#### Source breakdown

 - **For (1)**: Brooklyn.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Mazur, Supabase.

## *Proposal* Always use explicit column names in `GROUP BY`

Use column names rather than positional numbers in `GROUP BY` clauses. Positional references (`GROUP BY 1, 2`) are fragile, as reordering the `SELECT` list silently changes what is being grouped without any error.

Some consider positional references acceptable when the expression being grouped is complex or repeated verbatim in the `SELECT` list, to avoid duplication. 

What is a consensus between the sources is to never mix positional references with column names in the same `GROUP BY` clause.

##### Example

Bad:
```sql
SELECT
    school_id,
    cohort,
    count(student_id) AS total_students
FROM students
GROUP BY 1, 2
```

Good:
```sql
SELECT
    school_id,
    cohort,
    count(student_id) AS total_students
FROM students
GROUP BY school_id, cohort
```

#### Source breakdown

 - **For explicit names (2)**: PG.ai, Mozilla.
 - **Positional acceptable for complex expressions (4)**: PG.ai, Mozilla, Brooklyn, Mazur.
 - **Not mentioned (5)**: Holywell, Celko, GitLab, Supabase.

## Prefer `WHERE` over `HAVING` when either would suffice

`WHERE` filters rows before aggregation; `HAVING` filters after. When a condition does not depend on an aggregate result, place it in `WHERE`: it is more explicit about intent and filters earlier, reducing the rows that need to be aggregated.

##### Example

Bad:
```sql
SELECT
    school_id,
    count(student_id) AS total_students
FROM students
GROUP BY school_id
HAVING school_id IS NOT NULL
```

Good:
```sql
SELECT
    school_id,
    count(student_id) AS total_students
FROM students
WHERE school_id IS NOT NULL
GROUP BY school_id
```

#### Source breakdown

 - **For (2)**: Brooklyn, GitLab.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Mazur, Supabase.

## Use lateral column aliasing when grouping by name

When an expression in `SELECT` has a column alias, you can reference that alias directly in `GROUP BY` rather than repeating the full expression. This avoids duplication and makes the query easier to maintain.

##### Example

Bad:
```sql
SELECT
    date_trunc('year', enrolled_at) AS enrolment_year,
    count(student_id)               AS total_students
FROM enrolments
GROUP BY date_trunc('year', enrolled_at)
```

Good:
```sql
SELECT
    date_trunc('year', enrolled_at) AS enrolment_year,
    count(student_id)               AS total_students
FROM enrolments
GROUP BY enrolment_year
```

#### Source breakdown

 - **For (1)**: Mazur.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Supabase.

## Place non-aggregated columns first in `SELECT`, aggregated columns last

In a query with aggregation, list the grouping (non-aggregated) columns first, followed by the aggregated expressions. This mirrors the logical order of `GROUP BY` and makes the structure of the query easier to parse at a glance.

##### Example

Bad:
```sql
SELECT
    count(student_id) AS total_students,
    avg(score)        AS avg_score,
    school_id,
    cohort
FROM students
GROUP BY school_id, cohort
```

Good:
```sql
SELECT
    school_id,
    cohort,
    count(student_id) AS total_students,
    avg(score)        AS avg_score
FROM students
GROUP BY school_id, cohort
```

#### Source breakdown

 - **For (2)**: GitLab, Mazur.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Brooklyn, Supabase.

## Be explicit in boolean conditions

Write boolean comparisons using `IS true` and `IS false` rather than relying on implicit boolean evaluation. Why use `IS` instead of `=`? Unlike `= true`, the `IS` form always returns a definite boolean, never propagating `NULL`. When the column is `NULL`, `column = true` evaluates to `NULL` (which SQL treats as falsy), while `column IS true` evaluates to `FALSE`. This distinction matters: `= true` and `IS true` behave the same in a `WHERE` clause, but `IS true` makes the null behaviour explicit and works correctly in `CASE` expressions and derived columns where a `NULL` result would otherwise silently propagate.

##### Example

Bad:
```sql
SELECT *
FROM students
WHERE 
    is_active
    AND NOT has_scholarship
```

Good:
```sql
SELECT *
FROM students
WHERE 
    is_active IS true
    AND has_scholarship IS false
```

#### Source breakdown

 - **For (1)**: Mazur.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Supabase.

## Use `!=` instead of `<>`

Prefer `!=` over `<>` for the not-equal operator. Both are valid SQL, but `!=` is more common in other programming languages and reads more naturally as "not equal".

##### Example

Bad:
```sql
SELECT *
FROM students
WHERE 
    cohort <> 2024
    AND status <> 'withdrawn'
```

Good:
```sql
SELECT *
FROM students
WHERE 
    cohort != 2024
    AND status != 'withdrawn'
```

#### Source breakdown

 - **For (3)**: Brooklyn, GitLab, Mazur.
 - **Against (0)**
 - **Not mentioned (6)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Supabase.

## Break long `IN` lists into multiple indented lines

When an `IN` list contains many values, place each value on its own indented line. This makes the list easier to scan and simplifies adding or removing values in version control diffs.

##### Example

Bad:
```sql
SELECT *
FROM students
WHERE school_id IN (101, 102, 103, 104, 105, 106, 107, 108)
```

Good:
```sql
SELECT *
FROM students
WHERE school_id IN (
    101,
    102,
    103,
    104,
    105,
    106,
    107,
    108
)
```

#### Source breakdown

 - **For (2)**: Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Supabase.

## Avoid redundant parentheses

Do not add parentheses around expressions that do not require them. Extra parentheses suggest a precedence concern where there is none, and add visual noise that makes expressions harder to read.

##### Example

Bad:
```sql
SELECT *
FROM students
WHERE 
    (is_active IS true)
    AND (cohort > 2020)
    AND (school_id IN (101, 102))
```

Good:
```sql
SELECT *
FROM students
WHERE 
    is_active IS true
    AND cohort > 2020
    AND school_id IN (101, 102)
```

#### Source breakdown

 - **For (1)**: Celko.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Keep window functions on one line when they fit. Expand sub-clauses when they don't

If a window function fits on a single line without exceeding the line length limit, keep it there. When it needs to break across lines, place each sub-clause (`PARTITION BY`, `ORDER BY`, `ROWS/RANGE BETWEEN`) on its own line indented inside `OVER()`, and put the closing parenthesis on its own line at the same indentation level as the function.

##### Example

One line (fits):
```sql
SELECT
    student_id,
    row_number() OVER (PARTITION BY cohort ORDER BY score DESC) AS rank
FROM assessments
```

Multi-line (too long):
```sql
SELECT
    student_id,
    row_number() OVER (
        PARTITION BY cohort, school_id
        ORDER BY score DESC, enrolled_at ASC
    ) AS rank
FROM assessments
```

#### Source breakdown

 - **For (2)**: Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Supabase.
