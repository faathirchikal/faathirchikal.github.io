# Demo: governed question-to-SQL agent

status: draft · locked: none

A public Python repository that answers business questions over a public sample database. The model
fills a query spec and code writes the SQL. The repository includes recorded evaluation runs. Built
last.

## Criteria

1. The data is a public sample database loaded into DuckDB. A YAML file declares views, metrics,
   dimensions and one row-access axis, such as region.
2. The model returns a JSON spec of view, metrics, grouping, filters and period, validated against the
   declared members. Code builds the SQL and applies the user's row grant. A SQL guard refuses
   anything other than a plain SELECT over declared joins.
3. The model provider sits behind one small interface. Claude is the default, and a second provider is
   a constructor argument, set from `.env`.
4. A gold set of at least 30 questions with answers computed by hand-written SQL includes clarifying
   and decline cases. Scoring checks the rows within a relative tolerance, runs three times with a
   majority vote, and counts leaks outside the grant.
5. Recorded runs are committed as JSON, and a results page renders them, so a reader sees scores
   without an API key.

## Check

- `make eval` reproduces the committed scores within run-to-run variance.
- Leak tests try to read outside the grant through filters, joins and prompt text. All are refused.
