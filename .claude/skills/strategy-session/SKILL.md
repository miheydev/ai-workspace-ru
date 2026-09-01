---
name: strategy-session
description: "The model asks you questions about your company context and helps you see the bottlenecks and outline a plan. Triggered by 'проведи стратсессию', 'задай мне вопросы про компанию', '/strategy-session'."
---

# Skill: /strategy-session

> A conversation in reverse: you don't ask the model, it asks you.

Works only when `Context/Company/` is already filled in. The point is that the model sees your company as a whole — all eight sections at once — while you see your own patch of it every day. The questions come up at the seams.

---

## What you need as input

A filled-in `Context/Company/`. At least four sections out of eight.

Fewer than four — say so and offer `/enrich-company` first. Do not start the session: questions asked against an empty context come out generic, and that is exactly what makes the format useless.

Ask what we're focusing on: a direction, a specific problem, or a free-form review. If there's no answer — take free-form.

---

## What it does

### Step 1. Read the entire company context

All the files, in full. From there on you work only from them — inventing nothing and assuming nothing beyond what is written.

### Step 2. Find where things don't add up

Look for three things, in exactly this order:

**Contradictions between sections.** Production says one thing, shipping another. This is the most valuable one: people from different departments don't see it, because each of them reads their own file.

**Holes.** A process is described end to end, but one step is missing. That's usually where it hurts.

**Disproportions.** A lot of attention to something small and one line about something big.

### Step 3. Ask questions. One at a time

**Don't hand over a list.** One question — an answer — the next question, which takes that answer into account. A list of fifteen questions is something a person scrolls past and answers none of.

The question has to be one they can answer on the spot. Not «как вы видите стратегию развития», but «у вас в закупках написано, что заявка формируется под план производства, а в производстве — что план верстается по факту заказов. Что из этого первично?».

Keep in mind: **a question is worth asking only if something changes depending on the answer.** Everything else is small talk.

Five to seven questions are enough. After that attention runs out.

### Step 4. Assemble the result

After the conversation — a file. Not a retelling of the conversation, but three things:

- **Узкие места** — what's in the way, in descending order of what it costs. Each one with a reference to where it can be seen from.
- **Что чинится быстро** — what can be done in a week by one person.
- **Что требует решения** — the forks where a choice is needed, not work.

---

## Rules

**Don't advise before you've asked.** The temptation to hand over a plan after the first file is strong, and the plan will be wrong: the context describes how things are, not why they came to be that way. The reasons come only from the answers.

**Don't pretend you know the industry better than the person you're talking to.** You see the connections between files, he sees production. That's different knowledge, and yours is the weaker one.

**Separate "strange" from "wrong".** Much of what looks illogical from the outside has a reason on the inside. Ask for the reason before calling it a problem.

**Don't turn the conversation into an interrogation.** If a person answers «не знаю» to a question — that's also an answer, and often the most interesting one. Write it down and move on.

---

## What comes out

A file in `Projects/` named `разбор-<направление>-<дата>.md` with the three sections above.

Plus one line in chat: what turned out to be the biggest bottleneck.
