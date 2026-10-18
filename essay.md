---
title: Knowing whether it works
summary: Two projects where measurement was set up well and three where it was not, and the rule I took from them.
permalink: /essay/
---
Picking the method is the part of data science people like to talk about. In my experience it is
the easier half. The harder half is knowing whether the thing works, and noticing when it has
stopped working or never did.

Here are two projects where I set up the measurement well and three where I did not. The failures taught me more,
so they get more room.

## When the framing does the work

One planner I built sends a radioactive medical product to hospitals. It loses half its activity in
under two hours, so routes, departure times, fleet size and dose all depend on each other.

What untangled it was noticing they connect through one number, the departure time. Fix it, and
decay is a formula you can invert by hand. The tangled problem becomes a sweep over departure times
and fleet sizes, each cell an ordinary routing problem for an off-the-shelf solver.

That needed no clever algorithm, only time with the planners' spreadsheet to see which variable
everything hung from. When a method feels heavy, I now ask first whether the framing is right.

## Deciding what "works" means before building

The second is a platform where business teams get an LLM agent that answers questions from the data
warehouse. A demo that answers five questions well tells you almost nothing about the sixth.

So before the platform existed, I asked the analysts who own the dashboards for an answer key:
questions with the answers their dashboards give. A figure passes within half a percent, a question
that should be refused passes only when refused, and one row shown to the wrong user fails the run.

That key now decides go-live, when the older agent retires, and whether a new model is adopted.
The platform is not live yet: go-live waits on the signed answer key, which is the point of having one.

## Three that did not hold

### The study with no control

We were asked whether in-person promotional events lifted sales at nearby shops. The first version
reported the gap between a sales forecast with and without an event as the uplift. That was the
model compared with itself. The events also ran nationwide on the same dates, so no shop was
unexposed, and the effect could not be separated from the calendar.

The follow-up used geographic controls, placebo dates and permutation tests. It found a small,
delayed, local signal that is still only a correlation. A causal number needs a randomised holdout,
and that is the business's decision to make.

**What I do now.** Before modelling an effect, I name the comparison group. If there isn't one, the
deliverable is an experiment design, and I say so at the start.

### The trial that improved the wrong number

A warehouse study grouped picking tasks by location so pickers walked less. In a one-branch trial,
picking got faster. Then on-time dispatch got worse: grouping needs several released orders at once,
so orders waited before anyone picked them. Each pick was quicker, but orders left later, and on
time is what the warehouse is judged on. It also needed a hand-built floor map that only the trial
branch had.

**What I do now.** I measure what the operation is judged on, including any waiting my method adds,
and I list every input the method needs per site before the first trial.

### The bandit with no exit condition

I built a Thompson-sampling bandit that shifted campaign sends toward message variants beating their
control group. It ran in production until it was replaced by fixed rules. That can be a fine end
state for a converged bandit, but I had never written down when learning should end, and never
tested it against known answers before launch.

**What I do now.** An adaptive system ships with an exit condition, a fallback, and a simulator that
shows it recovering the right answer on synthetic data.

## What ties these together

Each failure was a measurement problem: no comparison group, a local metric where the
system's mattered, no definition of done. Each success was a measurement decision: the variable that made
the problem easy, and a definition of correct fixed before any code existed.

So my rule is to decide how I will know before deciding what to build: the comparison group, the
metric the business is judged on, the answer key, and the condition for switching it off.
