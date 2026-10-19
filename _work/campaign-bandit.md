---
title: Campaign allocation with a discounted bandit
summary: A Thompson-sampling allocator that moved campaign sends toward messages beating their control group.
status: Deployed
question: How should campaign sends shift toward the messages that work, while behavior drifts?
stack: [Python, Thompson sampling, Docker, GitLab CI]
order: 6
---
Kalbe Farma · 2024–2026 · built the allocation logic.

## Problem

A CRM team sends campaign messages to customer segments, testing several message variants (arms)
against a held-back control group. With a fixed even split, a weak arm keeps a full share of sends
long after the data shows it is weak. The team wanted the split to move toward arms that convert,
without recomputing shares by hand.

## Constraints

- **Conversion is relative.** Promotions move every arm together, so an arm is good only when it beats its control.
- **Behavior drifts,** so old results should count for less.
- **Arms come and go,** and a new arm has no data.
- **No arm may starve.** Every arm needs enough sends to keep learning.

## Approach

Thompson sampling with Beta posteriors, run as a scheduled batch job. This is the method from my
[published bandit research](https://doi.org/10.1088/1742-6596/1722/1/012096), applied to a live problem.

1. **Discounted counts.** Each day's outcomes are weighted by 0.99 per day of age.
2. **Relative reward.** Each arm and its control get a Beta posterior; joint samples score each arm
   by the ratio of its draw to its control's.
3. **Shares.** A softmax turns scores into shares, tightening as the campaign collects data, so the
   split explores early and commits later.
4. **New arms and floors.** A new arm gets a fixed 15% until it has data, and every arm keeps a minimum share.


## How it's built

| Layer | Tools |
|---|---|
| Inputs | Daily sends and conversions per arm and control group |
| Engine | Python, discounted Beta posteriors, Thompson sampling |
| Delivery | A scheduled GitLab CI job runs a Docker image: query results, compute shares, push the customer split to the CRM platform's API |

I used discounted Thompson sampling because it handles drift with one parameter, and it is the
method I had already tested in my published research.

## How I knew it worked

Not well enough. The job computed an interval on each arm's lift over control, but nothing tested
the allocator against known answers, and no business result has been confirmed.

## What I'd change

- **Simulate before launch.** A simulator with known true rates checks regret and recovery from
  drift before any customer sees a message. The [bandit simulator](https://faathirchikal.github.io/campaign-bandit-simulator/) shows that kind of test on synthetic data.
- **Show the uncertainty** to the CRM team, so they can see which arms are favored and how surely.
- **Define the exit.** Freezing a converged bandit into fixed rules is a reasonable end state, but
  the condition for it should be written down at launch.


## Status

Built and deployed as a daily batch job for the CRM team.
