---
name: aircury-sql
description: Aircury's SQL style guide. Use when writing, formatting, reviewing or refactoring SQL (queries, DDL, views, CTEs) so it follows the team's conventions for formatting, keywords, naming, aliasing, comments, data types, constraints, joins, views, subqueries and query constructs.
---

# Aircury's SQL Style Guide

Apply these rules whenever you write, format, review or refactor SQL. The guidelines are meant to inform, not dictate: if following a rule to the letter would do more harm than good in a given context (performance, business rules, an existing codebase convention), deviate and say why. Whatever you choose, stay consistent within the project.

## How to use

1. Read [README.md](README.md) for the index of every rule.
2. Open only the topic files relevant to the SQL at hand. Each rule has a short explanation and examples of what to do and what to avoid.

| File | Covers |
| --- | --- |
| [01-formatting.md](01-formatting.md) | Alignment, line breaks, indentation, spacing, commas, line length |
| [02-keywords.md](02-keywords.md) | Case of reserved words, functions and data types; full keyword forms |
| [03-naming.md](03-naming.md) | Table, column and general identifier naming |
| [04-aliasing.md](04-aliasing.md) | `AS`, table and column aliases, qualifying columns |
| [05-comments.md](05-comments.md) | Comment style and when to comment |
| [06-data-types.md](06-data-types.md) | Numeric, boolean, default and date/time values |
| [07-constraints-keys.md](07-constraints-keys.md) | Keys, constraint naming and placement |
| [08-joins.md](08-joins.md) | `JOIN` conditions and filters |
| [09-views.md](09-views.md) | View columns and view proliferation |
| [10-subqueries-ctes.md](10-subqueries-ctes.md) | CTEs vs subqueries, CTE scope and naming |
| [11-queries.md](11-queries.md) | `CASE`, `UNION`, `DISTINCT`, `GROUP BY`, `ORDER BY`, `HAVING`, boolean conditions, window functions |

Ignore the *Source breakdown* section under each rule: it records which external guides support the rule (see [sources.md](sources.md)) and doesn't change how the rule is applied.

## When reviewing SQL

For each violation, name the rule by its heading and file (e.g. "Use trailing commas — 01-formatting.md"), show the offending fragment and the corrected version.
