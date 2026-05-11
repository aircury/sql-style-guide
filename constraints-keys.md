# Constraints and keys

## Every table must have at least one key

Every table must define at least one key to uniquely identify each row. A table without a key has no reliable way to reference, update, or deduplicate its rows.

##### Example

Bad:
```sql
CREATE TABLE enrolments (
    student_id   integer   NOT NULL,
    course_id    integer   NOT NULL,
    enrolled_at  timestamp NOT NULL
);
```

Good:
```sql
CREATE TABLE enrolments (
    student_id   integer   NOT NULL,
    course_id    integer   NOT NULL,
    enrolled_at  timestamp NOT NULL,

    PRIMARY KEY (student_id, course_id)
);
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Give constraints explicit custom names

Name constraints explicitly so that error messages and schema introspection are meaningful. The exceptions are `UNIQUE`, `PRIMARY KEY`, and `FOREIGN KEY` — these can rely on database-generated names.

##### Example

Bad:
```sql
CREATE TABLE students (
    id         integer NOT NULL,
    email      text    NOT NULL,
    cohort     integer NOT NULL,

    PRIMARY KEY (id),
    CHECK (cohort > 2000)
);
```

Good:
```sql
CREATE TABLE students (
    id         integer NOT NULL,
    email      text    NOT NULL,
    cohort     integer NOT NULL,

    PRIMARY KEY (id),
    CONSTRAINT chk_students_cohort_min CHECK (cohort > 2000)
);
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Place multi-column constraints near both columns; table-level constraints at the end

Multi-column constraints should be positioned as close as possible to all the columns they reference. Table-level constraints (those that span multiple columns or apply to the table as a whole) go at the end of the `CREATE TABLE` definition, after all column declarations.

##### Example

Bad:
```sql
CREATE TABLE enrolments (
    PRIMARY KEY (student_id, course_id),
    student_id  integer   NOT NULL,
    course_id   integer   NOT NULL,
    enrolled_at timestamp NOT NULL,
    FOREIGN KEY (student_id) REFERENCES students (id)
);
```

Good:
```sql
CREATE TABLE enrolments (
    student_id  integer   NOT NULL,
    course_id   integer   NOT NULL,
    enrolled_at timestamp NOT NULL,

    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES students (id)
);
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Declare `PRIMARY KEY` first, immediately after the column list

The `PRIMARY KEY` constraint should be the first constraint declared in a `CREATE TABLE` statement, placed right after the last column definition.

##### Example

Bad:
```sql
CREATE TABLE courses (
    id          integer NOT NULL,
    name        text    NOT NULL,
    FOREIGN KEY (school_id) REFERENCES schools (id),
    PRIMARY KEY (id)
);
```

Good:
```sql
CREATE TABLE courses (
    id          integer NOT NULL,
    name        text    NOT NULL,

    PRIMARY KEY (id),
    FOREIGN KEY (school_id) REFERENCES schools (id)
);
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Keep each constraint single-focused

Each `CHECK()` constraint should validate exactly one rule, do not combine multiple conditions into a single constraint. Use `CHECK()` constraints to enforce numerical ranges, with at minimum a `> 0` guard where a value must be positive.

##### Example

Bad:
```sql
CREATE TABLE assessments (
    student_id  integer NOT NULL,
    score       numeric NOT NULL,
    weight      numeric NOT NULL,

    CONSTRAINT chk_assessment_values CHECK (score >= 0 AND score <= 100 AND weight > 0)
);
```

Good:
```sql
CREATE TABLE assessments (
    student_id  integer NOT NULL,
    score       numeric NOT NULL,
    weight      numeric NOT NULL,

    CONSTRAINT chk_assessments_score_min    CHECK (score >= 0),
    CONSTRAINT chk_assessments_score_max    CHECK (score <= 100),
    CONSTRAINT chk_assessments_weight_min   CHECK (weight > 0)
);
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Use `LIKE` and `SIMILAR TO` for string format validation in constraints

When constraining the format of a text column, use `LIKE` for simple pattern matching or `SIMILAR TO` for regex-style patterns. These are portable SQL standard expressions.

##### Example

Bad:
```sql
CREATE TABLE students (
    email       text NOT NULL,
    school_code text NOT NULL
);
```

Good:
```sql
CREATE TABLE students (
    email       text NOT NULL CHECK (email LIKE '%@%.%'),
    school_code text NOT NULL CHECK (school_code SIMILAR TO '[A-Z]{3}[0-9]{4}')
);
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.
