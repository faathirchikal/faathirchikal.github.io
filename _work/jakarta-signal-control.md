---
title: Where the leverage sits in Jakarta signal control
summary: A benchmark of nine traffic-signal controllers on a simulated 12-junction Jakarta corridor. Learned control held 43.5% less queue than the best classical method in the model.
status: Research, Monash thesis
question: Where do the gains in learned signal control come from on a motorcycle-heavy road?
stack: [Python, SUMO, PPO, SLURM HPC]
order: 1
photo: /figures/cawang.jpg
photo_w: 1600
photo_h: 682
photo_alt: Motorcycles fill the gaps between cars, a minibus and a bus on an arterial road at Cawang, Jakarta.
photo_credit: 'Cawang, Jakarta. Photograph by Fikri RA, <a href="https://creativecommons.org/licenses/by-sa/4.0/">CC BY-SA 4.0</a>.'
---

Monash University · Master of Data Science thesis · sole code author, first author with supervisor
Muhamad Risqi Utama Saputra. Not peer-reviewed. Code and results:
[marl-tsc-jakarta-corridor](https://github.com/faathirchikal/marl-tsc-jakarta-corridor).

## Problem

About three in four vehicles on Jakarta's saturated arterial roads are motorcycles, while research
on learned signal control mostly uses cars on tidy grids. I wanted to know where the gains come from
on a road like this: the learning itself, coordination between junctions, or the rules on which
signal phase may come next.


## Constraints

- **No public origin-destination data exists for Jakarta,** so traffic demand had to be built and
  checked against what could be observed.
- **Motorcycles filter between cars,** so the simulator has to model lanes finer than one vehicle wide.
- **A controller must be deployable.** Real signals run phases in a fixed order, and a learner that
  skips around may look good only because it breaks that rule.
- **Learned controllers are noisy.** One lucky training run proves nothing.


## Approach

[![Study overview: a 12-junction Jakarta corridor, nine controllers, and three levers tested. At twelve junctions the control strategy is worth 143 halting vehicles, the timing rules 35 to 38, and coordination under 15.]({{ "/figures/overview.png" | relative_url }})]({{ "/figures/overview.png" | relative_url }})

**The corridor.** I built a 12-junction model of the Cawang to Jatinegara corridor in SUMO, a
traffic simulator, from OpenStreetMap. Demand comes from a gravity model, checked against travel
times from TomTom and Google Routes: 13,825 trips over two simulated hours, with about 74%
motorcycles.

[![The corridor map with its twelve junctions, and the nested corridor sizes from one to twelve junctions.]({{ "/figures/corridor_map.png" | relative_url }})]({{ "/figures/corridor_map.png" | relative_url }})

**Nine controllers.** Four classical: fixed-time, coordinated fixed-time, and two versions of
Max-Pressure (the strongest classical method here). Four learned with PPO: independent agents, and
agents sharing a central critic built three ways (MLP, graph attention, graph attention with time).
The ninth, IQL, is a weaker learner kept as a positive control.

**A ladder of sizes.** Every controller runs on 1, 3, 6, 9 and 12 junctions, to see where learning starts to pay.

## How it's built

| Layer | Tools |
|---|---|
| Road network and demand | OpenStreetMap, SUMO, a gravity demand model checked against TomTom and Google Routes |
| Controllers | Python, PPO (independent and shared-critic agents), Max-Pressure |
| Compute | Monash M3 HPC (SLURM), 10 seeds per controller |
| Analysis | Equivalence tests, bootstrap intervals, every paper number re-derived from committed results |

I used a fixed equivalence margin because the question was which levers matter, and a margin set in
advance is what lets "no difference" count as a finding.

## How I knew it worked

- **Ten seeds per controller,** each scored over 20 episodes. Results are robust averages with
  bootstrap intervals.
- **A margin set in advance.** Two controllers count as equivalent when they are within 15 halting
  vehicles of each other, using a formal equivalence test. I fixed that margin before seeing results.
- **A positive control.** IQL had to come out worse, and it did, by 25.7
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

[![Queues by corridor size for each controller type, and the best learned controller against Max-Pressure. The gap leaves the 15-vehicle margin from six junctions and reaches 43.5% at twelve.]({{ "/figures/scale_crossover.png" | relative_url }})]({{ "/figures/scale_crossover.png" | relative_url }})

At twelve junctions the levers rank clearly. The control strategy matters most, at about 143
fewer halting vehicles from about 330. The deployable phase order costs 35 to 38, which is the 19 to 20% in the table. Coordination
through a critic stays inside the margin.

**The limit.** The simulated corridor fails the standard calibration check against real traffic
counts. Its GEH score is 42.88, where the usual acceptance level is 4. So these numbers compare
controllers inside the model. They are not a forecast for the real street.

## What I'd change

- **Calibrate against trajectories** before comparing controllers, so the absolute numbers mean
  something on the street.
- **Add stronger classical baselines,** such as signal timings set by Webster's method and
  vehicle-actuated control, which real cities already run.

## Status

Submitted in September 2026 as my Master of Data Science research paper, and presented as a
poster. Not peer-reviewed. The public repo has the code and every result file; a small 3×3 grid
runs end to end from a clone.
