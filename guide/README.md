# Guide index

Every rule in the guide, grouped by topic. Each rule ends with a source breakdown; the guides it cites are described in [Sources](sources.md).

## 1. [Formatting](01-formatting.md)

- [Left align all keywords, starting on the same character boundary](01-formatting.md#left-align-all-keywords-starting-on-the-same-character-boundary)
- [Each major clause on its own line](01-formatting.md#each-major-clause-on-its-own-line)
- [Place `SELECT`, `WHERE` and `HAVING` arguments below the keyword](01-formatting.md#place-select-where-and-having-arguments-below-the-keyword)
- [Use 4 spaces for indentation](01-formatting.md#use-4-spaces-for-indentation)
- [Place `JOIN` conditions below the keyword](01-formatting.md#place-join-conditions-below-the-keyword)
- [Terminate the line after an open parenthesis](01-formatting.md#terminate-the-line-after-an-open-parenthesis)
- [Indent CTE bodies one level](01-formatting.md#indent-cte-bodies-one-level)
- [Use one space between all language tokens](01-formatting.md#use-one-space-between-all-language-tokens)
- [Don't add extra spaces inside parentheses or before function parentheses](01-formatting.md#dont-add-extra-spaces-inside-parentheses-or-before-function-parentheses)
- [Put `AND` / `OR` at the beginning of the next line](01-formatting.md#put-and--or-at-the-beginning-of-the-next-line)
- [Add new lines after semicolons and keyword definitions](01-formatting.md#add-new-lines-after-semicolons-and-keyword-definitions)
- [Use blank lines between logically distinct sections](01-formatting.md#use-blank-lines-between-logically-distinct-sections)
- [Use trailing commas](01-formatting.md#use-trailing-commas)
- [Keep lines under 120 characters](01-formatting.md#keep-lines-under-120-characters)

## 2. [Keywords](02-keywords.md)

- [Use uppercase for reserved words](02-keywords.md#use-uppercase-for-reserved-words)
- [Full keyword forms for reserved words](02-keywords.md#full-keyword-forms-for-reserved-words)
- [Avoid vendor-specific keywords](02-keywords.md#avoid-vendor-specific-keywords)
- [Use lowercase for functions](02-keywords.md#use-lowercase-for-functions)
- [Use uppercase for data types](02-keywords.md#use-uppercase-for-data-types)

## 3. [Naming](03-naming.md)

- [General naming rules](03-naming.md#general-naming-rules)
    - [Use snake case](03-naming.md#use-snake-case)
    - [Names must be descriptive, consistent and unambiguous](03-naming.md#names-must-be-descriptive-consistent-and-unambiguous)
    - [Use unquoted alphanumeric names](03-naming.md#use-unquoted-alphanumeric-names)
    - [Do not use reserved words as identifiers](03-naming.md#do-not-use-reserved-words-as-identifiers)
- [Use collective, class or plural nouns for tables](03-naming.md#use-collective-class-or-plural-nouns-for-tables)
- [Avoid concatenating names for relationship tables](03-naming.md#avoid-concatenating-names-for-relationship-tables)
- [Column naming rules](03-naming.md#column-naming-rules)
    - [Use singular names for columns](03-naming.md#use-singular-names-for-columns)
    - [Use standardised prefixes and suffixes](03-naming.md#use-standardised-prefixes-and-suffixes)
    - [Do not use the table name as a column name or prefix](03-naming.md#do-not-use-the-table-name-as-a-column-name-or-prefix)

## 4. [Aliasing](04-aliasing.md)

- [Always use the `AS` keyword explicitly](04-aliasing.md#always-use-the-as-keyword-explicitly)
- [Align `AS` keywords](04-aliasing.md#align-as-keywords)
- [Always use aliasing for aggregated and other column expressions](04-aliasing.md#always-use-aliasing-for-aggregated-and-other-column-expressions)
- [Aim for clarity in table aliasing](04-aliasing.md#aim-for-clarity-in-table-aliasing)
- [Column aliases should semantically relate to what they represent](04-aliasing.md#column-aliases-should-semantically-relate-to-what-they-represent)
- [Always qualify column references in queries with `JOIN`](04-aliasing.md#always-qualify-column-references-in-queries-with-join)
- [Omit table references for single-table queries](04-aliasing.md#omit-table-references-for-single-table-queries)

## 5. [Comments](05-comments.md)

- [Use `/* */` for comments and `--` for commented-out code](05-comments.md#use---for-comments-and----for-commented-out-code)
- [Comment only where necessary](05-comments.md#comment-only-where-necessary)
- [Comment CTEs with confusing or notable logic](05-comments.md#comment-ctes-with-confusing-or-notable-logic)

## 6. [Data types](06-data-types.md)

- [Use `NUMERIC` or `DECIMAL` for monetary and precision values](06-data-types.md#use-numeric-or-decimal-for-monetary-and-precision-values)
- [Prefer `BOOLEAN` for logical values: use `true`/`false` instead of `1`/`0`](06-data-types.md#prefer-boolean-for-logical-values-use-truefalse-instead-of-10)
- [Default values should match the column type](06-data-types.md#default-values-should-match-the-column-type)
- [Use ISO 8601 format for date/time values and ISO temporal syntax in queries](06-data-types.md#use-iso-8601-format-for-datetime-values-and-iso-temporal-syntax-in-queries)

## 7. [Constraints and keys](07-constraints-keys.md)

- [Every table must have at least one key](07-constraints-keys.md#every-table-must-have-at-least-one-key)
- [Give constraints explicit custom names](07-constraints-keys.md#give-constraints-explicit-custom-names)
- [Place table-level constraints at the end](07-constraints-keys.md#place-table-level-constraints-at-the-end)
- [Declare `PRIMARY KEY` first, immediately after the column list](07-constraints-keys.md#declare-primary-key-first-immediately-after-the-column-list)
- [Keep each constraint single-focused](07-constraints-keys.md#keep-each-constraint-single-focused)
- [Use `LIKE` and `SIMILAR TO` for string format validation in constraints](07-constraints-keys.md#use-like-and-similar-to-for-string-format-validation-in-constraints)

## 8. [JOIN clauses](08-joins.md)

- [Use `ON` for all join conditions; avoid `USING`](08-joins.md#use-on-for-all-join-conditions-avoid-using)
- [Put the first-referenced table immediately after `ON`](08-joins.md#put-the-first-referenced-table-immediately-after-on)
- [For `INNER JOIN`, keep filter conditions in `WHERE`, not in the `JOIN` clause](08-joins.md#for-inner-join-keep-filter-conditions-in-where-not-in-the-join-clause)

## 9. [Views](09-views.md)

- [Always explicitly name the columns a view exposes](09-views.md#always-explicitly-name-the-columns-a-view-exposes)
- [Avoid view proliferation](09-views.md#avoid-view-proliferation)

## 10. [Subqueries and CTEs](10-subqueries-ctes.md)

- [Prefer CTEs over nested subqueries](10-subqueries-ctes.md#prefer-ctes-over-nested-subqueries)
- [Avoid correlated subqueries](10-subqueries-ctes.md#avoid-correlated-subqueries)
- [Each CTE should do one logical unit of work](10-subqueries-ctes.md#each-cte-should-do-one-logical-unit-of-work)
- [CTE and subquery names should be descriptive](10-subqueries-ctes.md#cte-and-subquery-names-should-be-descriptive)
- [Do not prefix or suffix CTE names with `cte`](10-subqueries-ctes.md#do-not-prefix-or-suffix-cte-names-with-cte)

## 11. [Queries](11-queries.md)

- [Combine multiple boolean conditions with `BETWEEN` and `IN()`](11-queries.md#combine-multiple-boolean-conditions-with-between-and-in)
- [`CASE` expressions](11-queries.md#case-expressions)
    - [Use `CASE` for conditional logic](11-queries.md#use-case-for-conditional-logic)
    - [Use a multi-line `CASE` structure](11-queries.md#use-a-multi-line-case-structure)
    - [A single short `WHEN`/`THEN` can stay on one line](11-queries.md#a-single-short-whenthen-can-stay-on-one-line)
    - [`CASE` inside a function call](11-queries.md#case-inside-a-function-call)
- [Prefer `UNION ALL` over `UNION`](11-queries.md#prefer-union-all-over-union)
- [Use `SELECT DISTINCT` instead of `GROUP BY` on all columns when deduplicating](11-queries.md#use-select-distinct-instead-of-group-by-on-all-columns-when-deduplicating)
- [Avoid `ORDER BY` unless necessary to produce the correct result](11-queries.md#avoid-order-by-unless-necessary-to-produce-the-correct-result)
- [Always use explicit column names in `GROUP BY`](11-queries.md#always-use-explicit-column-names-in-group-by)
- [Prefer `WHERE` over `HAVING` when either would suffice](11-queries.md#prefer-where-over-having-when-either-would-suffice)
- [Use lateral column aliasing when grouping by name](11-queries.md#use-lateral-column-aliasing-when-grouping-by-name)
- [Place non-aggregated columns first in `SELECT`, aggregated columns last](11-queries.md#place-non-aggregated-columns-first-in-select-aggregated-columns-last)
- [Be explicit in boolean conditions](11-queries.md#be-explicit-in-boolean-conditions)
- [Use `!=` instead of `<>`](11-queries.md#use--instead-of-)
- [Break long `IN` lists into multiple indented lines](11-queries.md#break-long-in-lists-into-multiple-indented-lines)
- [Avoid redundant parentheses](11-queries.md#avoid-redundant-parentheses)
- [Keep window functions on one line when they fit. Expand sub-clauses when they don't](11-queries.md#keep-window-functions-on-one-line-when-they-fit-expand-sub-clauses-when-they-dont)
