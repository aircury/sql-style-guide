# Formatting

## Left align all keywords, starting on the same character boundary

Align all clause keywords to the same left margin instead of right-aligning them to a common boundary (**river** alignment).

Left-aligned keywords are simpler to write and maintain. No padding arithmetic is needed, and adding a longer keyword (switching `JOIN` to `INNER JOIN`, for example) does not force re-alignment of everything else on the page. It also works without editor tooling, which makes it easier to enforce consistently across a team.

##### Example

Bad:
```sql
SELECT full_name,
       email,
       created_at
  FROM students
 WHERE school_urn = '02352'
```

Good:
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

## Use 4 spaces for indentation

Use **4 spaces** for each level of indentation. This is the most common choice among the sources consulted, providing enough visual separation to clearly distinguish nested levels without excessive horizontal growth.

##### Example

Bad:
```sql
SELECT
  full_name,
  email
FROM students
WHERE cohort > 2024
```

Good:
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

## Place `JOIN` conditions below the keyword

When joining tables, place the `ON` keyword on a new line, indented one level under the `JOIN` keyword, even when there is a single condition. This clearly separates the join condition from the join declaration and makes it easier to identify at a glance.

##### Example

Bad:
```sql
SELECT
    students.full_name,
    courses.name AS course_name
FROM students
INNER JOIN enrolments ON students.id = enrolments.student_id AND students.cohort = enrolments.cohort
INNER JOIN courses ON enrolments.course_id = courses.id
```

Good:
```sql
SELECT
    students.full_name,
    courses.name AS course_name
FROM students
INNER JOIN enrolments 
    ON students.id = enrolments.student_id
    AND students.cohort = enrolments.cohort
INNER JOIN courses 
    ON enrolments.course_id = courses.id
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

## Put `AND` / `OR` at the beginning of the next line

In a `WHERE`, `HAVING`, or `ON` clause with multiple conditions, place `AND` and `OR` at the **beginning** of the next line rather than at the end of the current one. The operators keep the same indentation as the first condition.

Placing boolean operators at the start of the line makes conditions easier to scan, as each condition begins with a clear logical marker, and the boundary between conditions is immediately visible. It also keeps diffs small: adding, removing or commenting out a condition touches only its own line.

##### Example

Bad:
```sql
SELECT *
FROM students
WHERE 
    cohort > 2024 AND
    is_active IS true AND
    school_urn IS NOT NULL
```

Good:
```sql
SELECT *
FROM students
WHERE 
    cohort > 2024
    AND is_active IS true
    AND school_urn IS NOT NULL
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

## Use trailing commas

In lists of arguments (columns in `SELECT`, values in `IN`, ...) place commas at the **end** of each line (trailing) rather than the **beginning** (leading).

Trailing commas are the more widely supported convention and feel natural to most developers. A missing comma is already flagged by most IDEs before the query is run.

##### Example

Bad:
```sql
SELECT
      full_name
    , email
    , cohort
FROM students
```

Good:
```sql
SELECT
    full_name,
    email,
    cohort
FROM students
```

#### Source breakdown

 - **For trailing commas (4)**: Celko, Mozilla, PG.ai, Mazur.
 - **For leading commas (1)**: Brooklyn.
 - **Not mentioned (4)**: Holywell, PostgreSQL, GitLab, Supabase.

## Keep lines under 120 characters

Lines should not exceed **120 characters**, to keep queries readable in side-by-side views, code reviews, and editors with limited width. The limit is deliberately generous: in queries with several levels of subqueries, indentation alone takes up a good part of the line. Treat it as a ceiling, not a target.

When a line goes over the limit, wrap the expression in parentheses. Break the line after the opening parenthesis, indent the content one level, and put the closing parenthesis on its own line, aligned with the line that opened it. Inside, break at logical points such as before an operator or after a comma, starting each line with the operator in the same way as `AND` / `OR`. The parentheses group the expression into one visible block, with a clear start and end.

Function calls already have their own parentheses, so break inside those instead (see [Terminate the line after an open parenthesis](#terminate-the-line-after-an-open-parenthesis)). If an expression is still hard to read after breaking it, move it into a CTE.

##### Example

Bad:
```sql
SELECT
    students.full_name,
    assessments.exam_score * 0.5 + assessments.coursework_score * 0.3 + assessments.attendance_score * 0.15 + assessments.participation_score * 0.05 AS final_grade
FROM students
INNER JOIN assessments
    ON students.id = assessments.student_id
```

Good:
```sql
SELECT
    students.full_name,
    (
        assessments.exam_score * 0.5
        + assessments.coursework_score * 0.3
        + assessments.attendance_score * 0.15
        + assessments.participation_score * 0.05
    ) AS final_grade
FROM students
INNER JOIN assessments
    ON students.id = assessments.student_id
```

#### Source breakdown

 - **For (2)**: Brooklyn, GitLab.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Mazur, Supabase.
