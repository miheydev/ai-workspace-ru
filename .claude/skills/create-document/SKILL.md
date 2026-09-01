---
name: create-document
description: "Builds a .docx from a template in Templates/ — акты, спецификации, отчёты. Triggers: 'сделай документ', 'подготовь акт', 'собери отчёт в docx', '/create-document'."
---

# Skill: /create-document

> A ready .docx from your template, with the data filled in.

The point is the template: describe the structure once — after that a document is built in seconds, and every document looks the same.

---

## What you need as input

1. **Which document** — акт, specification, report, letter.
2. **Template** — a file from `Templates/`. If there is no suitable one, offer to create it and show the structure before building anything.
3. **Data** — where to take it from: a file in `Inbox/`, a table, or the user dictates it.

---

## What it does

### Step 1. Read the template

Find the placeholders in it — spots like `[название]`, `{{дата}}`, `___`. Collect the list of them.

### Step 2. Ask only for what is missing

Whatever you can take from `Context/Company/` — take it from there, don't ask. Реквизиты, addresses, the name are already there.
Ask in one list, not one question at a time.

### Step 3. Build the file

Via `python-docx`. Layout rules:

- One font for the whole document, size 11.
- Headings — with the `Heading 1` / `Heading 2` styles, not manually bolded text.
- Tables — with the `Table Grid` style, header row in bold.
- Align numbers in tables to the right.
- No empty paragraphs for spacing — use spacing settings.

If `python-docx` is not installed — say so and offer to install it: `pip install python-docx`. Don't silently substitute a markdown file for the result.

### Step 4. Check before handing it over

Open the built file and make sure: not a single placeholder is left unfilled, the numbers match the source, the dates are in one format.

---

## Rules

**Don't invent реквизиты.** No data — leave the placeholder and say what is missing. A document with a made-up ИНН is more dangerous than an unfilled one.

**Calculate in the source, not in your head.** If the document has amounts — take them from the table or calculate them explicitly, showing the calculation.

**Don't change the template's wording.** The template has been agreed on. Your job is to substitute data, not to improve the text.

---

## What you get

A file in `Projects/`, named so that it is still clear a month later: `акт-<контрагент>-<дата>.docx`.

In chat — the path to the file and a list of what is left unfilled.
