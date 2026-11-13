# Comments

## Use `/* */` for comments and `--` for commented-out code

Use `/* */` for every written comment, whether it spans one line or several. Use `--` only to comment out lines of code, prefixing each line.

This keeps commented-out code visible in reviews. When a block of code is wrapped in `/* */`, a diff only shows the opening and closing markers, so it is easy to miss that the lines in between are disabled. With `--` on every line, each disabled line is marked.

Avoid committing commented-out code at all: delete it and rely on version control. Use `--` only when a line has to stay disabled for a reason.

##### Example

Bad:
```sql
-- Returns all active students enrolled in at least one course
-- during the current academic year.
SELECT
    students.full_name,
    count(enrolments.course_id) AS total_courses
FROM students
-- Current enrolments
INNER JOIN enrolments 
    ON students.id = enrolments.student_id
WHERE 
    students.is_active IS true
/*
    AND students.cohort > 2024
    AND students.school_urn IS NOT NULL
*/
```

Good:
```sql
/*
 * Returns all active students enrolled in at least one course
 * during the current academic year.
 */
SELECT
    students.full_name,
    count(enrolments.course_id) AS total_courses
FROM students
/* Current enrolments */
INNER JOIN enrolments 
    ON students.id = enrolments.student_id
WHERE 
    students.is_active IS true
    -- AND students.cohort > 2024
    -- AND students.school_urn IS NOT NULL
```

#### Source breakdown

 - **For `/* */` multi-line + `--` single-line (6)**: Holywell, Celko, PostgreSQL, PG.ai, GitLab, Supabase.
 - **For `/* */` exclusively (1)**: Brooklyn.
 - **Not mentioned (2)**: Mozilla, Mazur.

## Comment only where necessary

Add comments where the intent or logic is not self-evident. Do not comment every line or state what the code already makes obvious. Equally, do not leave complex or surprising logic without explanation.

##### Example

Bad (over-commented):
```sql
SELECT
    full_name, -- the student's full name
    cohort     -- the cohort year
FROM students  -- from the students table
WHERE 
    is_active IS true -- only active students
```

Bad (under-commented):
```sql
SELECT
    full_name,
    cohort,
    score * 0.6 + attendance_score * 0.4 AS final_grade
FROM students
WHERE 
    cohort = 2025
```

Good:
```sql
SELECT
    full_name,
    cohort,
    /* Weighted final grade: 60% assessment score, 40% attendance */
    score * 0.6 + attendance_score * 0.4 AS final_grade
FROM students
WHERE 
    cohort = 2025
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Comment CTEs with confusing or notable logic

When a CTE contains logic that is not immediately obvious, such as a complex filter, a business rule, or a non-trivial join strategy, add a comment above or inside it explaining what it does and why.

##### Example

Bad:
```sql
WITH eligible_students AS (
    SELECT student_id
    FROM enrolments
    WHERE 
        completed_at IS NOT NULL
        AND grade >= 50
        AND course_id NOT IN (
            SELECT id 
            FROM courses 
            WHERE is_pilot IS true
        )
)
SELECT *
FROM eligible_students
```

Good:
```sql
/*
 * Students who completed a non-pilot course with a passing grade (≥50).
 * Pilot courses are excluded as they follow a different grading scheme.
 */
WITH eligible_students AS (
    SELECT student_id
    FROM enrolments
    WHERE 
        completed_at IS NOT NULL
        AND grade >= 50
        AND course_id NOT IN (
            SELECT id 
            FROM courses 
            WHERE is_pilot IS true
        )
)
SELECT *
FROM eligible_students
```

#### Source breakdown

 - **For (3)**: Brooklyn, GitLab, Supabase.
 - **Against (0)**
 - **Not mentioned (6)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Mazur.
