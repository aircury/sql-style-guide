# JOIN clauses

## *Proposal* Use `ON` for all join conditions; avoid `USING`

Always express join conditions with `ON`, even when the joining columns share the same name. Avoid `USING`, as it silently merges the columns into one in the result set, which can cause unexpected behaviour when adding aliases or when the query is refactored.

##### Example

Bad:
```sql
SELECT
    students.full_name,
    enrolments.course_id
FROM students
INNER JOIN enrolments USING (student_id)
```

Good:
```sql
SELECT
    students.full_name,
    enrolments.course_id
FROM students
INNER JOIN enrolments
    ON students.id = enrolments.student_id
```

#### Source breakdown

 - **For (2)**: Brooklyn, GitLab.
 - **Against  (1)**: PostgreSQL.
 - **Not mentioned (6)**: Holywell, Celko, PG.ai, Mozilla, Mazur, Supabase.

## Put the first-referenced table immediately after `ON`

In a join condition, write the table that was introduced first (the left-hand table) immediately after the `ON` keyword. This makes fan-out risk visible at a glance: if the left table's key is not on the left side of the condition, a row duplication bug may be hiding.

##### Example

Bad:
```sql
SELECT
    students.full_name,
    enrolments.course_id
FROM students
INNER JOIN enrolments
    ON enrolments.student_id = students.id
```

Good:
```sql
SELECT
    students.full_name,
    enrolments.course_id
FROM students
INNER JOIN enrolments
    ON students.id = enrolments.student_id
```

#### Source breakdown

 - **For (2)**: Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Supabase.

## For `INNER JOIN`, keep filter conditions in `WHERE`, not in the `JOIN` clause

The `ON` clause should express only the relationship between tables. Row-level filters belong in `WHERE`. Mixing business logic into `ON` obscures intent, as a reader scanning `ON` expects join conditions, not data filters.

As an important note, this rule applies only to `INNER JOIN`. With `LEFT JOIN`, moving a condition from `ON` to `WHERE` changes the semantics and effectively converts it into an inner join.

##### Example

Bad:
```sql
SELECT
    students.full_name,
    enrolments.course_id
FROM students
INNER JOIN enrolments
    ON students.id = enrolments.student_id
    AND students.is_active IS true
    AND enrolments.completed_at IS NOT NULL
```

Good:
```sql
SELECT
    students.full_name,
    enrolments.course_id
FROM students
INNER JOIN enrolments
    ON students.id = enrolments.student_id
WHERE 
    students.is_active IS true
    AND enrolments.completed_at IS NOT NULL
```

#### Source breakdown

 - **For (1)**: Brooklyn.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Mazur, Supabase.
