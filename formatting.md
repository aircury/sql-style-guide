# Formatting

For all this rules, be mindful that if the **river** alignment described below if finally chosen, the rest of the rules of this section will need to be changed, as they only make sense when using left-aligned keywords.

## *Proposal* Left align all keywords, starting on the same character boundary

There are two major positions on keyword alignment: **river** and **left-aligned**. Throughout the guide, all the examples have been using the latter.

Arguments in favor of aligning keywords to the left are that they all start at the same left margin, which is simpler to write and maintain. No padding arithmetic is needed, and adding a longer keyword (switching `JOIN` to `INNER JOIN`, for example) does not force re-alignment of everything else on the page. It also works without editor tooling, which makes it easier to enforce consistently across a team.

The river argument is as follows: Right-aligning keywords to a common boundary creates a visual separation between structure and data, as the eye can scan keywords in one vertical sweep down the left channel, and implementation detail sits cleanly to the right. Arguments naturally fall into alignment under each other without extra rules.

##### Example

River:
```sql
SELECT full_name,
       email,
       created_at
  FROM students
 WHERE school_urn = '02352'
```

Left-align:
```sql
SELECT 
    full_name,
    email,
    created_at
FROM students
WHERE school_urn = '02352'
```

#### Source breakdown

 - **For river (2)**: Celko, Holywell.
 - **For left align (5)**: PG.ai, GitLab, Mozilla, Brooklyn, Mazur.
 - **No mention (2)**: PostgreSQL, Supabase.

## Each major clause on its own line

Each major SQL clause (`SELECT`, `FROM`, `WHERE`, `GROUP BY`, `HAVING`, `ORDER BY`, etc.) must start on its own line. This separates the logical structure of the query from its arguments and makes the query easier to scan, modify, and review.

##### Example

Bad:
```sql
SELECT full_name, email FROM students WHERE cohort > 2024 GROUP BY full_name, email
```

Good:
```sql
SELECT
    full_name,
    email
FROM students
WHERE cohort > 2024
GROUP BY
    full_name,
    email
```

#### Source breakdown

 - **For (7)**: Holywell, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur.
 - **Against (0)**
 - **Not mentioned (2)**: Celko, Supabase.

## *Proposal* Place arguments below the clause arguments

If a clause has only one argument, it may share the line with the root keyword. If it has multiple arguments, each must be on its own line, including the first. 

##### Example

Bad:
```sql
SELECT full_name, email, cohort
FROM students
WHERE cohort > 2024
```

Good:
```sql
SELECT
    full_name,
    email,
    cohort
FROM students
WHERE cohort > 2024
```

Good (single argument sharing the line):
```sql
SELECT count(*) AS total_students
FROM students
WHERE cohort > 2024
```

#### Source breakdown

Regarding a single argument sharing the line with the root keyword:

 - **For (3)**: Mozilla, PG.ai, Brooklyn.
 - **Against (0)**
 - **Not mentioned (6)**: Holywell, Celko, PostgreSQL, GitLab, Mazur, Supabase.

Regarding multiple arguments each on their own line, including the first:
 - **For (4)**: Mozilla, PG.ai, Brooklyn, Mazur.
 - **Against (2)**: Holywell, Celko (the river pattern places the first argument on the same line as the keyword).
 - **Not mentioned (3)**: PostgreSQL, GitLab, Supabase.

## *Proposal* Use 4 spaces for indentation

Use **4 spaces** for each level of indentation. This is the most common choice among the sources consulted, providing enough visual separation to clearly distinguish nested levels without excessive horizontal growth. There is, however, an argument to use **2 spaces** instead, as it gives enough visual separation while keeping the query less horizontal when complex operations are needed.

##### Example

4 spaces:
```sql
SELECT
    full_name,
    email
FROM students
WHERE cohort > 2024
```

2 spaces:
```sql
SELECT
  full_name,
  email
FROM students
WHERE cohort > 2024
```

#### Source breakdown

 - **For 4 spaces (2)**: Brooklyn, Holywell.
 - **For 2 spaces (1)**: GitLab.
 - **Not mentioned (6)**: Celko, PostgreSQL, PG.ai, Mozilla, Mazur, Supabase.

## *Details to discuss* Place `JOIN` conditions below the keyword

When joining tables, place the `ON` / `USING` keyword on a new line, indented one level under the `JOIN` keyword. This clearly separates the join condition from the join declaration and makes it easier to identify at a glance.

An alternative, adopted by Brooklyn and Mazur, is to place a single join condition directly on the same line as the `JOIN` keyword, and to only move to separate lines when there are multiple conditions.

##### Example

Bad:
```sql
SELECT
    students.full_name,
    courses.course_name
FROM students
INNER JOIN enrolments ON students.id = enrolments.student_id AND students.cohort = enrolments.cohort
INNER JOIN courses ON enrolments.course_id = courses.id
```

Good:
```sql
SELECT
    students.full_name,
    courses.course_name
FROM students
INNER JOIN enrolments 
    ON students.id = enrolments.student_id
    AND students.cohort = enrolments.cohort
INNER JOIN courses 
    ON enrolments.course_id = courses.id
```

Good (single condition):
```sql
SELECT
    students.full_name,
    courses.course_name
FROM students
INNER JOIN enrolments 
    ON students.id = enrolments.student_id
    AND students.cohort = enrolments.cohort
INNER JOIN courses ON enrolments.course_id = courses.id
```

#### Source breakdown

 - **For indented `ON` (1)**: Mozilla.
 - **For inline single / indented multiple (2)**: Brooklyn, Mazur.
 - **Not mentioned (6)**: Holywell, Celko, PostgreSQL, PG.ai, GitLab, Supabase.

## Terminate the line after an open parenthesis

When a parenthesised construct spans multiple lines, the opening parenthesis should terminate the line it starts on. The contents should be indented one level, and the closing parenthesis should sit on its own line, aligned with the opening construct.

##### Example

Bad:
```sql
WITH active_students AS (SELECT
    id,
    full_name
    FROM students
    WHERE is_active IS true)
SELECT *
FROM active_students
```

Good:
```sql
WITH active_students AS (
    SELECT
        id,
        full_name
    FROM students
    WHERE is_active IS true
)
SELECT *
FROM active_students
```

#### Source breakdown

 - **For (3)**: Holywell, PG.ai, Mozilla.
 - **Against (0)**
 - **Not mentioned (6)**: Celko, PostgreSQL, Brooklyn, GitLab, Mazur, Supabase.

## Indent CTEs under `WITH`

Each CTE name and its body should be indented one level under the `WITH` keyword. Leave a blank line between each CTE for visual separation.

##### Example

Bad:
```sql
WITH
active_students AS (
SELECT id, full_name FROM students WHERE is_active IS true
),
recent_enrolments AS (
SELECT student_id, course_id FROM enrolments WHERE created_at > '2024-01-01'
)
SELECT *
FROM active_students
INNER JOIN recent_enrolments
    ON active_students.id = recent_enrolments.student_id
```

Good:
```sql
WITH active_students AS (
    SELECT
        id,
        full_name
    FROM students
    WHERE is_active IS true
),
recent_enrolments AS (
    SELECT
        student_id,
        course_id
    FROM enrolments
    WHERE created_at > '2024-01-01'
)
SELECT *
FROM active_students
INNER JOIN recent_enrolments
    ON active_students.id = recent_enrolments.student_id
```

#### Source breakdown

 - **For (2)**: PostgreSQL, Brooklyn.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PG.ai, Mozilla, GitLab, Mazur, Supabase.

## Use one space between all language tokens

Always use exactly one space between language tokens: before and after operators (`=`, `>`, `<`, `!=`, ...) and after commas. Never crowd tokens together or add extra padding spaces to align values across lines.

##### Example

Bad:
```sql
SELECT
    full_name,
    cohort+1   AS next_cohort,
    score*0.1  AS weighted_score
FROM students
WHERE 
    cohort>2024
    AND is_active=true
```

Good:
```sql
SELECT
    full_name,
    cohort + 1 AS next_cohort,
    score * 0.1 AS weighted_score
FROM students
WHERE 
    cohort > 2024
    AND is_active IS true
```

#### Source breakdown

 - **For (3)**: Holywell, Celko, PostgreSQL.
 - **Against (0)**
 - **Not mentioned (6)**: PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Don't add extra spaces inside parentheses or before function parentheses

Do not pad the inside of parentheses with spaces. Do not add a space between a function name and its opening parenthesis.

##### Example

Bad:
```sql
SELECT
    count( * )           AS total_students,
    coalesce( score, 0 ) AS score
FROM students
WHERE cohort IN ( 2023, 2024 )
```

Good:
```sql
SELECT
    count(*)           AS total_students,
    coalesce(score, 0) AS score
FROM students
WHERE cohort IN (2023, 2024)
```

#### Source breakdown

 - **For (3)**: Brooklyn, Mazur, PostgreSQL.
 - **Against (0)**
 - **Not mentioned (6)**: Holywell, Celko, PG.ai, Mozilla, GitLab, Supabase.

## *Proposal*  Put `AND` / `OR` at the beginning of the next line

In a `WHERE`, `HAVING`, or `ON` clause with multiple conditions, place `AND` and `OR` at the **beginning** of the next line rather than at the end of the current one.

Placing boolean operators at the start of the line makes conditions easier to scan, as each condition begins with a clear logical marker, and the boundary between conditions is immediately visible.

##### Example

Beginning of next line:
```sql
SELECT *
FROM students
WHERE 
    cohort > 2024
    AND is_active IS true
    AND school_urn IS NOT NULL
```

End of line:
```sql
SELECT *
FROM students
WHERE 
    cohort > 2024 AND
    is_active IS true AND
    school_urn IS NOT NULL
```

#### Source breakdown

 - **For (3)**: PG.ai, Mozilla, Brooklyn.
 - **Against (2)**: Celko, Mazur.
 - **Not mentioned (4)**: Holywell, PostgreSQL, GitLab, Supabase.

## Add new lines after semicolons and keyword definitions

Leave a blank line after each semicolon to separate queries. Also leave a new line after each keyword definition to clearly demarcate logical blocks within a script.

##### Example

Bad:
```sql
SELECT count(*) AS total FROM students; SELECT count(*) AS total FROM courses;
```

Good:
```sql
SELECT count(*) AS total
FROM students;

SELECT count(*) AS total
FROM courses;
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Use blank lines between logically distinct sections

Leave a blank line between logically distinct sections of code, such as between CTEs, or between groups of related conditions in a long `WHERE` clause. This follows general coding best practices.

##### Example

Bad:
```sql
WITH active_students AS (
    SELECT id, full_name
    FROM students
    WHERE is_active IS true
)
SELECT
    active_students.full_name,
    count(enrolments.course_id) AS total_courses
FROM active_students
INNER JOIN enrolments 
    ON active_students.id = enrolments.student_id
GROUP BY 
    active_students.full_name
```

Good:
```sql
WITH active_students AS (
    SELECT
        id,
        full_name
    FROM students
    WHERE is_active IS true
)

SELECT
    active_students.full_name,
    count(enrolments.course_id) AS total_courses
FROM active_students
INNER JOIN enrolments
    ON active_students.id = enrolments.student_id
GROUP BY 
    active_students.full_name
```

#### Source breakdown

 - **For (4)**: Holywell, Celko, Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (5)**: PostgreSQL, PG.ai, Mozilla, GitLab, Supabase.

## *Proposal* Use trailing commas

In lists of arguments (columns in `SELECT`, values in `IN`, ...) place commas at the **end** of each line (trailing) rather than the **beginning** (leading).

Trailing commas are the more widely supported convention and feel natural to most developers. Leading commas, on the other hand, make it easy to spot a missing comma since it is the first character visible on each line.

##### Example

Trailing commas:
```sql
SELECT
    full_name,
    email,
    cohort
FROM students
```

Leading commas:
```sql
SELECT
      full_name
    , email
    , cohort
FROM students
```

#### Source breakdown

 - **For trailing commas (4)**: Celko, Mozilla, PG.ai, Mazur.
 - **For leading commas (1)**: Brooklyn.
 - **Not mentioned (4)**: Holywell, PostgreSQL, GitLab, Supabase.

## *Details to discuss* Set a line length limit

Lines should not exceed a set character limit to improve readability in side-by-side views, code reviews, and editors with limited width. There is no consensus on the exact threshold, but it ranges between 80 and 120 characters.

#### Source breakdown

 - **For (2)**: Brooklyn, GitLab.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Mazur, Supabase.
