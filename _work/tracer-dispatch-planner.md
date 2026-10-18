---
title: Dispatch planning for a product that decays on the road
summary: A planner that routes a radiopharmaceutical with a half-life under two hours and sets each batch's dose.
status: In client testing
order: 2
---
Kalbe Farma · acting Data Science Lead · lead builder, with a small team.

## Problem

A manufacturer produces a short-lived radiopharmaceutical, used in hospital imaging, that loses half
its activity in under two hours. It is made early each morning and driven to dozens of hospitals,
some reached by a scheduled flight. Each hospital orders a dose for the time it will use it.

Planners worked the day out in a spreadsheet: who rides on which vehicle, when each vehicle leaves,
and how much activity to make so every dose is still enough when it arrives. Those questions depend on
each other. A longer route means more decay, so more activity has to be produced, and production has
a ceiling.

## Constraints

- **The dose is set at the time of use.** Every minute on the road raises the activity that must be
  produced.
- **There is one production batch per site per day,** with a fixed reserve for quality control
  inside its ceiling.
- **Vehicles cost money.** Each extra driver shortens routes and reduces the dose needed, so fleet
  size is a cost decision.
- **Some legs are flights,** with handling windows on both sides. A missed flight outranks every
  other failure.
- **Plans must reproduce.** A plan that changes when it is re-run cannot be signed off.

## Approach

**Framing.** Routing and dosing look like one joint problem, but they connect through a single
number: the departure time that anchors the batch. Once the departure is fixed, decay is a closed
formula. Activity at time t is the starting activity times one half raised to t over the half-life.
That turns one hard problem into a sweep of easy ones.

1. **Sweep.** For each candidate departure time and each fleet size, solve a vehicle routing problem
   with time windows (OR-Tools). Late arrivals carry a steep penalty per minute, and each vehicle has
   a stop cap.
2. **Back-solve the dose.** For each stop, the activity to load is the requested dose at its time of
   use, plus a safety buffer, divided by the decay factor for the elapsed time since departure.
3. **Allocate vehicles.** A small dynamic program splits the fleet across sites and flight groups.
   The ranking puts missed flights first, then late stops, then cost, then balance.
4. **Recommend.** Among plans that fit under the production ceiling, prefer those within half a
   percent of the cheapest, then the fewest late stops, then the fewest vehicles.
5. **Hold departures.** After routing, each vehicle leaves as late as its tightest stop allows. The
   dose is anchored to the planned departure, so a late truck can only raise the dose and never leaves
   a hospital short.

![Sweep over departure time and fleet size, each cell a routing solve, dose back-solved from the departure anchor]({{ "/diagrams/tracer-sweep.svg" | relative_url }})

**Refusing to guess.** A missing travel time is reported, never treated as zero. A partial solve is
an error, never a shorter plan. A plan that has been reviewed is used exactly as approved and never
re-solved, because a time-limited solver can return a different answer on a second run.

## How I knew it worked

- **Physics tests.** The decay formula round-trips, multi-leg journeys decay the same as one long
  leg, and nothing jumps at hour boundaries. All decay maths lives in one module, and a test enforces
  that.
- **Exhaustive check.** The fleet allocation is checked against full enumeration on small cases.
- **A pinned reference day.** Every output of one reference day is saved, so any change in
  behaviour shows up as a difference someone has to accept.
- **The client's own numbers.** The production worksheet and the delivery documents reproduce the
  figures in the client's workbook.

I also tested a feature and dropped it. Letting vehicles return for a second trip raised total
activity on the only real case available, so it stays off.

## What I'd change

- **Compare the optimiser numerically with the workbook.** The production side is tied to the
  client's figures, but the routing side is checked only against its own tests.
- **Keep the design document in step with the code.** A sign-off removed one term from the decay
  model, and the design document still describes it.
- **Confirm the assumptions sooner.** The safety buffer, reload time and vehicle cost are still
  waiting on client confirmation, and the recommendation depends on all three.

## Status

In client acceptance testing. The client is trialling it on a test system before switching over from the spreadsheet.
