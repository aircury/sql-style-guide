# Subqueries and CTEs

## Prefer CTEs over nested subqueries

When a query requires intermediate results, use a CTE (`WITH`) rather than a nested subquery. CTEs give each step a name, making the query easier to read, test, and debug.

##### Example

Bad:
```sql
SELECT
    full_name,
    total_courses
FROM (
    SELECT
        students.full_name,
        count(enrolments.course_id) AS total_courses
    FROM students
    INNER JOIN enrolments
        ON students.id = enrolments.student_id
    WHERE students.is_active IS true
    GROUP BY students.full_name
) AS student_courses
WHERE total_courses > 3
```

Good:
```sql
WITH student_courses AS (
    SELECT
        students.full_name,
        count(enrolments.course_id) AS total_courses
    FROM students
    INNER JOIN enrolments
        ON students.id = enrolments.student_id
    WHERE students.is_active IS true
    GROUP BY students.full_name
)

SELECT
    full_name,
    total_courses
FROM student_courses
WHERE total_courses > 3
```

#### Source breakdown

 - **For (7)**: Celko, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.
 - **Against (0)**
 - **Not mentioned (2)**: Holywell, PostgreSQL.

## Avoid correlated subqueries

A correlated subquery references a column from the outer query, causing it to execute once per row. This leads to poor performance and is almost always replaceable with a `JOIN` or a CTE.

##### Example

Bad:
```sql
SELECT
    full_name,
    (
        SELECT count(*)
        FROM enrolments
        WHERE enrolments.student_id = students.id
    ) AS total_enrolments
FROM students
```

Good:
```sql
WITH enrolment_counts AS (
    SELECT
        student_id,
        count(*) AS total_enrolments
    FROM enrolments
    GROUP BY student_id
)

SELECT
    students.full_name,
    enrolment_counts.total_enrolments
FROM students
INNER JOIN enrolment_counts
    ON students.id = enrolment_counts.student_id
```

#### Source breakdown

 - **For (2)**: Celko, PG.ai.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, PostgreSQL, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Each CTE should do one logical unit of work

A CTE should encapsulate a single, well-defined step in the query's logic. Avoid cramming multiple transformations into one CTE. A query made of several simple CTEs is easier to understand and debug than one made of fewer complex ones.

##### Example

Bad:
```sql
WITH student_data AS (
    SELECT
        students.full_name,
        count(enrolments.course_id) AS total_courses,
        avg(assessments.score)      AS avg_score,
        max(assessments.score)      AS top_score
    FROM students
    INNER JOIN enrolments 
        ON students.id = enrolments.student_id
    INNER JOIN assessments 
        ON enrolments.id = assessments.enrolment_id
    WHERE students.is_active IS true
    GROUP BY students.full_name
)

SELECT * 
FROM student_data
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

student_enrolments AS (
    SELECT
        active_students.full_name,
        enrolments.course_id,
        enrolments.id AS enrolment_id
    FROM active_students
    INNER JOIN enrolments
        ON active_students.id = enrolments.student_id
),

student_scores AS (
    SELECT
        student_enrolments.full_name,
        avg(assessments.score) AS avg_score,
        max(assessments.score) AS top_score
    FROM student_enrolments
    INNER JOIN assessments
        ON student_enrolments.enrolment_id = assessments.enrolment_id
    GROUP BY student_enrolments.full_name
)

SELECT * 
FROM student_scores
```

#### Source breakdown

 - **For (3)**: Brooklyn, GitLab, Supabase.
 - **Against (0)**
 - **Not mentioned (6)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Mazur.

## *Details to discuss* CTE names should be descriptive

CTE names should clearly convey what the CTE produces. Prefer names that read like a noun phrase describing the result set  (like `active_students`) rather than abbreviations or vague labels like `data` or `tmp`.

While some sources argue for names that are as verbose as needed to be unambiguous, other argues for conciseness: names should be clear but not padded with unnecessary words. Both agree that clarity is the goal: they differ on where to draw the line between informative and verbose.

##### Example

Bad:
```sql
WITH d AS (
...
),

tmp AS (
...
),

final AS (
...
)
```

Good:
```sql
WITH active_students AS (
...
),

completed_enrolments AS (
...
),

student_pass_rates AS (
...
)
```

#### Source breakdown

 - **For descriptive (1)**: Brooklyn.
 - **For concise but clear (1)**: GitLab.
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Mazur, Supabase.

## Do not prefix or suffix CTE names with `cte`

Avoid names like `cte_active_students` or `active_students_cte`. The `WITH` keyword already makes the CTE context clear, so the redundant label adds noise without adding information.

##### Example

Bad:
```sql
WITH cte_active_students AS (
    SELECT 
        id, 
        full_name
    FROM students
    WHERE is_active IS true
)

SELECT * 
FROM cte_active_students
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

 - **For (1)**: Brooklyn.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Mazur, Supabase.
