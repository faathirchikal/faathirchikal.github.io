---
title: A governed platform for data question agents
summary: A platform where a team describes its tables in YAML and gets an LLM agent that answers data questions without ever seeing data it shouldn't.
status: In development
question: How can any team get a data agent that answers correctly and never shows rows a user may not see?
stack: [Python, LangGraph, Gemini, Cube, BigQuery, FastAPI]
order: 5
---
Kalbe Farma · 2026 to now · sole designer and builder.

## Problem

Staff ask business questions in the company's internal chat assistant, and a domain agent answers by querying
the data warehouse. The first agent was built by hand, and each new domain would repeat that work,
including the access rules. The goal: a domain team describes its tables in YAML and gets a governed
agent without writing a service.

## Constraints

- **Row-level access is non-negotiable.** A sales representative sees only their own territory, and
  the agent refuses whenever access is in doubt.
- **Numbers must match the dashboards** analysts already trust.
- **Domain teams know their data and have no LLM engineering background,** so what they write must be declarative.
- **One team's mistake must not break another team's agent.**

## Approach

**What a team writes.** YAML naming its views (one fact table, at most one join), metrics, filters,
a glossary, and a set of verified questions with answers.

**The model never writes SQL.** It fills a JSON spec (view, metrics, grouping, filters, period),
where every field is an enumeration of what the domain declared. Validators refuse unknown members
and invalid aggregations. Code builds the SQL through a semantic layer (Cube).

**Access is enforced by code, outside the model.** The user's rights travel in a short-lived signed
token the model never sees, and are checked at three independent layers: the semantic layer, a
parse of the generated SQL, and a SQL guard. Any one refusing means no rows.

**Routing and isolation.** A routing agent only considers agents the user's roles can reach. Each
domain runs in its own container, so a broken YAML file takes down one agent only.


## How it's built

| Layer | Tools |
|---|---|
| Domain definition | YAML: views, metrics, glossary, verified questions |
| Agents | Python, FastAPI, LangGraph, Gemini models, a routing agent, one container per domain |
| Query layer | Cube semantic layer over BigQuery, a SQL guard (sqlglot) |
| Knowledge base | Product and outlet search with embeddings (Qdrant) |
| Platform | Registry of agents, roles and grants in PostgreSQL, React admin console |
| Delivery | Docker, GitLab CI, tracing with Langfuse |

I kept SQL writing in code because a fixed spec can be validated field by field, and access rules
can be enforced where the model cannot reach them.

## How I knew it worked

Before building, analysts write an answer key: questions with the answers their dashboards give.
Scoring uses no LLM judge.

| Outcome | Passes when |
|---|---|
| A figure | returned rows match within 0.5% |
| A ranking | leading rows match in order |
| A question it should decline | the agent declines or asks to clarify |
| Any case | no row outside the user's access appears, in any run |

Each case runs three times and the majority counts; about a third are held out from tuning. Going
live needs the match threshold on both the tuning and held-out cases, and zero leaks. The same test decides when the
hand-built agent retires and whether a new model is adopted.

## What I'd change

- **Get the answer key signed before scoring starts,** so every comparison runs against final answers.
- **Give the model names to ground on.** When a view held only product codes, the agent invented
  product names. A name lookup belongs in the view from the start.

## Status

In development. Access controls, the engine and the routing agent are built and tested. Go-live
waits on the analysts signing off the answer key.
