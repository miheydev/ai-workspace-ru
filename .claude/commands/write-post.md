---
description: "Writes a post for social media with per-platform rules. Triggers: 'напиши пост', 'сделай пост для телеграма', '/write-post'."
language: en
---

Write a social media post.

## Context
Use:
- `Context/Company/about.md` — about the company
- `Context/tone-of-voice.md` — tone and style

## What is needed
Ask me:
1. Тема поста?
2. Для какой площадки? (Telegram / VK / LinkedIn / Instagram)
3. Цель? (экспертность / продажа / вовлечение / новость)

## Rules per platform
- **Telegram:** up to 1500 characters, paragraphs, emoji in moderation, no hashtags
- **VK:** up to 2000 characters, can be longer, hashtags at the end
- **LinkedIn:** professional tone, 1000-1500 characters, storytelling
- **Instagram:** up to 2200 characters, hashtags (10-15), CTA at the end

## Format
- Save to `Projects/` with a name like `пост-тг-тема-дата.md`
- Show a short preview in chat
