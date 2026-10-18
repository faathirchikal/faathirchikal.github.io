---
title: Campaign allocation with a discounted bandit
summary: A Thompson-sampling allocator that moved campaign sends toward messages beating their control group. Ran in production until replaced by fixed rules.
status: Replaced in 2026
order: 5
---
Kalbe Farma · Data Scientist · built the allocation logic. Ran in production until a later version
replaced it with fixed rules.

## Problem

A CRM team sends campaign messages to customer segments. Each campaign tests several message
variants, called arms, against a held-back control group. With a fixed even split, a weak arm keeps
receiving a full share of sends long after the data shows it is weak.

The team wanted the split to move toward the arms that convert, without anyone recomputing shares
by hand before each send.

## Constraints

- **Conversion is relative.** Seasonality and promotions move every arm together, so an arm counts
  as good only when it beats its own control group.
- **Behaviour drifts.** What worked last month may not work now, so old results should count for
  less.
- **Arms come and go.** Marketers add new variants mid-campaign, and a new arm has no data.
- **No arm may starve.** Every arm needs enough sends to keep learning.

## Approach

The allocator is Thompson sampling with Beta posteriors, run as a scheduled batch job.

1. **Discounted counts.** Each day's conversions and non-conversions are weighted by 0.99 per day of
   age, so a result from ten weeks ago weighs about half as much as yesterday's.
2. **Posteriors.** Each treatment arm and its control get a Beta posterior with a uniform prior.
3. **Relative reward.** The job draws 10,000 joint samples and scores each arm by the ratio of its
   treatment draw to its control draw.
4. **Shares.** A softmax turns the scores into shares. The temperature tightens as the campaign
   collects more days, so the split explores early and commits later.
5. **New arms and floors.** Each new arm gets a fixed 15% until it has data. Every arm gets a
   minimum share, and the rest is rescaled to sum to one.
6. **Assignment.** Treatment customers are shuffled and cut into blocks by share. Control customers
   stay in control.

![Allocation flow: reward table, discounted Beta posteriors, sampled ratios, softmax shares, floors, customer split]({{ "/diagrams/bandit-flow.svg" | relative_url }})

## How I knew it worked

I did not know well enough. The job computed an interval on each arm's lift over control, but nobody outside
the job saw it, and nothing tested the allocator against known answers. No stakeholder has confirmed
a business result, so this case study claims none. That gap drives most of the list below.

## What I'd change

- **Simulate before launch.** The repository holds no offline simulator, so live traffic was the
  allocator's first real test. A simulator with known true rates would have checked regret and
  recovery from drift before any customer saw a message. The bandit demo on this site is that simulator.
- **Show the uncertainty.** The lift intervals belonged on the CRM team's report, so they could see
  which arms the allocator favoured and how sure it was.
- **Fail loudly.** If the bandit step raised an error, the job fell back to an even split and only
  logged it. A campaign could run unoptimised for weeks with nobody noticing.
- **Tie exploration to evidence.** The temperature followed the number of days of data, but it
  should follow the number of sends, because a quiet arm with many days is still uncertain.
- **Define the exit.** A bandit that has converged can be frozen into fixed rules, and that is a
  reasonable end state. I never wrote down when that should happen or what the fixed rules should
  be, so when the job was later replaced by fixed segments, nobody could say whether learning had
  finished or simply stopped.

## Status

Replaced. A later version keeps the segment selection and sends by fixed rules, with no learned
allocation. The reason for the change is not recorded.
