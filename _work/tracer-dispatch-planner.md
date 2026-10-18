---
title: Dispatch planning for a product that decays on the road
summary: A planner that routes a radiopharmaceutical with a half-life under two hours and sets each batch's dose.
status: In acceptance testing
question: When should each vehicle leave, and how much should be produced, for a product that decays on the road?
stack: [Python, OR-Tools, FastAPI, SvelteKit, PostgreSQL, Docker]
order: 3
---
Kalbe Farma · 2025 to now · lead builder, with a small team.

## Problem

A short-lived radiopharmaceutical, used in hospital imaging, loses half its activity in under two
hours. It is made early each morning and delivered to dozens of hospitals, some by scheduled flight.
Each hospital orders a dose for the time it will use it.

Planners worked out the day in a spreadsheet: which vehicle serves which hospital, when it leaves,
and how much to produce so every dose is still enough on arrival. These decisions depend on each
other. A longer route means more decay, so more must be produced, and production has a ceiling.

## Constraints

- **The dose is set at the time of use,** so every minute on the road raises what must be produced.
- **One production batch per site per day,** with a fixed quality-control reserve inside its ceiling.
- **Vehicles cost money.** More vehicles shorten routes and lower the dose, so fleet size is a cost decision.
- **Some legs are flights.** A missed flight outranks every other failure.
- **Plans must reproduce.** A plan that changes on a re-run cannot be signed off.

## Approach

**Framing.** Routing and dosing connect through one number: the departure time. Once it is fixed,
decay is a closed formula, so one hard joint problem becomes a sweep of ordinary ones.

1. **Sweep.** For each departure time and fleet size, solve a vehicle routing problem with time
   windows (OR-Tools), with a steep penalty per late minute.
2. **Back-solve the dose.** Each stop's activity to load is its requested dose plus a safety buffer,
   divided by the decay over the time since departure.
3. **Allocate vehicles.** A small dynamic program splits the fleet across sites and flight groups,
   ranking missed flights first, then late stops, then cost.
4. **Recommend.** Among plans under the production ceiling, prefer those within half a percent of
   the cheapest, then the fewest late stops, then the fewest vehicles.
5. **Hold departures.** Each vehicle leaves as late as its tightest stop allows. Leaving early means
   more decay on the road, so more activity has to be produced and more of it is wasted.


**Refusing to guess.** A missing travel time or a partial solve stops the run with an error. An approved plan is never re-solved, because a time-limited solver can return a
different answer on a second run.

The [decay-aware routing demo](https://faathirchikal.github.io/decay-routing-demo/) rebuilds the sweep on an
invented city.

## How it's built

| Layer | Tools |
|---|---|
| Inputs | Hospital orders, flight schedules, road travel times (open-source routing engine, optional Google Routes check) |
| Engine | Python, OR-Tools (routing with time windows), a dynamic program for the fleet split |
| Application | FastAPI back end, SvelteKit web app with maps, PDF and Excel reports |
| Users | Logins with roles (admin, manager, staff), an approval step, cost fields hidden by role |
| Data | PostgreSQL as the system of record |
| Delivery | Docker, GitLab CI with tests, type checks and security scans |

I swept departure time because once it is fixed, decay is a formula, and every cell becomes a
standard routing problem that is fast to solve and easy to explain to planners.

## How I knew it worked

- **Physics tests.** The decay formula round-trips, and a multi-leg journey decays the same as one long leg.
- **Exhaustive check.** The fleet allocation matches full enumeration on small cases.
- **A pinned reference day.** Any change in behaviour shows up as a difference someone must accept.
- **The manufacturer's own numbers.** The production figures reproduce the manufacturer's workbook.

I also tested letting vehicles make a second trip. It raised total activity on the test case, so it stays off.

## What I'd change

- **Compare routing numerically with the workbook.** Production is tied to the manufacturer's figures;
  routing is checked only against its own tests.
- **Confirm cost assumptions earlier.** The recommendation depends on the safety buffer and vehicle
  cost, so they should be agreed before the sweep runs.

## Status

In acceptance testing with a company within the Kalbe group, on a test system before switching over from the spreadsheet.
