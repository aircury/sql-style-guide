# Alias rules

## Always use the `AS` keyword explicitly

Never omit the keyword when aliasing, as it provides readability.

##### Example

Bad:
```sql
SELECT
    s.full_name                     student_name,
    coalesce(count(e.course_id), 0) AS total_enrolments
FROM students AS s
LEFT JOIN enrolments e
    ON s.id = e.student_id
GROUP BY s.full_name
```

Good:
```sql
SELECT
    s.full_name                     AS student_name,
    coalesce(count(e.course_id), 0) AS total_enrolments
FROM students AS s
LEFT JOIN enrolments AS e
    ON s.id = e.student_id
GROUP BY s.full_name
```

#### Source breakdown

 - **For (9)**: Holywell, Celko, Supabase, Brooklyn, GitLab, Mozilla, Mazur, PG.ai, PostgreSQL.
 - **Against (0)**
 - **Not mentioned (0)**

## Always use aliasing for aggregated and other column expression

Always use an alias name for columns that are the result of aggregation, `CASE` statements, `sum()`, `count()`, ... Otherwise, depending of the database provider, the name could be inherited from the column used in the transformation or the function name.

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
    avg(score) AS grade_avg
FROM grades
GROUP BY cohort_name
```

#### Source breakdown

 - **For (4)**: Holywell, Celko, Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (5)**: GitLab, Mozilla, Supabase, PG.ai, PostgreSQL.

## *Proposal* Aim for clarity in table aliasing

Prefer using the full table name over an alias when the name is short enough to be readable. When aliasing is unavoidable (long or ambiguous names, self-joins), use a meaningful alias rather than an abbreviation.

Regarding what "short" means, there are different postures in the sources that are in favor of not aliasing: it ranges from less that 20 characters to less than 3 words. Those source argue that short table names and meaningful aliases improve query readability (for example, `schools` is more readable than `s`).

Other sources argue that keeping table names and big aliases increase visual noise across longer queries, especially with joins. They argue for using short, abbreviated aliases or the initials of the table words (for example, `ec` aliases `external_courses`).  Both positions are arguing from readability, but they just weigh verbosity versus brevity differently.

The only consensus is that table aliases should never be generic characters that convey no meaning, such as `x`, `t1`, ...

##### Example

If we want to avoid short table aliasing and abbreviation aliases, this is the *bad* example. If we argue the contrary, this is the *good* example:
```sql
SELECT
    s.name      AS school_name,
    sda.name    AS district_name,
    s.address   AS school_address
FROM schools AS s
INNER JOIN school_district_areas AS sda
    ON s.district_code = sda.code
```

This would be the *good* example for the first position, and the *bad* one if always using aliases is the preferred rule:
```sql
SELECT
    schools.name      AS school_name,
    districts.name    AS district_name,
    schools.address   AS school_address
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
    avg(score) AS average_scores
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
    schools.adress,
    schools.urn AS school_urn
FROM schools
WHERE schools.cohort > 2024
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

 - **For (2)**: Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (7)**: Mozilla, PostgreSQL, Supabase, Celko, PG.ai, GitLab, Holywell.
