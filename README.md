# Aircury's SQL Style Guide

This is the proposed SQL style guide for Aircury.  These guidelines are meant to inform, not dictate. Every project comes with its own constraints: efficiency concerns, business rules, specific contexts... There will be times when following a rule to the letter does more harm than good. Trust your judgement, and don't be afraid to deviate when you have good reason to. Just remember to be consistent throughout your projects with the decisions you make.

## Proposal considerations

Most of the rules come from other SQL style guides, where a decision has been made with some reasoning, and no other guide contradicts it. Those rules are as-is, with no modifications planned. Others, however, come mostly from my opinion or are not a consensus between the guides. Those roles are open and encouraged to be discussed. They will be marked with *Proposal*. Some other times, only part of the rule may be up for discussion. Those will be marked with *Details to discuss*.

# Sources

## Celko

*Joe Celko's SQL Programming Style* is a seminal, vendor-agnostic textbook that laid the foundational philosophy for modern SQL formatting and best practices. Published in 2005, it remains highly respected for its deep dive into the theoretical and structural aspects of writing clean code, even if some of its specific efficiency considerations or procedural advice feel slightly dated in the context of modern database architectures. Often cited as the direct predecessor to many of today’s web-based frameworks, it is widely utilized by database administrators and backend developers seeking a rigorous, historically grounded approach to database design that emphasizes logic and standardized typography over automated tooling.

**You can find the source here:** https://poornaprajnalibrary.com/admin/uploads/cmbooks/Joe%20Celko%27s%20SQL%20Programming%20Style%20%28The%20Morgan%20Kaufmann%20Series%20in%20Data%20Management%20Systems%29%20%28%20PDFDrive%20%29.pdf

## Holywell

Simon Holywell's SQL Style Guide is a widely adopted, vendor-agnostic framework designed to promote consistency, readability, and maintainability in collaborative database development. Created as an easily accessible, web-based successor to Joe Celko’s textbook standards, it is extensively utilized across the data industry by startups, enterprise teams, and open-source projects, often serving as a foundational baseline for corporate guidelines. Its significant popularity is reflected in its high search visibility, translations into over a dozen languages, ongoing community debates regarding its polarising formatting rules (such as "river alignment"), and the development of dedicated automated linters built to enforce its specific conventions.

**You can find the source here:** https://www.sqlstyle.guide/

## PostgreSQL

The PostgreSQL implicit style guide is an unofficial but highly influential set of conventions derived directly from the consistent formatting found within the official PostgreSQL documentation. Rather than a published rulebook, it serves as a pragmatic, dialect-specific baseline that native developers naturally adopt. Its popularity stems from its absolute alignment with the database's own ecosystem, providing a reliable, friction-free reference point for engineers building applications, writing tutorials, or developing extensions specifically tailored to the nuances of the Postgres environment.

**You can find the source here:** https://www.postgresql.org/docs/current/sql.html

## PG.ai

The Postgres.ai Style Guide is a modern, dialect-specific framework tailored specifically for the PostgreSQL ecosystem, with a strong emphasis on database-as-code principles and CI/CD integration. Designed for large-scale, automated environments, it moves beyond basic formatting to include rules that optimize for query performance, safe database migrations, and seamless collaboration in Git-based workflows. It is frequently adopted by advanced database engineering teams and DBAs who require a rigorous standard that addresses both the visual readability of SQL and the strict operational realities of managing high-traffic, production-grade Postgres deployments.

**You can find the source here:** https://postgres.ai/rules/rules-pages

## Mozilla

Mozilla’s SQL Style Guide is a specialized framework embedded within their broader data documentation, designed primarily to handle the complexities of large-scale telemetry and analytical data warehousing. The guide is heavily focused on readability and maintainability for massive analytical queries. It is particularly popular among data engineers and analysts working in modern cloud data environments (such as BigQuery), serving as a stellar example of how a major open-source organization structures its internal data pipelines and querying practices.

**You can find the source here:** https://docs.telemetry.mozilla.org/concepts/sql_style

## Brooklyn

The Brooklyn Data Co. SQL Style Guide is a prominent, modern framework heavily oriented towards analytics engineering and the modern data stack, particularly within dbt (data build tool) ecosystems. It champions pragmatic readability and developer experience, advocating for accessible conventions. Widely adopted by data consultancies and agile startups, it serves as a highly practical, analytics-first baseline that prioritizes clean visual hierarchy and consistency for teams building robust data warehouses in platforms like Snowflake or BigQuery.

**You can find the source here:** https://github.com/brooklyn-data/co/blob/main/sql_style_guide.md

## GitLab

The GitLab SQL Style Guide is a highly opinionated and rigorously enforced framework developed to maintain consistency across one of the world’s largest open-source, Postgres-backed monolithic applications. Focused heavily on performance and the avoidance of common database anti-patterns, it mandates strict formatting rules, naming conventions, and query structures optimized for massive codebases and continuous integration. Because it is actively used to govern global community contributions via merge requests, it is widely referenced by backend engineers and enterprise teams seeking a battle-tested standard for managing complex database interactions in highly collaborative, high-velocity environments.

**You can find the source here:** https://handbook.gitlab.com/handbook/enterprise-data/platform/sql-style-guide/

## Mazur

Matt Mazur's SQL Style Guide is a highly influential, independently created framework that prioritizes human readability and a clean visual hierarchy for analytical query writing. Born from practical experience in data analytics, it focuses on sensible, easy-to-follow formatting rules without the overwhelming rigidity of some enterprise-level guides. Its accessibility and focus on the "visual shape" of the code have made it a favorite among data scientists, analysts, and indie developers who want a lightweight, elegant standard that makes complex data wrangling scripts instantly understandable to anyone reviewing the code.

**You can find the source here:** https://github.com/mattm/sql-style-guide

## Supabase

The Supabase Postgres SQL Style Guide is a modern, highly pragmatic framework uniquely designed to be ingested as a system prompt by AI coding assistants (such as GitHub Copilot, Cursor, ...) to enforce consistent styling during automated code generation. Tailored specifically for the Supabase ecosystem, it champions accessible, developer-friendly conventions that prioritize readability over raw performance. Its popularity is surging among full-stack developers and agile teams utilizing backend-as-a-service (BaaS) platforms, as it serves a powerful dual purpose: acting as a clean, readable rulebook for human engineers while actively preventing AI tools from generating chaotic, non-standard database migrations and queries in Postgres environments.

**You can find the source here:** https://supabase.com/docs/guides/getting-started/ai-prompts/code-format-sql


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

# Comments

## *Proposal* Use `/* */` for multi-line comments and `--` for single-line comments

Use `/* */` for block or multi-line comments. Use `--` for single-line and inline comments; end the line with a newline after the comment. Do not use `--` for block comments that may need to expand, and do not use `/* */` for inline remarks where a single `--` is clearer.
One of the source advocates using `/* */` exclusively so that single-line comments can expand without syntax changes.

##### Example

Bad:
```sql
-- This query returns all active students
-- enrolled in at least one course
-- during the current academic year
SELECT
    students.full_name,
    count(enrolments.course_id) AS total_courses
FROM students
-- Current enrolments
INNER JOIN enrolments 
    ON students.id = enrolments.student_id
WHERE 
    students.is_active IS true
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
-- Current enrolments
INNER JOIN enrolments 
    ON students.id = enrolments.student_id
WHERE 
    students.is_active IS true
```

Good (always `/* */`):
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
    -- Weighted final grade: 60% assessment score, 40% attendance
    score * 0.6 + attendance_score * 0.4 AS final_grade
FROM students
WHERE 
    cohort = 2025
```

#### Source breakdown

 - **For (2)**: Holywell, Celko.
 - **Against (0)**
 - **Not mentioned (7)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Add a brief description for calculations; link to the metric definition if available

Any non-trivial calculation or derived metric should have a short comment explaining what it computes. If the metric has a formal definition (e.g. in a data dictionary or documentation), include a link.

##### Example

Bad:
```sql
SELECT
    student_id,
    sum(score) / nullif(count(*), 0)                     AS avg_score,
    count(CASE WHEN passed IS true THEN 1 END) / count(*) AS pass_rate
FROM assessments
GROUP BY student_id
```

Good:
```sql
SELECT
    student_id,
    -- Average assessment score, excluding students with no attempts
    sum(score) / nullif(count(*), 0) AS avg_score,
    /* 
     * Pass rate: share of assessments where the student passed
     * See: https://docs.aircury.com/metrics/pass-rate 
     */
    count(CASE WHEN passed IS true THEN 1 END) / count(*) AS pass_rate
FROM assessments
GROUP BY student_id
```

#### Source breakdown

 - **For (1)**: GitLab.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Brooklyn, Mazur, Supabase.

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

---

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

---

# JOINs

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

---

# Preferred Query Constructs

## Combine multiple boolean conditions with `BETWEEN` and `IN()`

Use `BETWEEN` instead of combining multiple `AND` conditions to express a range. Use `IN()` instead of chaining `OR` clauses to match against a set of values. Both make the intent clearer and are easier to scan.

##### Example

Bad:
```sql
SELECT *
FROM enrolments
WHERE 
    cohort >= 2022
    AND cohort <= 2024
    AND (
        status = 'active'
        OR status = 'pending'
        OR status = 'deferred'
    )
```

Good:
```sql
SELECT *
FROM enrolments
WHERE 
    cohort BETWEEN 2022 AND 2024
    AND status IN ('active', 'pending', 'deferred')
```

#### Source breakdown

 - **For (1)**: Holywell.
 - **Against (0)**
 - **Not mentioned (8)**: Celko, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Use `CASE` for conditional logic

Use `CASE` expressions for inline conditional logic. When the same expression is compared against multiple values, use the simple `CASE <expression> WHEN <value>` form rather than repeating the expression in every `WHEN` branch.

##### Example

Bad:
```sql
SELECT
    full_name,
    CASE
        WHEN grade_band = 'A' THEN 'Distinction'
        WHEN grade_band = 'B' THEN 'Merit'
        WHEN grade_band = 'C' THEN 'Pass'
        ELSE 'Fail'
    END AS grade_label
FROM assessments
```

Good:
```sql
SELECT
    full_name,
    CASE grade_band
        WHEN 'A' THEN 'Distinction'
        WHEN 'B' THEN 'Merit'
        WHEN 'C' THEN 'Pass'
        ELSE 'Fail'
    END AS grade_label
FROM assessments
```

#### Source breakdown

 - **For (3)**: Holywell, Celko, GitLab.
 - **Against (0)**
 - **Not mentioned (6)**: PostgreSQL, PG.ai, Mozilla, Brooklyn, Mazur, Supabase.

## Prefer `UNION ALL` over `UNION`

`UNION ALL` is more performant than `UNION` because it skips the sort and deduplication step. Only use `UNION` when removing duplicate rows is genuinely part of the requirement.

##### Example

Bad:
```sql
SELECT 
    student_id, 
    course_id 
FROM enrolments 
WHERE status = 'active'
UNION
SELECT 
    student_id, 
    course_id 
FROM archived_enrolments 
WHERE status = 'active'
```

Good:
```sql
SELECT 
    student_id, 
    course_id 
FROM enrolments 
WHERE status = 'active'
UNION ALL
SELECT 
    student_id, 
    course_id 
FROM archived_enrolments 
WHERE status = 'active'
```

#### Source breakdown

 - **For (4)**: Holywell, Celko, Brooklyn, GitLab.
 - **Against (0)**
 - **Not mentioned (5)**: PostgreSQL, PG.ai, Mozilla, Mazur, Supabase.

## Use `SELECT DISTINCT` instead of `GROUP BY` on all columns when deduplicating

When the goal is to remove duplicate rows rather than aggregate, use `SELECT DISTINCT`. Using `GROUP BY` on every column in the select list to achieve the same result obscures intent.

##### Example

Bad:
```sql
SELECT
    student_id,
    course_id
FROM enrolments
GROUP BY
    student_id,
    course_id
```

Good:
```sql
SELECT DISTINCT
    student_id,
    course_id
FROM enrolments
```

#### Source breakdown

 - **For (1)**: Brooklyn.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Mazur, Supabase.

## Avoid `ORDER BY` unless necessary to produce the correct result

Do not add `ORDER BY` to a query just for convenience or aesthetics. Sorting is expensive and the results of most queries will be re-processed downstream (by application code, a window function, or another query) where the order is irrelevant or re-applied anyway.

##### Example

Bad:
```sql
SELECT
    student_id,
    count(course_id) AS total_courses
FROM enrolments
GROUP BY student_id
ORDER BY student_id
```

Good:
```sql
SELECT
    student_id,
    count(course_id) AS total_courses
FROM enrolments
GROUP BY student_id
```

#### Source breakdown

 - **For (1)**: Brooklyn.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Mazur, Supabase.

## *Proposal* Always use explicit column names in `GROUP BY`

Use column names rather than positional numbers in `GROUP BY` clauses. Positional references (`GROUP BY 1, 2`) are fragile, as reordering the `SELECT` list silently changes what is being grouped without any error.

Some consider positional references acceptable when the expression being grouped is complex or repeated verbatim in the `SELECT` list, to avoid duplication. 

What is a consensus between the sources is to never mix positional references with column names in the same `GROUP BY` clause.

##### Example

Bad:
```sql
SELECT
    school_id,
    cohort,
    count(student_id) AS total_students
FROM students
GROUP BY 1, 2
```

Good:
```sql
SELECT
    school_id,
    cohort,
    count(student_id) AS total_students
FROM students
GROUP BY school_id, cohort
```

#### Source breakdown

 - **For explicit names (2)**: PG.ai, Mozilla.
 - **Positional acceptable for complex expressions (4)**: PG.ai, Mozilla, Brooklyn, Mazur.
 - **Not mentioned (5)**: Holywell, Celko, GitLab, Supabase.

## Prefer `WHERE` over `HAVING` when either would suffice

`WHERE` filters rows before aggregation; `HAVING` filters after. When a condition does not depend on an aggregate result, place it in `WHERE`: it is more explicit about intent and filters earlier, reducing the rows that need to be aggregated.

##### Example

Bad:
```sql
SELECT
    school_id,
    count(student_id) AS total_students
FROM students
GROUP BY school_id
HAVING school_id IS NOT NULL
```

Good:
```sql
SELECT
    school_id,
    count(student_id) AS total_students
FROM students
WHERE school_id IS NOT NULL
GROUP BY school_id
```

#### Source breakdown

 - **For (2)**: Brooklyn, GitLab.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Mazur, Supabase.

## Use lateral column aliasing when grouping by name

When an expression in `SELECT` has a column alias, you can reference that alias directly in `GROUP BY` rather than repeating the full expression. This avoids duplication and makes the query easier to maintain.

##### Example

Bad:
```sql
SELECT
    date_trunc('year', enrolled_at) AS enrolment_year,
    count(student_id)               AS total_students
FROM enrolments
GROUP BY date_trunc('year', enrolled_at)
```

Good:
```sql
SELECT
    date_trunc('year', enrolled_at) AS enrolment_year,
    count(student_id)               AS total_students
FROM enrolments
GROUP BY enrolment_year
```

#### Source breakdown

 - **For (1)**: Mazur.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Supabase.

## Place non-aggregated columns first in `SELECT`, aggregated columns last

In a query with aggregation, list the grouping (non-aggregated) columns first, followed by the aggregated expressions. This mirrors the logical order of `GROUP BY` and makes the structure of the query easier to parse at a glance.

##### Example

Bad:
```sql
SELECT
    count(student_id) AS total_students,
    avg(score)        AS avg_score,
    school_id,
    cohort
FROM students
GROUP BY school_id, cohort
```

Good:
```sql
SELECT
    school_id,
    cohort,
    count(student_id) AS total_students,
    avg(score)        AS avg_score
FROM students
GROUP BY school_id, cohort
```

#### Source breakdown

 - **For (2)**: GitLab, Mazur.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Brooklyn, Supabase.

## Be explicit in boolean conditions

Write boolean comparisons using `IS true` and `IS false` rather than relying on implicit boolean evaluation. Why use `IS` instead of `=`? Unlike `= true`, the `IS` form always returns a definite boolean, never propagating `NULL`. When the column is `NULL`, `column = true` evaluates to `NULL` (which SQL treats as falsy), while `column IS true` evaluates to `FALSE`. This distinction matters: `= true` and `IS true` behave the same in a `WHERE` clause, but `IS true` makes the null behaviour explicit and works correctly in `CASE` expressions and derived columns where a `NULL` result would otherwise silently propagate.

##### Example

Bad:
```sql
SELECT *
FROM students
WHERE 
    is_active
    AND NOT has_scholarship
```

Good:
```sql
SELECT *
FROM students
WHERE 
    is_active IS true
    AND has_scholarship IS false
```

#### Source breakdown

 - **For (1)**: Mazur.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Supabase.

## Use `!=` instead of `<>`

Prefer `!=` over `<>` for the not-equal operator. Both are valid SQL, but `!=` is more common in other programming languages and reads more naturally as "not equal".

##### Example

Bad:
```sql
SELECT *
FROM students
WHERE 
    cohort <> 2024
    AND status <> 'withdrawn'
```

Good:
```sql
SELECT *
FROM students
WHERE 
    cohort != 2024
    AND status != 'withdrawn'
```

#### Source breakdown

 - **For (3)**: Brooklyn, GitLab, Mazur.
 - **Against (0)**
 - **Not mentioned (6)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, Supabase.

## Break long `IN` lists into multiple indented lines

When an `IN` list contains many values, place each value on its own indented line. This makes the list easier to scan and simplifies adding or removing values in version control diffs.

##### Example

Bad:
```sql
SELECT *
FROM students
WHERE school_id IN (101, 102, 103, 104, 105, 106, 107, 108)
```

Good:
```sql
SELECT *
FROM students
WHERE school_id IN (
    101,
    102,
    103,
    104,
    105,
    106,
    107,
    108
)
```

#### Source breakdown

 - **For (2)**: Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Supabase.

## Avoid redundant parentheses

Do not add parentheses around expressions that do not require them. Extra parentheses suggest a precedence concern where there is none, and add visual noise that makes expressions harder to read.

##### Example

Bad:
```sql
SELECT *
FROM students
WHERE 
    (is_active IS true)
    AND (cohort > 2020)
    AND (school_id IN (101, 102))
```

Good:
```sql
SELECT *
FROM students
WHERE 
    is_active IS true
    AND cohort > 2020
    AND school_id IN (101, 102)
```

#### Source breakdown

 - **For (1)**: Celko.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

---

# CASE Statements

## Use a multi line `CASE` structure

For `CASE` expressions that span multiple lines, indent each `WHEN` one level under `CASE`. Place `THEN` on the same line as its `WHEN`. Put `ELSE` at the same indentation level as `WHEN`. Close with `END` on its own line, aligned with `CASE`.


##### Example

Bad:
```sql
SELECT
    full_name,
    CASE WHEN grade >= 70 THEN 'Pass'
    WHEN grade >= 50 THEN 'Borderline'
    ELSE 'Fail' END AS result
FROM assessments
```

Good:
```sql
SELECT
    full_name,
    CASE
        WHEN grade >= 70 THEN 'Pass'
        WHEN grade >= 50 THEN 'Borderline'
        ELSE 'Fail'
    END AS result
FROM assessments
```

#### Source breakdown

 - **For (2)**: Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Supabase.

## A single short `WHEN`/`THEN` can stay on one line

When a `CASE` has only one `WHEN` branch and the `THEN` value is short, the entire expression can be written on a single line.

##### Example

```sql
SELECT
    full_name,
    CASE WHEN is_active IS true THEN details END AS details
FROM students
```

#### Source breakdown

 - **For (1)**: Brooklyn.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Mazur, Supabase.

## `CASE` inside a function call

When a `CASE` expression is nested inside a function call, place `CASE` and `END` each on their own lines, indented one level inside the function's parentheses.

##### Example

Bad:
```sql
SELECT
    count(CASE WHEN grade >= 50 THEN student_id ELSE NULL END) AS passing_students
FROM assessments
```

Good:
```sql
SELECT
    count(
        CASE
            WHEN grade >= 50 THEN student_id
            ELSE NULL
        END
    ) AS passing_students
FROM assessments
```

#### Source breakdown

 - **For (1)**: Brooklyn.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Mazur, Supabase.

---

# Window Functions

## Keep window functions on one line when they fit; expand sub-clauses when they don't

If a window function fits on a single line without exceeding the line length limit, keep it there. When it needs to break across lines, place each sub-clause (`PARTITION BY`, `ORDER BY`, `ROWS/RANGE BETWEEN`) on its own line indented inside `OVER()`, and put the closing parenthesis on its own line at the same indentation level as the function.

##### Example

One line (fits):
```sql
SELECT
    student_id,
    row_number() OVER (PARTITION BY cohort ORDER BY score DESC) AS rank
FROM assessments
```

Multi-line (too long):
```sql
SELECT
    student_id,
    row_number() OVER (
        PARTITION BY cohort, school_id
        ORDER BY score DESC, enrolled_at ASC
    ) AS rank
FROM assessments
```

#### Source breakdown

 - **For (2)**: Brooklyn, Mazur.
 - **Against (0)**
 - **Not mentioned (7)**: Holywell, Celko, PostgreSQL, PG.ai, Mozilla, GitLab, Supabase.

---

# Views

## Always explicitly name the columns a view exposes

Never rely on `SELECT *` in a view definition. Always list the columns the view exposes by name. This prevents silent breakage when the underlying table gains, loses, or reorders columns.

##### Example

Bad:
```sql
CREATE VIEW active_students AS
SELECT *
FROM students
WHERE is_active IS true
```

Good:
```sql
CREATE VIEW active_students AS
SELECT
    id,
    full_name,
    cohort,
    school_id
FROM students
WHERE is_active IS true
```

#### Source breakdown

 - **For (1)**: Celko.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.

## Avoid view proliferation

Do not create views as a default abstraction layer. Each view should exist because it solves a specific, recurring problem, such as enforcing access control, encapsulating a complex join, or standardising a derived metric. If a view has no clear purpose, it becomes maintenance burden without value.

##### Example

Bad:
```sql
-- A view that just mirrors the base table with no added value
CREATE VIEW students_view AS
SELECT
    id,
    full_name,
    cohort,
    school_id,
    is_active
FROM students
```

Good:
```sql
-- A view that encapsulates a meaningful, reusable definition
CREATE VIEW active_students_with_school AS
SELECT
    students.id,
    students.full_name,
    students.cohort,
    schools.name AS school_name
FROM students
INNER JOIN schools
    ON students.school_id = schools.id
WHERE students.is_active IS true
```

#### Source breakdown

 - **For (1)**: Celko.
 - **Against (0)**
 - **Not mentioned (8)**: Holywell, PostgreSQL, PG.ai, Mozilla, Brooklyn, GitLab, Mazur, Supabase.
