---
name: knowledge-base
description: "Turns scattered documents into a linked knowledge base in Knowledge/. Triggered by 'изучи документ', 'добавь в базу знаний', '/knowledge-base'."
---

# Skill: /knowledge-base

> Documents, regulations and articles laid out so that the model finds what it needs on its own.

The approach follows [Karpathy's LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f): the base grows gradually, each note is about one thing, notes link to each other.

---

## Where things go

Subfolders are created as needed — they are not in the repository up front.

| What it is | Where | Example |
|---|---|---|
| How a process works here | `Knowledge/Процессы/` | приёмка сырья, согласование дегустации |
| Rules and regulations | `Knowledge/Регламенты/` | требования к упаковке |
| Knowledge from outside | `Knowledge/Справочники/` | как считается срок годности |
| Information about us | `Context/Company/` | not here — that folder has its own files |

---

## What it does

### Step 1. Read the document in full

Not skimming. The job is to understand what here is knowledge and what is formatting.

### Step 2. Split into notes

**One note — one thing.** A forty-page regulation turns not into one file but into eight: one per process it describes. Otherwise the model will read forty pages every time for the sake of one paragraph.

The file name is what is inside it: `приёмка-сырья.md`, not `регламент-2024-итог.md`.

### Step 3. Link them

At the end of every note — a `## Связано` section with links to the neighbouring ones. The links matter more than the content: through them the model reaches what you did not ask about directly.

### Step 4. Update the table of contents

`Knowledge/README.md` — one line per note: its name and what it is about. This is what the model reads first.

---

## Rules

**Do not copy the document wholesale.** A note is what you understood, not what was written. A copy of the source adds nothing but volume.

**Keep the source.** In the note's header — where it came from and when. Six months later this is the only way to tell whether it is out of date.

**Do not mix "as is" and "as it should be".** The current process and the desired one are different notes. Otherwise the model will give advice based on a regulation that does not exist.

**Do not smooth over contradictions.** Found two documents that say different things — write it down that way, with links to both. That is a find, not a problem.

---

## Output

- New notes in the right `Knowledge/` folder.
- An updated table of contents.
- In chat — what was added, and separately: which contradictions were found.
