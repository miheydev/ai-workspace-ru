---
description: "Collects information about your company from open sources and lays it out across the sections of Context/Company/. Triggers: 'обогати контекст', 'собери про нашу компанию', '/enrich-company'."
language: en
---

# Skill: /enrich-company

> Fills the empty sections of `Context/Company/` with whatever could be found about the organization in open sources.

Explain to the model once who you are — after that it knows it in every conversation.

---

## When to run it

- The first time — right after you have deployed the workspace.
- After that — once a quarter, or when something has noticeably changed: a new site, a new product line, a new accounting system.

Takes 15–25 minutes: eight sections, a search for each one, cross-checking.

**The first minutes need a person at the keyboard.** A fresh clone has no permissions granted, so the first web requests will ask for confirmation. Approve them — after that the run goes on its own and there is no need to watch it.

---

## What is needed as input

Ask the user and do not start until you have:

1. **Name of the organization** — as in the registry, in full.
2. **Website** — if there is one.
3. **ИНН** — if known. Raises accuracy a lot: registry data is found by it.
4. **Brands and trademarks** — the names the company is known by from the outside.

The fourth item matters more than it seems. A search by the legal name often returns nothing: press, reviews and social media live under the brand, not under the «ООО».

Do not ask for anything else. Everything else is your job.

---

## What it does — step by step

### Step 1. Recon

Find the basics: the legal entity, year of registration, addresses, type of activity, website, public mentions. This is the foundation for all the other sections.

**Entry point by ИНН — `saby.ru/contragents/<ИНН>`.** Most aggregators do not open by ИНН directly: list-org serves a captcha, rusprofile gives a 404, checko requires ОГРН in the address. Saby serves the card by ИНН and it contains the ОГРН, and with the ОГРН the rest of them open.

### Step 2. Eight sections

For each file in `Context/Company/` collect whatever you find. The files already contain hints in comments — those hints are the spec for the section.

| File | What to look for |
|---|---|
| `about.md` | what it does, who the clients are, what makes it different, legal entities |
| `sites.md` | addresses, what happens where, capacity |
| `products.md` | product categories, key items, seasonality |
| `team.md` | structure, areas of responsibility, how decisions are made |
| `procurement.md` | how the process works, raw material categories, selection criteria |
| `production.md` | cycle, process cards, losses, planning |
| `logistics.md` | order assembly, delivery, documents |
| `systems.md` | which systems, what is stored where, how to get an export |

Sections that cannot be reconstructed from open sources — `procurement`, `production`, `logistics`, `systems` — fill in with what is visible from the outside and **explicitly mark that a person from the inside is needed here**. That is a normal outcome, not a failure.

### Step 3. Cross-checking

Every fact that affects decisions — volumes, sites, legal entities, product composition — confirm with sources of **two different classes**:

- **registry** — ЕГРЮЛ and everything that grows out of it;
- **the company's website** — its own statements;
- **press and industry publications** — the outside view.

Two links to different aggregator sites do not count as independent: checko, saby and «За честный бизнес» are one ЕГРЮЛ in three wrappers. No confirmation — mark as unverified.

### Step 4. Self-check

Go through all eight files again and cross out everything you cannot confirm with a link.

Two things are not crossed out: **«не найдено» entries** and **explicitly marked estimates**. An estimate has no link by definition — it should not have one, it should be labeled as an estimate.

Text written by a person is not crossed out either — anything without the «заполнено агентом» mark was not written by you (see the rule below).

Then write a `## Чего не нашлось` section at the end of each file — as a list.

---

## Rules

**Do not make things up.** If there is no fact — write «не найдено». A plausible invention in the context is worse than emptiness: it will quietly spoil every later answer, and nobody will notice where the error came from.

**Cross-check against the page's source text, not against a retelling of it.** Tools that "read" a page often return a summary — and details that are not on the page appear in the summary. On a run of this skill that is how «две площадки в Екатеринбурге» appeared, even though on the page all the sites were under the «Санкт-Петербург» heading. The error comes not from the model's head but from the intermediate retelling, and it looks completely credible. An important fact — open the source text.

**A website can contradict itself.** Different pages of the same site quite often contain different versions: a different team composition, a different number of sites, different figures. Do not pick "the more recent one" — record the discrepancy with both links. That is a finding, not an obstacle.

**Every fact with a link.** Put the source right next to the statement, not in a general list at the end. In a month nobody will remember where the number came from.

**Distinguish fact from estimate.** «Выручка 200 млн» is a fact with a source. «Компания растёт» is an estimate, and it must be labeled as an estimate.

**Do not rewrite what a person has already filled in.** If the file has text without the «заполнено агентом» mark — add next to it, not on top of it.

---

## What you get

Eight files in `Context/Company/`, each one:

- filled in according to its own hints;
- with links to sources;
- with a `## Чего не нашлось` section at the end;
- with a mark in the header: `> Заполнено агентом <дата>. Проверьте и дополните изнутри.`
- with the same mark on every block you wrote yourself: `<!-- агент, <дата> -->`. Without it, the next run cannot tell your text from a person's and will overwrite theirs.

Plus a short report in the chat: what was found, what was not, where a person is needed.

---

## What this skill does not do

- It does not go into your accounting systems. Open sources only.
- It does not collect data about individuals beyond public positions.
- It does not evaluate the company and does not give recommendations. It collects facts, the conclusions are your job.
