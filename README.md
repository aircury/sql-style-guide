# Aircury's SQL Style Guide

This is Aircury's SQL style guide. These guidelines are meant to inform, not dictate. Every project comes with its own constraints: efficiency concerns, business rules, specific contexts... There will be times when following a rule to the letter does more harm than good. Trust your judgement, and don't be afraid to deviate when you have good reason to. Just remember to be consistent throughout your projects with the decisions you make.

## How the rules were chosen

Most of the rules come from other SQL style guides (listed under [Sources](guide/sources.md)), where a decision was made with some reasoning and no other guide contradicts it. Where the guides disagreed, or a rule came from our own experience, it was discussed by the team in this repository's issues before being settled. Each rule ends with a source breakdown showing which guides support it, which go against it, and which do not mention it.

## Changing or proposing a rule

The guide is not set in stone. If you think a rule should change, open an issue using the *Rule amendment* or *Rule disagreement* template, explaining what you would change and why. If you think a rule is missing, open an issue using the *Rule proposal* template.

## Using the guide with AI agents

The guide is packaged as an agent skill ([`guide/SKILL.md`](guide/SKILL.md)). Install it in your project with:

```sh
npx skills add aircury/sql-style-guide
```

# Rules structure

See the [guide index](guide/README.md) for the full list of rules.

 1. [Formatting](guide/01-formatting.md)
 2. [Keywords](guide/02-keywords.md)
 3. [Naming](guide/03-naming.md)
 4. [Aliasing](guide/04-aliasing.md)
 5. [Comments](guide/05-comments.md)
 6. [Data types](guide/06-data-types.md)
 7. [Constraints and keys](guide/07-constraints-keys.md)
 8. [JOIN clauses](guide/08-joins.md)
 9. [Views](guide/09-views.md)
 10. [Subqueries and CTEs](guide/10-subqueries-ctes.md)
 11. [Queries](guide/11-queries.md)
