---
title: Knowing whether it works
summary: Two projects where measurement went right and three where it did not, and the rule I took from them.
permalink: /essay/
---
Picking the method is the part of data science people like to talk about. A solver, a model, an
LLM agent. In my experience it is the easier half of the job. The harder half is knowing whether the
thing works, and noticing when it has stopped working or never did.

Here are two projects where that went well and three where it did not, from several years of
applied work at a pharmaceutical company. The failures taught me more, so they get more room.

## When the framing does the work

One planner I built sends a radioactive medical product to hospitals. It loses half its activity in
under two hours, so every minute on the road raises the dose that has to be made. At first it looks
like one tangled problem: routes, departure times, fleet size and dose all depend on each other.

What untangled it was noticing that they connect through one number, the departure time. Fix the
departure, and decay is a formula you can invert by hand. The tangled problem becomes a sweep over
departure times and fleet sizes, and each cell is an ordinary routing problem that an off-the-shelf
solver handles in about a second.

None of that needed a clever algorithm. It needed someone to sit with the planners' spreadsheet long
enough to see which variable everything else hung from. When a method feels heavy, I now ask first
whether I have framed the problem right.

## Deciding what "works" means before building

The second success is an internal platform where business teams get an LLM agent that answers
questions from the data warehouse. LLM demos are easy to like. A demo that answers five questions
well tells you almost nothing about the sixth.

So before the platform existed, I asked the analysts who own the dashboards to write an answer key of
questions, each with the answer their dashboard gives. An answer passes when its numbers match within
half a percent. A question the agent should refuse passes only when it refuses. One row shown to a
user who may not see it fails the whole run. Each case runs three times, and part of the set is held
back from tuning.

That answer key now decides everything. The platform goes live when it clears the bar. The older
hand-built agent retires when the platform scores at least as well on the same questions. A new model
or framework has to pass the same test, so nobody switches technology on impressions.

The platform is not live yet, because the analysts have not signed off their answers. A bar checked against unsigned answers is a bar in name only.

## Three that did not hold

### The study with no control

We were asked whether in-person promotional events lifted sales at the shops near them. The first
version matched events to nearby shops, trained a sales forecast, and reported the gap between the
forecast with an event and without one as the uplift.

That gap was the model compared with itself. Nothing checked it against shops that had no event.
When we came back to it, we found the events ran nationwide on the same dates, so every candidate
comparison shop had been exposed too. The effect could not be separated from the calendar.

The follow-up treated it as an experiment. It used geographic controls, placebo dates and
permutation tests, and logged every attempt, including the ones that found nothing. It found a
small, delayed, local signal that is still only a correlation. A causal number needs a randomised
holdout, which means the business choosing to skip events in some areas, and that decision belongs
to them.

**What I do now.** Before modelling an effect, I write down what the comparison group is. If there
isn't one, the deliverable is the design of an experiment, and I say so at the start.

### The trial that improved the wrong number

A warehouse study grouped picking tasks by location so pickers walked less. In a live trial at one
branch, picking got faster.

Then on-time dispatch got worse. The warehouse system reveals where an item sits only when the order
is released, and grouping needs several released orders at once. So orders waited in a short
grouping window before anyone picked them. Each pick was quicker, but the orders left later, and on
time is what the warehouse is measured on.

It also needed a hand-built floor map, and only the trial branch had one. Even a successful version
could not have spread to the other branches. Both lessons shaped the project that followed. It tries to
derive the layout from transaction data, and it judges every change, order release included, by
total picker minutes across the day.

**What I do now.** I ask what the operation is judged on, and I measure that, including any waiting my
method adds. I also list every input the method needs per site before the first trial.

### The bandit nobody switched off on purpose

For a CRM team I built a Thompson-sampling bandit that shifted campaign sends toward the message
variants that beat their control group, discounting old results so it could follow drift. It ran in
production until a later version replaced it with fixed rules. I don't know why. The
variants may simply have converged, and freezing a converged bandit into rules is a reasonable thing
to do.

What bothers me is that nobody could say. I never wrote down when learning should end, what the
frozen rules should be, or what would justify turning it back on. I also never tested it
against known answers. An offline simulator with known true rates would have shown whether it found
the best variant before any customer saw a message.

**What I do now.** An adaptive system ships with an exit condition and a fallback written down, and
with a simulator that shows it recovering the right answer on synthetic data.

## What ties these together

Each failure was a measurement problem. The study had no comparison group, the warehouse trial
measured a local number instead of the system's, and the bandit had no definition of done. The
successes were measurement decisions too: one picked the variable that made the problem easy, and the
other fixed the definition of correct before any code existed.

So my working rule is to decide how I will know before deciding what to build. I write down the
comparison group, the metric the business is judged on, the answer key, and the condition for
switching it off. It is less fun than choosing a model, and it is the part that decides whether the
model mattered.
