---
name: context-management
description: "How not to bloat context in a long session and what to do when you hit the limit. Load during long work or on 'слишком длинный запрос'."
---

# Skill: /context-management

> The model remembers everything that happened in the conversation. When the conversation gets too long, it hits the ceiling and loses the beginning.

This is not a breakage, it is the physics of the tool. Managing it is simple if you know three commands.

---

## Three commands

| Command | What it does | When |
|---|---|---|
| `/compact` | Compresses the conversation into a summary and continues with it | You finished a big task and are starting the next one |
| `/clear` | Wipes the conversation completely | You are switching to another topic |
| `/context` | Shows how much is taken up | When it seems the model has started thinking worse |

---

## The rules that save the most

**One session — one or two tasks.** A third task in the same conversation always goes worse than the first two: the model drags along everything that came before and gets confused.

**Result goes into a file, not into the chat.** A long answer in the chat takes up room in the conversation forever. The same answer in a file takes up one line — the path to it. As a bonus, you can find it a month later.

**Do not ask to re-read what has already been read.** The model remembers it. Re-reading a large file is the most frequent reason a conversation ends earlier than it should.

**A large file — measure it first.** Ask how many lines it has and read the part you need. You read in full what you are going to rework, not what you are just looking into.

**Switched to another topic — `/clear`.** The context of the previous task only gets in the way of the new one: the model will drag someone else's details into it.

---

## Signs that it is time to compact

- The model started asking again about things you agreed on an hour ago.
- Answers became generic where they used to be precise.
- References appeared to files that do not exist.

Any one of the three is a reason to run `/compact` without waiting for an error.

---

## What to do when you already hit the wall

1. Do not start over — ask for the current state to be written into a file.
2. `/clear`.
3. The new session starts by reading that file.

You lose the conversation, but not the work. That is exactly why the result is always written into files.
