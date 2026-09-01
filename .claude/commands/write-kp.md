---
description: "Builds a commercial proposal from the template and the company context. Triggers: 'подготовь КП', 'составь коммерческое предложение', '/write-kp'."
language: en
---

Write a commercial proposal.

## Context
Use information from:
- `Context/Company/about.md` — our company, products, prices
- `Context/tone-of-voice.md` — communication style
- `Knowledge/` — extra materials about the products

## What is needed
Ask me:
1. Для кого КП? (компания, контакт, сфера)
2. Какой продукт/услугу предлагаем?
3. Есть ли особые условия? (скидка, срок, пакет)

## Result format
- Save to `Projects/` with a clear name (for example: `КП-для-компании-X.md`)
- Structure: greeting → the client's problem → our solution → what's included → price → next step
- Tone: professional but alive (not bureaucratese)
- Length: 1-2 pages
