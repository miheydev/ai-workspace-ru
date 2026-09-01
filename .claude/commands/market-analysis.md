---
description: "Analysis of the market and competitors for the company niche: size, trends, players, pricing, positioning. Triggers: 'проанализируй рынок', 'посмотри конкурентов', '/market-analysis'."
language: en
---

Run a market analysis for my business.

## Context
Use:
- `Context/Company/about.md` — company info, clients, competitors
- `Context/Company/products.md` — what we sell and to whom
- `Knowledge/` — additional materials

`about.md` is still empty — say so and offer to run `/enrich-company` first. Do not invent what the company does.

## What to do

### 1. Market overview
- Market size and dynamics
- Key trends (what is growing, what is declining)
- Key players

### 2. Competitor analysis
- Strengths and weaknesses of each
- Pricing
- Positioning

### 3. Our opportunities
- Unoccupied niches
- Where we are stronger than competitors
- Quick wins — what can be done quickly

### 4. Recommendations
- Top 3 growth priorities
- What to try over the next month

## Important rules

1. **Always use WebSearch.** Market size, trends, players and prices are searched for, not recalled.
2. **Freshness.** Add the current year to the query, otherwise you get data from three years ago.
3. **A source next to every number.** No source — write «не найдено», do not estimate silently.
4. **Honesty.** Little data on a question — say so, do not fill the gap with plausible text.

## Format
- Save to `Knowledge/анализ-рынка-[дата].md`
