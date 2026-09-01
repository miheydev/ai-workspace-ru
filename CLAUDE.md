---
language: en
---

# Company Workspace

> Main instruction file. Claude Code reads it first on every start, before answering anything.
> Workspace still empty? Say: «помоги настроить рабочее пространство под нас».

## Quick Start

**Who we are and how we work** → [Context/Company/](Context/Company/) — eight sections, one per area
**How we talk to customers** → [Context/tone-of-voice.md](Context/tone-of-voice.md)
**Work already done** → [Projects/](Projects/)
**Documents about the business** → [Knowledge/README.md](Knowledge/README.md)
**Document templates** → [Templates/README.md](Templates/README.md)
**Files to process** → [Inbox/](Inbox/)

## Where to look for what

**Before any task, decide which context it needs and read it.** Never guess about the company — it is written down.

| Task | Read first |
|---|---|
| Any document for a customer | [about.md](Context/Company/about.md) + [tone-of-voice.md](Context/tone-of-voice.md) + a template from [Templates/](Templates/) |
| Purchasing, suppliers, raw materials | [procurement.md](Context/Company/procurement.md) |
| Production, process cards, losses | [production.md](Context/Company/production.md) |
| Shipping, delivery, paperwork | [logistics.md](Context/Company/logistics.md) |
| Product range and sales | [products.md](Context/Company/products.md) |
| Numbers, reports, exports | [systems.md](Context/Company/systems.md) — it says where things live |
| Who is responsible for what | [team.md](Context/Company/team.md) |
| Sites and capacity | [sites.md](Context/Company/sites.md) |
| How our process works | [Knowledge/](Knowledge/) |

**The context you need does not exist — do not invent it.** Say which section is missing and offer to fill it.

## Structure

```
Context/
  Company/          — who we are: eight sections, one per area
  tone-of-voice.md  — how we talk to customers
Projects/           — results of work (proposals, letters, analyses, posts)
Knowledge/          — documents about the business (services, procedures, cases)
Templates/          — document templates with placeholders
Inbox/              — files to process (drop a file → ask to sort it out)
.claude/
  commands/         — commands, invoked with a slash
  skills/           — skills: a folder with SKILL.md inside
```

## Inbox

`Inbox/` is a **temporary** folder for files waiting to be processed. Drop a file there and say «разбери входящие».

**After working with a file from Inbox — remove it.** Decide where it belongs (`Knowledge/`, `Projects/`, a section of `Context/Company/`), **move** it there (not copy), and update whatever it affects. Never put a processed file back — Inbox is a queue, not an archive.

**Do NOT check Inbox automatically at session start** — it wastes context. Open it when asked.

## File link format

**Render every file reference in chat as a clickable markdown link** — never paste a bare path.
- **Why:** bare paths are not clickable in the chat UI; the reader cannot open them.
- **How:** `[name](path)`, path relative to the folder the session started in. Add `:line` for a specific line.
- **URL-encode spaces as `%20`** in the href. Keep the spaces in the visible `[name]`.

---

## Critical rules (MANDATORY)

### 0. Git commits without Co-Authored-By

**DO NOT add** the `Co-Authored-By` line to commit messages. No mentions of Claude, Opus, Anthropic in commits.

### 1. Read before editing

**NEVER** overwrite a file without reading it first.
- First `Read` → understand the content → then `Edit` or `Write`
- Exception: creating a new file
- **Why:** the file may hold a colleague's edits that are not in your history. A blind overwrite erases their work silently.

### 2. Artifact creation: Write-First

**Principle:** If the result is worth more than one session — write it to a file, don't keep it in chat.

**Create WITHOUT asking:**
- Document for a customer → file in `Projects/`
- Meeting processed → summary + update to the affected context section
- Analysis/research → result in `Knowledge/`
- README for navigation in new folders

**DO NOT create:**
- Wrappers, utilities, abstractions "for the future"
- Documentation for something that hasn't changed
- Duplicate files

**Scratchpad rule:** If an analysis takes >1 screen of text — write the result to a file first, then give a brief summary in chat.

### 3. Security

- **DO NOT** commit `.env`, credentials, API keys
- **DO NOT** output secrets in output — give the path to the file instead of its contents
- Check `.gitignore` before committing
- If a secret is found in a file → report it immediately
- **Why:** what reaches git stays in history even after the file is deleted

### 4. Facts about the company come from a source

**Numbers, dates, names, customer names — ONLY from `Context/`, `Knowledge/`, or from a person.**
- No source → omit the claim and say what is missing. A gap beats plausible fiction.
- **Why:** the document goes out under the company's name. One invented figure costs more than ten blanks — and the customer is the one who finds it.

### 5. Text generation: no AI boilerplate

- Follow [Context/tone-of-voice.md](Context/tone-of-voice.md) when it is filled in
- **DO NOT** use: negative parallelism ("не X, а Y"), rhetorical self-answering ("Результат? Разрушительный."), filler openers ("стоит отметить", "в современных реалиях"), bureaucratic language
- **DO NOT** over-polish to the point of losing the voice — structure and remove repetition, but keep the speaker's own words. Conversational phrases are not filler; they are how the person talks.

### 6. Self-check before "done"

**For tasks with >1 item** — before saying "done", go through the list of what was requested and mark the status of each item. Only then give a summary in chat.
- **How:** 1) list of what was requested, 2) status of each item (done / skipped / unclear), 3) complete skipped items, 4) **only then** write the summary
- **When NOT needed:** for single-step tasks (one file, one command) — overkill
- **Why:** without the list it is easy to close three items out of five and say "done". The human ends up catching it.

### 7. Ask before anything irreversible

- **DO NOT** delete files without confirmation
- **DO NOT** overwrite someone else's edits
- Big task → show the plan first, then do it
- **Why:** asking costs ten seconds, restoring costs half a day

---

## Context management

The model carries the whole conversation. When it grows too long, quality drops before anything breaks.

| Command | When to use |
|---------|-------------|
| `/context` | Check how much context is currently used |
| `/compact` | Compress history into a summary — task not finished, but context has grown large |
| `/clear` | Full reset when switching to an unrelated task |

- **Maximum 2–3 tasks per session.** More than that — say so and suggest splitting.
- **Delegate heavy reading to subagents.** Exploring files, searching, reading long transcripts — offload it. The subagent burns its own context and returns a short answer.
- **Read files economically** — don't read a whole file for a few lines, don't re-read what you already read this session.
- **After completing each task** — write the result to a file. If the session dies, the work survives.

Full detail → [.claude/skills/context-management/SKILL.md](.claude/skills/context-management/SKILL.md)

## Where this repository lives

It is public and it is for learning: everything committed is visible to everyone. That is why `.gitignore` keeps exports and spreadsheets out of git by default — xlsx, csv, pdf, docx and everything under `Inbox/`. The files still sit in the folder and are read normally; they just don't travel into history.

When the repository becomes ours — private on GitHub, on our own server, or inside the organization — we change these rules. What lives next to the context and what lives apart is the owner's call.

**Claude: if you don't know who can see this repository, ask before putting anything sensitive into it.**

## Sharing within the team

**One section, one owner.** Purchasing is described by the buyer, production by the production lead, shipping by the logistics lead. Overlaps don't arise because they don't exist by construction.

---

## Adding your own skill

1. Create a folder in `.claude/skills/` — for example `my-task/`
2. Inside it, a file `SKILL.md`: what to do, where to take context from, what the result looks like
3. Restart Claude Code — `/my-task` appears in the list

**The folder is required.** A plain file `.claude/skills/my-task.md` does not become a command — Claude Code looks for `SKILL.md` inside a folder. Files in `.claude/commands/*.md` do become commands directly.

## What to use

Just say the task in your own words — Claude will pick the right one. The list is not exhaustive; the rest is in [.claude/commands/](.claude/commands/) and [.claude/skills/](.claude/skills/), and the files are readable.

| What you need | Command |
|---|---|
| Fill in the company context | `/enrich-company` |
| Process a meeting | `/meeting-review` |
| Build a document from a template | `/create-document` |
| File a document into the knowledge base | `/knowledge-base` |
| Find weak spots by being asked questions | `/strategy-session` |
| Turn a repeatable piece of work into a skill | `/create-skill` |
| Set the workspace up for yourself | `/setup` |

---

## Response language

- **Talk to the user in Russian.** These instructions are in English to save context; the conversation is not.
- Documents, letters, proposals — Russian, unless asked otherwise
- Technical terms in English are fine
- Instruction files (this file, skills, commands) — English
- Company context, templates, README — Russian, because people read and fill them
