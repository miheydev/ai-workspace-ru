---
description: "Builds a commercial proposal from the template and the company context. Triggers: 'подготовь КП', 'составь коммерческое предложение', '/write-kp'."
language: en
---

Write a commercial proposal.

## Context
Use information from:
- `Templates/proposal.md` — the structure of the proposal, take it from here
- `Context/Company/products.md` — the range, packaging, shelf life, prices
- `Context/Company/logistics.md` — shipping, delivery, minimum batch
- `Context/Company/about.md` — the company and how we differ
- `Context/tone-of-voice.md` — communication style
- `Knowledge/` — extra materials about the products

`products.md` is empty — say so. A proposal without a range and prices is a letter, not a proposal.

## What is needed
Ask me:
1. Для кого КП? (компания, контакт, кто они — сеть, ресторан, дистрибьютор, розница)
2. Какие позиции предлагаем? (или «подбери сам под их профиль»)
3. Есть ли особые условия? (цена от объёма, отсрочка, пробная партия)

## Result format
- Save to `Projects/` with a clear name (for example: `КП-для-компании-X.md`)
- Structure — from `Templates/proposal.md`, do not invent your own
- **Do not invent prices, shelf life or packaging.** No figure in the context — leave the placeholder and say what is missing. A wrong price in a proposal is a commitment
- Tone: professional but alive (not bureaucratese)
- Length: one page, plus the range table
