# Naming rules

These rules apply mostly to names chosen by developers, and may not apply when using replicated data from a source that does not adhere to the rules. In those cases, it is better to keep the original structure for clarity reasons (for example, not renaming columns to use `snake_case` in tables that are replicas of endpoints of APIs that use `camelCase`).

## General naming rules

### Use snake case

Always use snake case (lowercase with underscores, like `snake_case`) for naming.

##### Example

Bad:
```sql
SELECT
    firstName,
    last_name,
    Cohort
FROM students
```

Good:
```sql
SELECT
    first_name,
    last_name,
    cohort
FROM students
```

#### Source breakdown

 - **For (9)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.
 - **Against (0)**
 - **Not mentioned (0)**

### *Details to discuss* Names must be descriptive, consistent and unambiguous

Use descriptive names and consistent across tables (for example, `cohort` should mean the same and have the same data type everywhere). Do not use descriptive prefixes that explain the role or type of the identifier (`tbl_`, `sp_`, `fk_`, ...).

Besides, avoid abbreviations unless they are commonly understood (for example, only use `urn` if the whole team understands that it means "unique reference number").

Vague identifiers is the only thing not all guides are in favor of removing, and it refers to names like `id`, `type` or `name` without a prefix to state what they identify. An argument can be made that most of the times those identifiers implicitly state what they identify by virtue of the table they reside in (if `users` has a column `id`, it is clearly the unique identifier of an user). This is the part of the rule to be discussed.

##### Example

Bad:
```sql
SELECT
    e.pid,
    e.created_at,
    c.date_creation,
    c.cohort
FROM enrolments AS e
INNER JOIN tbl_courses AS c
    ON e.course_id = c.id	
```

Good:
```sql
SELECT
    e.participant_id,
    e.created_at AS enrolment_created_at,
    c.created_at AS course_created_at,
    c.cohort
FROM enrolments AS e
INNER JOIN courses AS c
    ON e.course_id = c.id	
```

#### Source breakdown

Regarding descriptive names:
 - **For (4)**: Holywell, Celko, Mozilla, Supabase.
 - **Against (0)**
 - **Not mentioned (5)**: PostgreSQL, PG.ai, Brooklyn, GitLab, Mazur.

Regarding consistent naming across tables:
 - **For (1)**: Celko.
 - **Against (0)**
 - **Not mentioned (8)**: PostgreSQL, PG.ai, Brooklyn, GitLab, Mazur, Mozilla, Supabase, Holywell.

Regarding not using descriptive prefixes:
 - **For (3)**: Holywell, Celko, Supabase.
 - **Against (1)**: PG.ai.
 - **Not mentioned (5)**: PostgreSQL, Brooklyn, Mazur, Mozilla, GitLab.
 
Regarding avoiding abbreviations:
 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Brooklyn, GitLab, Mazur, Mozilla, Supabase.

Regarding no vague identifiers:
 - **For (3)**: Holywell, Celko, GitLab.
 - **Against (1)**: PG.ai.
 - **Not mentioned (5)**: PostgreSQL, Brooklyn, Mazur, Mozilla, Supabase.

### Use unquoted alphanumeric names

Use only lowercase alphanumeric characters for names separated by underscores, beginning always with a letter. For readability reasons, they should never end in an underscore or contain multiple consecutive underscores. Always avoid quoted identifiers for names, as they are prone to cause query errors (many database providers are case insensitive, so forgetting the quotes leads to incorrect column assignations).

##### Example

Bad:
```sql
CREATE TABLE "Enrolments" (
    1st_grade     numeric(4,2),
    student__name varchar,
    course_id_    integer,
    "Is Active"   boolean,
    createdat     timestamp
)
```

Good:
```sql
CREATE TABLE enrolments (
    final_grade  numeric(4,2),
    student_name varchar,
    course_id    integer,
    is_active    boolean,
    created_at   timestamp
)
```

#### Source breakdown

Regarding the use of only alphanumeric characters, starting with a letter:
 - **For (4)**: Holywell, Celko, PG.ai, Mozilla.
 - **Against (0)**
 - **Not mentioned (5)**: PostgreSQL, Brooklyn, GitLab, Mazur, Supabase.

Regarding no consecutive underscores:
 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Brooklyn, GitLab, Mazur, Mozilla, Supabase.

Regarding quoted identifiers:
 - **For (3)**: Holywell, Celko, GitLab.
 - **Against (0)**
 - **Not mentioned (6)**: PostgreSQL, PG.ai, Brooklyn, Mazur, Mozilla, Supabase.

### Do not use reserved words as identifiers

Never use words such as `sum`, `count`, ... It will have to be quoted everywhere it is used or it will fail.

##### Example

Bad:
```sql
CREATE TABLE students (
    id         integer PRIMARY KEY,
    first_name varchar,
    last_name  varchar,
    group      varchar
)
...
CREATE TABLE union (...)
```

Good:
```sql
CREATE TABLE students (
    id          integer PRIMARY KEY,
    first_name  varchar,
    last_name   varchar,
    cohort_name varchar
)
...
CREATE TABLE merges (...)
```

#### Source breakdown

 - **For (3)**: Brooklyn, GitLab, Supabase.
 - **Against (0)**
 - **Not mentioned (6)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Mazur.

## *Proposal* Use collective, class or plural nouns for tables

The core reasoning behind this rule is that tables hold many rows, and each row represents one entity of something, so the table name should reflect its plural nature. Use collective nouns if possible (for example, `staff` over `employees`). Otherwise, use plural nouns.

The only exception to this rule would be for "singleton" tables: tables that will only contain and will contain one row. Those should be in singular to reflect their nature.

##### Example

Bad:
```sql
CREATE TABLE acrobats (...);
CREATE TABLE course (...);
```

Good:
```sql
CREATE TABLE troupe (...);
CREATE TABLE courses (...);
```

#### Source breakdown

 - **For (4)**: Holywell, Celko, Mazur, Supabase.
 - **Against (1)**: PG.ai.
 - **Not mentioned (4)**: Brooklyn, GitLab, PostgreSQL, Mozilla.

## Avoid concatenating names for relationship tables

Avoid using concatenated names for relationship tables, preferring a common descriptive name (`syllabus` instead of `course_materials`). If there is no common term, the concatenation will probably be the better fit.

##### Example

Bad:
```sql
CREATE TABLE student_courses (...);
CREATE TABLE student_dormitories (...);
```

Good:
```sql
CREATE TABLE enrolments (...);
CREATE TABLE residences (...);
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: Brooklyn, GitLab, PostgreSQL, Mozilla, Mazur, Supabase, PG.ai.

## Column naming rules

### Use singular names for columns

A column holds one value per row, so using singular reflects that. An exception can be made for `array` or `json` columns, but those should be avoided where possible.

##### Example

Bad:
```sql
CREATE TABLE students (
    id        integer PRIMARY KEY, 
    full_name varchar,
    absences  integer
)
```

Good:
```sql
CREATE TABLE students (
    id            integer PRIMARY KEY, 
    full_name     varchar,
    absence_count integer
)
```

#### Source breakdown

 - **For (4)**: Holywell, Celko, Supabase, PostgreSQL.
 - **Against (0)**
 - **Not mentioned (5)**: Brooklyn, GitLab, Mozilla, Mazur, PG.ai.

### *Details to discuss* Use standardised prefixes and suffixes

Use standardised prefixes and suffixes to convey meaning, making them understandable without the need to inspect their values and data types. 

A non comprehensive list of some standard prefixes and suffixes is as follows:

| Prefix/Suffix | Meaning |
| ------------- | ------- |
| `_id`         | An unique identifier (some key) |
| `_status`     | A flag or status |
| `_total`, `total_` | Total or sum of values |
| `_name`       | A name |
| `_size`       | A size |
| `_date`       | A date (for example, `2026-04-14`) |
| `_at`         | A date and time (for example, `2026-04-14 00:00:00`) |
| `_addr`, `_address` | An address, physical or intangible |
| `is_`, `has_` | A boolean flag |
| `_eur`, `_ms` | Numeric unit of the data (seconds, litres, ...) |
| `_month`, `_day` | Specific part of a date |

Where sources differ and should be discussed is in the use of `_id`. Some state that tables should never contain bare `id` columns, as they are vague and make the naming inconsistent (`courses.id` as the primary key and `xxxx.course_id` as the foreign key). Others state that if a key is a single column, it should be named `id`.

##### Example

Bad:
```sql
SELECT
    treatment,
    last_treatment,
    name
FROM patients
```

Good:
```sql
SELECT
    has_treatment,
    last_treatment_date,
    name
FROM patients
```

#### Source breakdown

Regarding general standardised suffixes and prefixes:
 - **For (5)**: Holywell, Celko, Brooklyn, GitLab, Mazur.
 - **Against (0)**
 - **Not mentioned (4)**: PostgreSQL, Mozilla, PG.ai, Supabase.

For avoiding bare `id` columns:
 - **For (2)**: Holywell, Celko.
 - **Against (2)**: Brooklyn, Supabase.
 - **Not mentioned (5)**: PostgreSQL, Mozilla, PG.ai, GitLab, Mazur.

### Do not use the table name as a column name or prefix

Do not duplicate the table name in column name. This means two different things:
1. Do not use the table name as a prefix for a column (no `courses.course_name`).
2. Do not name a column identically to its table. A complete identical should never happen per previous rules, as tables should be collective/plural and columns singular, but things like `grades.grade` should be avoided.

##### Example

Bad:
```sql
CREATE TABLE grades (
    id           integer PRIMARY KEY,
    grade        numeric(3,1),
    grade_letter varchar(1)
)
```

Good:
```sql
CREATE TABLE grades (
    id     integer PRIMARY KEY,
    score  numeric(3,1),
    letter varchar(1)
)
```

#### Source breakdown

 - **For (3)**: Holywell, Celko, Supabase.
 - **Against (0)**
 - **Not mentioned (6)**: Brooklyn, GitLab, Mozilla, Mazur, PG.ai, PostgreSQL.
