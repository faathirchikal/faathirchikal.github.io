# Demo: campaign bandit simulator

status: draft · locked: none

A single page that runs in the browser in plain JavaScript, with no build step. It simulates a
campaign with message arms of known true conversion rates and compares allocation policies live.
Built second.

## Criteria

1. The reader sets the true rate for each arm and the control, the daily send volume, the drift and
   the number of days, then presses run.
2. Four policies run side by side on the same random seed: an even split, epsilon-greedy, Thompson
   sampling, and discounted Thompson sampling with lift over control.
3. Charts show the share per arm over time, the cumulative regret, and the posterior for each arm on
   the current day.
4. A drift preset swaps the best arm halfway through. Only the discounted policy recovers quickly, and
   the reader can see that on the share chart.
5. The posterior update is unit-tested against hand-computed Beta parameters, including unequal
   control and treatment sizes.
6. The page is one HTML file plus one script, deployable to GitHub Pages, and runs at phone width.

## Operator brief

**Simulator.** Controls sit in a left panel, or at the top on a phone. There are three charts:
shares as a stacked area, regret as lines per policy, and posteriors as density curves. A seed field
makes a run repeatable, and a short note under each chart says what to look for.

## Check

- Open the file locally. The drift preset produces the recovery described in criterion 4.
- The test page shows all posterior tests passing.
