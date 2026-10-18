---
title: A recommender for a consumer loyalty app
question: What should each member see in a six-item product carousel?
summary: A recency-weighted item-item recommender with a fallback for new members. Offline hit rate 18–64% above recent popularity. Live.
status: Live
stack: [Python, BigQuery, Cloud Storage, Docker, GitLab CI]
order: 2
---
Kalbe Farma · 2026 · sole builder of the rebuild, which replaced an older recommender.

## Problem

A consumer loyalty app shows members a six-item product carousel. It had to work for members with a
long purchase history, members with one purchase, and members with none.

## Constraints

- **Three kinds of member.** Frequent, one-purchase and brand-new members need different signals.
- **Health products need care.** Sensitive health and maternity products may only reach members who signalled the need.
- **Variety matters.** A carousel of one brand in four pack sizes wastes five of six slots.

## Approach

1. **Item-item model.** A co-purchase model (EASE family) on a purchase matrix where recent
   purchases count more (180-day half-life), plus product-description embeddings.
2. **New members.** They see what members of the same gender and province bought recently.
3. **Rules after ranking.** At most two items per product line and per brand, one pack size per
   product, nothing from an earlier life stage than the member has reached, and the sensitive-product gate.

## How it's built

| Layer | Tools |
|---|---|
| Data | Purchase history from BigQuery |
| Model | Python, item-item co-purchase model (EASE), text embeddings |
| Delivery | Weekly GitLab CI schedule runs a Docker image that writes the top six per member to Cloud Storage, where the app reads it |
| Safety | A publish gate refuses to overwrite the live file when a check fails |

I chose an item-item model because it ranks every member in one weekly batch, its picks read as
"often bought with", and it scored within the margin I set of the most accurate candidate.

## How I knew it worked

Offline, on a time split: train before a cut-off, then check the next 90 days, with every in-stock
item ranked and 20,000 customers per segment. The metric is hit rate@6, meaning the six-item
carousel holds something the member later bought.

| Segment | Model | Recent popularity |
|---|---|---|
| Frequent members | 0.446 | 0.273 (+64%) |
| One-purchase members | 0.301 | 0.255 (+18%) |
| New members (segment popularity) | 0.307 | 0.108 global popularity (+184%) |

A second, untouched cut-off gave the same picture. Among the candidates, the shipped model was 4.5%
behind the most accurate one, inside the margin I fixed before the results, and it raised the
worst-segment catalogue coverage from 0.511 to 0.774.

These are offline numbers. No A/B test has run, so there is no business result yet.

## What I'd change

- **Log impressions from day one,** so a holdback test can measure the business effect.

## Status

Live since 30 Sep 2026, refreshed weekly.
