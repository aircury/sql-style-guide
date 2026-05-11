# Data types

## Use `numeric` or `decimal` for monetary and precision values

When storing monetary amounts or any value where exact precision matters, use `numeric` or `decimal`. Never use `real` or `float` for these — they use binary floating-point arithmetic and can introduce rounding errors. Only reach for `real` or `float` when the computation genuinely requires floating-point math (e.g. scientific calculations).

##### Example

Bad:
```sql
CREATE TABLE school_fees (
    student_id   integer      NOT NULL,
    amount       float        NOT NULL,
    discount     real         NOT NULL,
    paid_at      timestamp    NOT NULL
);
```

Good:
```sql
CREATE TABLE school_fees (
    student_id   integer        NOT NULL,
    amount       numeric(10, 2) NOT NULL,
    discount     decimal(5, 2)  NOT NULL,
    paid_at      timestamp      NOT NULL
);
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Prefer `boolean` for logical values — use `true`/`false`, not `1`/`0`

Use the `boolean` type for columns that represent a true/false state. Store and compare values using the literals `true` and `false` rather than integer substitutes `1` and `0`. This makes intent explicit and avoids ambiguity when reading queries.

##### Example

Bad:
```sql
SELECT
    full_name,
    is_active
FROM students
WHERE 
    is_active = 1
    AND has_scholarship = 0
```

Good:
```sql
SELECT
    full_name,
    is_active
FROM students
WHERE 
    is_active IS true
    AND has_scholarship IS false
```

#### Source breakdown

 - **For (1)**: GitLab.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Brooklyn, Mazur, Supabase.

## Default values should match the column type

A column's default value must be the same type as the column. 

##### Example

Bad:
```sql
CREATE TABLE enrolments (
    student_id  integer   NOT NULL,
    enrolled_at timestamp NOT NULL,
    is_active   boolean   NOT NULL DEFAULT 1,
    score       numeric   NOT NULL DEFAULT '0'
);
```

Good:
```sql
CREATE TABLE enrolments (
    student_id  integer   NOT NULL,
    enrolled_at timestamp NOT NULL,
    is_active   boolean   NOT NULL DEFAULT true,
    score       numeric   NOT NULL DEFAULT 0
);
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Use ISO 8601 format for date/time values and ISO temporal syntax in queries

Store dates and times using ISO 8601 format: `YYYY-MM-DDTHH:MM:SS.SSSSS`. Use ISO temporal syntax when expressing date/time literals in queries rather than vendor-specific formats or implicit casts.

##### Example

Bad:
```sql
SELECT *
FROM enrolments
WHERE 
    enrolled_at > '01/09/2024'
    AND completed_at < '31/07/2025 23:59:59'
```

Good:
```sql
SELECT *
FROM enrolments
WHERE 
    enrolled_at > DATE '2024-09-01'
    AND completed_at < TIMESTAMP '2025-07-31T23:59:59'
```

#### Source breakdown

 - **For (4)**: Holywell, Celko, PG.ai, Supabase.
 - **Against (0)**
 - **Not mentioned (5)**: PostgreSQL, Mozilla, Brooklyn, GitLab, Mazur.
