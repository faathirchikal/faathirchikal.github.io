---
title: A governed platform for data question agents
summary: A platform where a team describes its tables in YAML and gets an LLM agent that answers data questions without ever seeing data it shouldn't.
status: In development
order: 4
---
Kalbe Farma · acting Data Science Lead · sole designer and builder.

## Problem

Staff ask business questions in a company chat assistant, and a domain agent answers each one by
querying the data warehouse. The first agent was built by hand: its own SQL builder, its own wiring
of the access rules, and its own evaluation.

Every new domain would repeat that work, and the access rules were already copied between repos
with nothing keeping the copies identical. A mistake in one copy could let a user see rows their
role does not allow.

The goal is that a domain team describes its tables in a folder of YAML and gets a governed agent
without writing a service.

## Constraints

- **Row-level access is the hard requirement.** A sales representative may see only their own
  territory, and a wrong answer is better than a leaked one.
- **Numbers must match the dashboards** that analysts already trust.
- **Domain teams know their data, not LLM engineering.** What they write has to be declarative.
- **One team's mistake must not break another team's agent.**

## Approach

**What a domain team writes.** A domain file names the dataset and its access axes. Each view is
one fact table with at most one join to a dimension, plus its metrics, filters and time key. A
glossary, short rules and a set of verified questions with answers complete the folder.

**The model never writes SQL.** It fills a JSON spec: view, metrics, grouping, filters, period and
limit. Each field is an enumeration of what the domain declared. Validators refuse unknown members,
stock metrics summed across months, and mixed units. Code then builds the SQL through a semantic
layer (Cube), which maps business terms such as "net sales" to tables and formulas.

**Access is applied by code, and any doubt means no data.** The routing service signs a short-lived token
carrying the user's access rights for that domain, and the model never sees it. Those rights are applied in
three places:

1. The semantic layer adds them as filters and stops with an error if they are missing.
2. The backend parses the generated SQL and requires a top-level condition for every restricted axis.
3. A SQL guard accepts only a plain SELECT with declared joins and allowed aggregates.

Any one of them refusing means no rows are returned.

**Routing.** A routing agent narrows the agents to those the user's roles can reach, then picks one.
A user with no access in a domain is refused before any model call.

**Isolation.** Each domain runs as its own process and container, so a broken YAML file takes down one
agent only.

![Request path: user, routing agent, domain agent, JSON spec, semantic layer with access filter, SQL guard, warehouse]({{ "/diagrams/agent-request-path.svg" | relative_url }})

## How I knew it worked

Before building, I settled what "works" means. Analysts write an answer key: questions with the
answers their dashboards give. Scoring uses no LLM judge:

| Outcome | Passes when |
|---|---|
| A figure | returned rows match within 0.5% relative tolerance |
| A ranking | leading rows match in order |
| A question it should decline | the agent asks a clarifying question or declines |
| Any case | no row outside the user's access appears, in any run |

Each case runs three times and the majority counts. About a third of cases are held out from tuning.
Going live needs the match threshold on both halves, zero leaks, every decline correct, and
cases spread across at least two different access levels.

The same rule decides when the hand-built agent retires: the platform agent must score at or above
it on the same set with zero leaks. A new model or framework faces the same test, so no
technology change happens on impressions.

## What I'd change

- **Get the answer key signed earlier.** The scoring was ready before the analysts had
  confirmed their answers, so the first comparison ran on a draft key. The gate is only as good as
  the answers it checks against.
- **Give the model names to ground on.** When a view held only product codes, the agent invented
  product names. A name lookup belongs in the view from the start.
- **Set a response-time budget.** Accuracy has a bar to clear and speed has none yet.

## Status

In development. The access controls, the engine and the routing agent are built and tested. Go-live waits
on the analysts signing off the answer key.
