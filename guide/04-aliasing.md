# Alias rules

## Always use the `AS` keyword explicitly

Never omit the keyword when aliasing, as it provides readability.

##### Example

Bad:
```sql
SELECT
    students.full_name                       student_name,
    coalesce(count(enrolments.course_id), 0) total_enrolments
FROM students
LEFT JOIN student_course_enrolments enrolments
    ON students.id = enrolments.student_id
GROUP BY students.full_name
```

Good:
```sql
SELECT
    students.full_name                       AS student_name,
    coalesce(count(enrolments.course_id), 0) AS total_enrolments
FROM students
LEFT JOIN student_course_enrolments AS enrolments
    ON students.id = enrolments.student_id
GROUP BY students.full_name
```

#### Source breakdown

 - **For (9)**: Holywell, Celko, Supabase, Brooklyn, GitLab, Mozilla, Mazur, PG.ai, PostgreSQL.
 - **Against (0)**
 - **Not mentioned (0)**

## Align `AS` keywords

In a `SELECT` list, align the `AS` keywords of consecutive aliased columns: pad each expression so that every `AS` sits one space after the longest aliased expression. Columns without an alias are left as they are, and do not count towards the longest expression. An expression that spans several lines (a broken expression, a multi-line `CASE` or window function) is not aligned either: its `AS` goes one space after the closing line.

Aligned aliases make the names of the result set readable as a single column, and separate what is computed from what it is called.

##### Example

Bad:
```sql
SELECT
    students.full_name AS student_name,
    count(enrolments.course_id) AS total_courses,
    max(enrolments.enrolled_at) AS last_enrolled_at,
    students.cohort
FROM students
INNER JOIN enrolments
    ON students.id = enrolments.student_id
GROUP BY students.full_name, students.cohort
```

Good:
```sql
SELECT
    students.full_name          AS student_name,
    count(enrolments.course_id) AS total_courses,
    max(enrolments.enrolled_at) AS last_enrolled_at,
    students.cohort
FROM students
INNER JOIN enrolments
    ON students.id = enrolments.student_id
GROUP BY students.full_name, students.cohort
```

## Always use aliasing for aggregated and other column expressions

Always use an alias name for columns that are the result of aggregation, `CASE` expressions, `sum()`, `count()`, ... Otherwise, depending on the database provider, the name could be inherited from the column used in the transformation or the function name.

##### Example

Bad:
```sql
SELECT
    cohort_name,
    count(*),
    avg(score)
FROM grades
GROUP BY cohort_name
```

Good:
```sql
SELECT
    cohort_name,
    count(*)   AS total_students,
    avg(score) AS average_score
FROM grades
GROUP BY cohort_name
```

#### Source breakdown

 - **For (4)**: Holywell, Celko, Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (5)**: GitLab, Mozilla, Supabase, PG.ai, PostgreSQL.

## Aim for clarity in table aliasing

Prefer using the full table name over an alias when the name is short enough to be readable. When aliasing is unavoidable (long or ambiguous names, self-joins), use a meaningful alias rather than an abbreviation or the initials of the table words (`schools`, not `s`; `districts`, not `sda`).

In a long query, an abbreviation forces the reader to scroll back to the `FROM` or `JOIN` clause to remember which table it stands for. A meaningful name identifies the table anywhere in the query, and makes the business logic easier to follow for developers new to the codebase.

Never use generic aliases that convey no meaning, such as `x` or `t1`.

##### Example

Bad:
```sql
SELECT
    s.name    AS school_name,
    sda.name  AS district_name,
    s.address AS school_address
FROM schools AS s
INNER JOIN school_district_areas AS sda
    ON s.district_code = sda.code
```

Good:
```sql
SELECT
    schools.name    AS school_name,
    districts.name  AS district_name,
    schools.address AS school_address
FROM schools
INNER JOIN school_district_areas AS districts
    ON schools.district_code = districts.code
```

#### Source breakdown

 - **For (3)**: Brooklyn, Mazur, GitLab.
 - **Against (2)**: Holywell, PostgreSQL.
 - **Not mentioned (4)**: Celko, Mozilla, Supabase, PG.ai.


## Column aliases should semantically relate to what they represent

Aliases should relate in some way to the object or expression they are aliasing. If they are computed values, they should reflect the transformation applied, and should reference the source that is being selected. Basically, a column alias should follow the same naming rules as declared column names in tables.

##### Example

Bad:
```sql
SELECT
    cohort_name,
    count(*)   AS total,
    avg(score) AS scores_1
FROM grades
GROUP BY cohort_name
```

Good:
```sql
SELECT
    cohort_name,
    count(*)   AS total_students,
    avg(score) AS average_score
FROM grades
GROUP BY cohort_name
```

#### Source breakdown

 - **For (7)**: Brooklyn, Mazur, GitLab, Supabase, Celko, Holywell, PG.ai.
 - **Against (0)**
 - **Not mentioned (2)**: Mozilla, PostgreSQL.

## Always qualify column references in queries with `JOIN`

In queries with multiple tables, always qualify every column reference with its table name or alias. This improves the clarity and readability of the query, as you are able to tell at a glance where each column comes from.

##### Example

Bad:
```sql
SELECT
    full_name,
    email,
    course_id,
    enrolled_at
FROM students
INNER JOIN enrolments
    ON students.id = enrolments.student_id
```

Good:
```sql
SELECT
    students.full_name,
    students.email,
    enrolments.course_id,
    enrolments.enrolled_at
FROM students
INNER JOIN enrolments
    ON students.id = enrolments.student_id
```

#### Source breakdown

 - **For (4)**: Brooklyn, Mazur, GitLab, Holywell.
 - **Against (0)**
 - **Not mentioned (5)**: Mozilla, PostgreSQL, Supabase, Celko, PG.ai.

## Omit table references for single-table queries

In single-table queries, omit the table prefix from column references as they do not provide any meaningful context while increasing the verbosity of the query.

##### Example

Bad:
```sql
SELECT
    schools.address,
    schools.urn AS school_urn
FROM schools
WHERE schools.cohort > 2024
```

Good:
```sql
SELECT
    address,
    urn AS school_urn
FROM schools
WHERE cohort > 2024
```

#### Source breakdown

 - **For (2)**: Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (7)**: Mozilla, PostgreSQL, Supabase, Celko, PG.ai, GitLab, Holywell.
