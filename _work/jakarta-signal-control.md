---
title: Where the leverage sits in Jakarta signal control
summary: A benchmark of nine traffic-signal controllers on a simulated 12-junction Jakarta corridor. Learned control held 43.5% less queue than the best classical method in the model.
status: Research, Monash thesis
order: 1
---

Monash University · Master of Data Science thesis · sole code author, first author with supervisor
Muhamad Risqi Utama Saputra. **Status:** research paper submitted for the degree in September 2026,
not peer-reviewed. Code and results: [marl-tsc-jakarta-corridor](https://github.com/faathirchikal/marl-tsc-jakarta-corridor).

## Problem

Jakarta's arterial roads are saturated, and about three in four vehicles on them are motorcycles.
Research on learned traffic-signal control mostly uses car traffic on tidy grids. I wanted to know
where the gains actually come from on a road like this. Is it the learning itself, coordination
between junctions, or the rules on which signal phase may come next?

## Constraints

- **No public origin-destination data exists for Jakarta,** so traffic demand had to be built and
  checked against what could be observed.
- **Motorcycles filter between cars,** so the simulator has to model lanes finer than one vehicle wide.
- **A controller must be deployable.** Real signals run phases in a fixed order, and a learner that
  skips around may look good only because it breaks that rule.
- **Learned controllers are noisy.** One lucky training run proves nothing.

## Approach

**The corridor.** I built a 12-junction model of the Cawang to Jatinegara corridor in SUMO, a
traffic simulator, from OpenStreetMap. Demand comes from a gravity model, checked against travel
times from TomTom and Google Routes: 13,825 trips over two simulated hours, with about 74%
motorcycles.

**Nine controllers.** These are fixed-time, coordinated fixed-time, two versions of Max-Pressure
(the strongest classical method here), and learned controllers trained with PPO. The learned ones
range from fully independent agents to agents sharing a central critic, built three ways (MLP,
graph attention, and graph attention with time).

**A ladder of sizes.** Every controller runs on 1, 3, 6, 9 and 12 junctions, so I could see where
learning starts to pay.

## How I knew it worked

- **Ten seeds per controller,** each scored over 20 episodes. Results are robust averages with
  bootstrap intervals.
- **A margin set in advance.** Two controllers count as equivalent when they are within 15 halting
  vehicles of each other, using a formal equivalence test. I fixed that margin before seeing results.
- **A positive control.** A weaker learning method (IQL) had to come out worse, and it did, by 25.7
  halting vehicles. That shows the test can tell controllers apart.
- **Every number re-derives.** All 595 numbers in the paper are recomputed from the results
  committed to the public repo.

## What I found

| Finding | Result |
|---|---|
| Small corridors (1 to 3 junctions) | Learned control is within two halting vehicles of the best classical method |
| Full corridor (12 junctions) | Learned control holds 43.5% less queue than the best classical method |
| Coordination through a central critic | Every critic tested is equivalent to independent agents |
| Deployable phase order | Costs 19 to 20% more queue than a free choice of phase, and re-tuning the minimum green recovers about half |

At twelve junctions the levers rank clearly. The control strategy matters most, at about 143
fewer halting vehicles from about 330. The deployable phase order costs 35 to 38. Coordination
through a critic stays inside the margin.

**The limit.** The simulated corridor fails the standard calibration check against real traffic
counts. Its GEH score is 42.88, where the usual acceptance level is 4. So these numbers compare
controllers inside the model. They are not a forecast for the real street.

## What I'd change

- **Calibrate against trajectories** before comparing controllers, so the absolute numbers mean
  something on the street.
- **Add stronger classical baselines,** such as signal timings set by Webster's method and
  vehicle-actuated control, which real cities already run.
- **Report the timings each controller actually used,** so a traffic engineer can judge whether a
  plan is one they would sign off.

## Status

Submitted in September 2026 as the research paper for my Master of Data Science, and presented
as a poster. It is not peer-reviewed. The public repo has the code and every result file. The full
12-junction run needs files that are too large to publish, and a small 3×3 grid runs end to end from a clone.
