---
title: A branch replenishment planner
summary: A four-stage planner that sets branch stock, shares scarce stock fairly and routes trucks, with every figure explained.
status: In acceptance testing
question: How much should each branch hold, who gets scarce stock, and which truck carries it?
stack: [Python, OR-Tools, FastAPI, React, BigQuery, Docker]
order: 4
---
Kalbe Farma · 2026 to now · lead builder, with a small team.

## Problem

A national distribution company within the Kalbe group restocks its branches from a few distribution centers. Planners did it
by hand in a spreadsheet: how much each branch should hold, how much to ship today, from which
center, on which truck. It was slow, and nobody could see why a number was what it was. The planners
wanted a daily tool that explains itself, with the final say on every quantity kept by them.

## Constraints

- **Planners decide.** The tool recommends and explains; every figure can be overridden.
- **No cost data.** With no order or holding costs, textbook cost-optimal policies had nothing to optimize.
- **Stock is shared.** When a center cannot cover every branch, the rule for who goes short must be fair and explainable.
- **Trucks have limits:** volume, payload, temperature class and region.
- **Plans must reproduce** on any machine.

## Approach

Four stages per branch and product, each consuming the one before.

| Stage | Question | Method |
|---|---|---|
| 1. Standard stock | How much should the branch hold? | Days of cover times forecast daily demand |
| 2. Shipment need | How much to ship now? | Standard stock minus all pending supply, rounded to cartons |
| 3. Allocation | Who gets scarce stock? | Committed orders first, then raise branches together toward equal days of cover |
| 4. Transport | Which truck, in what order? | Vehicle routing with pickup and delivery, then bin packing |

**Days of cover as the policy.** With no cost data, days of cover is the parameter the business
already maintains and understands.

**Even fill for allocation.** When stock is short, branches rise together toward the same days of
cover, like water finding one level. The shortfall spreads evenly and the rule fits in one sentence.

**Routing that sheds the right freight.** Each delivery is optional in the routing model
(OR-Tools), with a drop penalty that grows with priority and volume, so the lowest-priority freight
is dropped first. The search stops after a fixed number of solutions, so plans reproduce
on any machine.

**Every number explains itself.** Each figure carries a generated sentence naming its terms, and
every planner override is kept beside the engine's figure.


## How it's built

| Layer | Tools |
|---|---|
| Inputs | Stock, demand forecasts, open orders and fleet data from BigQuery, refreshed each morning |
| Engine | Python, pandas, OR-Tools (pickup and delivery, bin packing) |
| Application | FastAPI back end, React and TypeScript web app with maps |
| Users | Planner and logistics roles, sized for about ten planners at once |
| Data | PostgreSQL for saved plans and overrides |
| Delivery | Docker, GitLab CI with tests and an image build |

I used days of cover as the policy because it is the parameter planners already maintain, and the
data needed for a cost-optimal policy does not exist.

## How I knew it worked

- **Invariants under test,** such as allocated plus unserved equals demand.
- **Pinned outputs** for one frozen day, so any formula change shows up as a difference someone must accept.
- **Daily data checks.** Implausible data blocks the day or flags the row before a planner sees it.

The planners' rules were compared line by line with the engine's. That is weaker than matching
their workbook's numbers, and no planner has run a real day on it yet, so there is no savings figure yet.

## What I'd change

- **Match the spreadsheet's numbers early.** A numeric match on a few real days finds disagreements
  faster than reviewing rules.
- **Model demand uncertainty.** Deriving safety stock from forecast error would size it to each product's real risk.

## Status

In acceptance testing with the distribution company. The first two stages run on real data; allocation and transport wait
on confirmed fleet and cost inputs.
