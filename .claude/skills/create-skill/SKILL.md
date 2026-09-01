---
description: "Turns a repeatable piece of your work into a reusable skill in .claude/skills/. Triggers: 'сделай из этого скилл', 'упакуй это в скилл', 'опиши этот процесс скиллом', '/create-skill'."
language: en
---

# Skill: /create-skill

> The work you explain to a new colleague every time — written down once, so you never explain it again.

A skill is not a clever prompt. It is your instruction to an employee, saved as a file. This skill writes that file for you: it asks about the work, you answer in your own words, it assembles the folder.

---

## Two ways in

**You already have it written down.** A regulation, an instruction, a checklist, a description of the process — put the file in `Inbox/` and say «сделай из этого скилл». Read the file first, then ask only about what is missing from it.

**You just did the work.** The result came out well and you want it repeatable — say «упакуй то, что мы сейчас сделали, в скилл». Reconstruct the steps from what actually happened in this session, then confirm them with the user before writing anything.

---

## What you need as input

Ask, and do not start until you have it. Ask one question at a time, not a list — the user is describing their own work, not filling in a form.

1. **When it is used** — what happens right before someone needs this. This becomes the `description`, and by it the model decides whether the skill applies at all.
2. **What is needed at the start** — what you would ask a colleague before they could begin.
3. **The steps** — in the order they actually happen. If the user says «ну там понятно», ask them to walk through the last real case out loud.
4. **What must never happen** — the boundaries. Ask directly: «что здесь можно испортить?» This is the shortest part and the most valuable.
5. **What a finished result looks like** — how they know the work is done and correct.

Point 5 gets skipped most often, and its absence is what makes a skill produce plausible-looking but useless output.

---

## What it does — step by step

### Step 1. Draft out loud

Before creating any files, show the user the skeleton in the chat: name, description, inputs, steps, rules. Two or three sentences per part. Let them correct it — this is much cheaper than rewriting the file afterwards.

### Step 2. Choose a name

An English folder name, a verb where possible: `create-invoice`, `check-supplier`, `review-spec`. It becomes the command the user types, so it should be readable and short.

Check `.claude/skills/` first: if a skill for this work already exists, propose improving it instead of creating a second one.

### Step 3. Write the file

Create `.claude/skills/<name>/SKILL.md` with frontmatter (`description`, `language`) and the sections below. Follow the shape of the skills already in this repository — open one and match it.

The `description` must contain the triggers: the words a person would actually say when they need this work done. Without them the model never reaches for the skill.

### Step 4. Rules — the part that matters most

Every skill needs a rules section, and it must be concrete. «Не выдумывай, нет факта — пиши "не найдено"» is worth more than a page of instructions. Write the rules from the answer to question 4 above.

Where the work touches numbers, prices, cost or personal data, say explicitly what may not be sent outside and what may not be invented.

### Step 5. Try it once

Offer to run the new skill on a real case immediately. A skill that has never run once is a draft, not a skill. After the run, fix what turned out to be missing — usually it is an input nobody thought to ask for.

---

## Rules

**Do not write it for them.** The value of a skill is that it holds *their* way of working, not a generic one. If an answer is vague, ask again rather than filling the gap with something plausible.

**Do not invent steps that were not named.** A step that appears in the file but not in the user's description will be executed as if it were theirs.

**Keep it short.** A skill nobody rereads is worthless. Steps in the order they happen, rules in one line each, no introductions.

**One skill — one job.** If the description needs the word «и» twice, it is two skills.

---

## Where to find ready-made ones

Anthropic keeps a public library of skills and templates: [github.com/anthropics/skills](https://github.com/anthropics/skills). They are brought here the same way as anything else — ask the model to go and copy what you need. Useful as examples of how a skill is put together; the valuable ones are still your own.
