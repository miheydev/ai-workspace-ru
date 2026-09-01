---
description: "Meeting transcript or recording → summary, decisions, tasks. Triggers: 'разбери встречу', 'сделай саммари планёрки', '/meeting-review'."
language: en
---

# Skill: /meeting-review

> Turns a meeting transcript into three things you can use: what was discussed, what was decided, who does what.

The difference from a plain summary: decisions and tasks are pulled out separately and by name. A summary is read once; tasks keep working.

---

## What you need as input

A transcript file in `Inbox/`. Any format: txt, md, docx, the output of any transcription.

If there is no file — ask where it is.

---

## What it does

### Step 1. Read it in full

Not in pieces. A decision often gets reversed forty minutes after it was made, and a short retelling of the first half will be a lie.

### Step 2. Split it into three layers

**Discussion** — what was talked about, what the positions were. Briefly.

**Decisions** — what was settled. For each decision: what was decided, who decided it, what follows from it. If there was no decision, only a discussion — do not turn one into the other.

**Tasks** — who, what, by when. Without an assignee it is not a task but a wish: mark it as such.

### Step 3. Pull out separately what was left unresolved

Questions that were raised and not closed. A week later this is the most useful part of the file.

---

## Rules

**Write only what was said.** Do not fill in the logic for the participants, do not attribute conclusions nobody drew.

**Quote when the wording matters.** Especially in decisions: «делаем к пятнице» and «постараемся к пятнице» are different things.

**Do not smooth over disagreements.** If two people argued and did not agree — write it that way. A smooth summary of an argument is useless.

**Names as they are in the transcript.** Do not guess job titles and do not make up who is who.

---

## What comes out

One file in `Projects/` of the form `встреча-ГГГГ-ММ-ДД-тема.md`:

```
# Встреча <дата> — <тема>

## О чём говорили
## Решения
## Задачи
| Кто | Что | К какому сроку |
## Осталось нерешённым
```

Plus three lines in chat: how many decisions, how many tasks, what is left open.
