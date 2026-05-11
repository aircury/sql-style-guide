# Keyword rules

## *Proposal* Use uppercase for reserved words

Always use **UPPERCASE** for reserved words (`SELECT`, `WHERE`, `AS`, ...). This differentiates the SQL logic from the tables and columns used in the queries and table definitions.

##### Example

Bad:
```sql
select
    adress,
    urn as school_urn
from schools
where cohort > 2024
``` 

Good:
```sql
SELECT
    adress,
    urn AS school_urn
FROM schools
WHERE cohort > 2024
```

#### Source breakdown

 - **For (5)**: Holywell, Celko, PostgreSQL, Mozilla, GitLab.
 - **Against (4)**: PG.ai, Brooklyn, Mazur, Supabase.
 - **Not mentioned (0)**

## Full keyword forms for reserved words

Always use the full keyword forms instead of the abbreviation.

##### Example

Bad:
```sql
SELECT
    *
FROM schools sc
JOIN students st
    ON sc.urn = st.school_urn
WHERE cohort > 2024
``` 

Good:
```sql
SELECT
    *
FROM schools sc
INNER JOIN students st
    ON sc.urn = st.school_urn
WHERE cohort > 2024
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, Mozilla, GitLab, PG.ai, Brooklyn, Mazur, Supabase.

## Avoid vendor-specific keywords

If an ANSI SQL keyword exists for a function or procedure, use it instead of a vendor-specific one. 

##### Example

Bad:
```sql
SELECT 
    full_name, 
    -- Specific to PostgreSQL
    date_part('year', enrolment_date) AS enrolment_year
FROM students
```

Good:
```sql
SELECT 
    full_name,
    -- ANSI SQL function
    extract(YEAR FROM enrolment_date) AS enrolment_year
FROM students
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, Mozilla, GitLab, PG.ai, Brooklyn, Mazur, Supabase.

## *Proposal* Use lowercase for functions

Treat functions as identifiers. This implies writing them in lower case: `count()` instead of `COUNT()`.

Right now, people use lowercase and uppercase for functions. From what I have seen, most of the single-worded functions are written in uppercase (`COALESCE`, `SUM`, ...), whereas multi-worded and more 'complex' ones are written in lowercase (`string_agg`, `json_array_elements`). This makes function declaration inconsistent format-wise.

My proposal is to always use **lowercase** for functions. The [Bouma](https://en.wikipedia.org/wiki/Bouma) (kind of the 'shape') of uppercase words is a rectangle, making easy for our eyes to distinguish it from regular words. Using uppercase for the reserved words and the functions may hinder the readability of a query. An opposite argument could be made for the contrary statement (using uppercase for functions) using the same reasoning: it will make it easier for the developers to distinguish column names and aliases from column transformations.

##### Example

Bad:
```sql
SELECT
    school_urn,
    COUNT(*)                    AS total_students,
    string_agg(full_name, ', ') AS student_names
FROM students
GROUP BY school_urn
```

Good:
```sql
SELECT
    school_urn,
    count(*)                    AS total_students,
    string_agg(full_name, ', ') AS student_names
FROM students
GROUP BY school_urn
```

#### Source breakdown

 - **For (5)**: PG.ai, Brooklyn, Mazur, Supabase, Mozilla.
 - **Against (3)**: GitLab, Holywell, Celko.
 - **Not mentioned (1)**: PostgreSQL.

## *Proposal* Use lowercase for data types

Similarly to the functions, there is currently a mix of lowercase and uppercase when using data type casting and for table definitions. Either uppercase or lowercase should be chosen.

The proposal of using **lowercase** is based on consistency between all rules (using uppercase only for reserved words), and is more in line with more recent guides and trends.

##### Example 

Bad:
```sql
CREATE TABLE users (
    id         INTEGER      PRIMARY KEY,
    email      VARCHAR(255) NOT NULL,
    created_at TIMESTAMP    DEFAULT CURRENT_TIMESTAMP,
    is_active  BOOLEAN      NOT NULL
)
```

Good:
```sql
CREATE TABLE users (
    id         integer      PRIMARY KEY,
    email      varchar(255) NOT NULL,
    created_at timestamp    DEFAULT CURRENT_TIMESTAMP,
    is_active  boolean      NOT NULL
)
```

#### Source breakdown

 - **For (5)**: PG.ai, Brooklyn, Mazur, Supabase, PostgreSQL.
 - **Against (3)**: GitLab, Holywell, Celko.
 - **Not mentioned (1)**: Mozilla.
