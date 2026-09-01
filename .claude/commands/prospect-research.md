---
description: Research a prospective client — company profile, problem hypotheses, solution proposals (5 parallel agents)
---

# Prospect Research

You are the orchestrator of prospective-client research. Your task is to assemble a full company profile from open data, surface problem hypotheses and form solution proposals with a sellability score.

---

## INPUT

Extract from $ARGUMENTS:
- **Company name** (required)
- **Website URL** (required)
- **Context** (optional): who introduced us, what we already know, who the contact is

If the name or the URL is missing — ask the user.

---

## PROCESS

### Step 1: Surface Research

1. Open the company website via WebFetch — understand what they do, their scale, their format
2. Read `Context/Company/about.md` — to understand what we offer.
   Empty — say so and offer `/enrich-company` first. Do not guess what the company does: the whole «Стратегия» section is built on it.
3. Determine the industry, rough scale, key business lines

### Step 2: Parallel research (5 agents)

Launch **5 Agent agents in parallel** (subagent_type: "general-purpose"). Give each one the company name, URL and context.

---

**Agent 1 — PROFILER**

```
You are a corporate data analyst. Research the company and collect a factual profile.

COMPANY: [name]
WEBSITE: [url]

What to look for:
1. Legal entity (ИНН, ОГРН) — search checko.ru, rusprofile.ru, companies.rbc.ru
2. Founders and CEO (full names)
3. Year founded
4. Financials (revenue, profit, trend) — СПАРК, РБК Компании, Контур.Фокус
5. Headcount (estimate)
6. Business structure: business lines, subsidiaries, branches
7. Legal and actual address

Use WebSearch and WebFetch. Search queries:
- "[название] ИНН ОГРН"
- "[название] учредители генеральный директор"
- "[название] выручка прибыль"
- site:checko.ru "[название]"
- site:rusprofile.ru "[название]"

Response format:
## Корпоративный профиль

| Параметр | Значение | Источник |
|----------|----------|----------|
| Юрлицо | ... | ... |
| ИНН | ... | ... |
| Гендиректор | ... | ... |
| Учредители | ... | ... |
| Год основания | ... | ... |
| Выручка | ... | ... |
| Сотрудники | ... | ... |

## Структура бизнеса
[описание направлений]

## Источники
[все URL]
```

---

**Agent 2 — MARKET ANALYST**

```
You are a market analyst. Research the company's positioning, its competitors and recent news.

COMPANY: [name]
WEBSITE: [url]
INDUSTRY: [industry from Step 1]

What to look for:
1. What exactly the company does (products, services, target audience)
2. Competitors (3-5 main ones)
3. Positioning and USP
4. Recent news from the last 1-2 years (expansion, M&A, leadership change, new products)
5. Growth plans (expansion, new markets, investment)
6. Clients / partners (if public information)

Use WebSearch. Search queries:
- "[название] новости 2025 2026"
- "[название] конкуренты рынок"
- "[название] планы развитие экспансия"
- "[название] site:rbc.ru OR site:kommersant.ru OR site:vedomosti.ru OR site:vc.ru"
- "[название] интервью CEO основатель"

Response format:
## Продукт и рынок

### Что делает компания
[описание]

### Конкуренты
| Конкурент | Чем отличается |
|-----------|---------------|

### Последние новости (1-2 года)
- [дата]: [событие] — [источник]

### Планы развития
[что известно]

### Источники
[все URL]
```

---

**Agent 3 — PAIN HUNTER**

```
You are a business-problem researcher. Find every sign of pain: employee reviews, job openings, customer complaints, negative coverage.

COMPANY: [name]
WEBSITE: [url]

What to look for:
1. Employee reviews — DreamJob, Glassdoor, otzovik. Rating, pros/cons, key complaints
2. Job openings on hh.ru — how many are open, which roles (mass hiring = churn)
3. Customer reviews — Яндекс.Карты, Google, 2ГИС, review sites
4. Negative media coverage — lawsuits, scandals, complaints
5. Signs of operational problems (from job openings and reviews)

Use WebSearch and WebFetch. Search queries:
- "[название] отзывы сотрудников"
- "[название] отзывы работников dreamjob"
- site:dreamjob.ru "[название]"
- site:hh.ru "[название]" вакансии
- "[название] проблемы жалобы"
- "[название] отзывы клиентов"

Response format:
## Сигналы проблем

### Отзывы сотрудников
- Рейтинг: X/5 (N отзывов)
- Плюсы: [что хвалят]
- Минусы: [ключевые жалобы — цитаты]

### Вакансии
- Открыто: N вакансий
- Типы: [какие позиции, массовый ли найм]
- Что это значит: [интерпретация]

### Отзывы клиентов
- Рейтинг: X/5
- Ключевые жалобы: [если есть]

### Негатив в СМИ
[если найдено]

### Источники
[все URL]
```

---

**Agent 4 — TECHNOLOGIST**

```
You are an IT-infrastructure expert. Determine the company's technological maturity and its experience with AI.

COMPANY: [name]
WEBSITE: [url]

What to look for:
1. Which IT systems they use (from job openings, the website, interviews): CRM, ERP, POS, BI, LMS
2. Whether there is an in-house IT department (from job openings: are they hiring developers, an IT director)
3. Experience with AI / automation (from news, interviews, case studies)
4. Chatbots on the website, AI features in the app
5. Technology stack (from job openings: Python, 1С, iiko, Bitrix, etc.)

Use WebSearch and WebFetch. Search queries:
- "[название] вакансии разработчик IT"
- "[название] автоматизация AI искусственный интеллект"
- "[название] CRM ERP система"
- site:hh.ru "[название]" (разработчик OR программист OR IT OR data)
- "[название] цифровизация технологии"

Also open the company website via WebFetch and check:
- Whether there is a chatbot
- Whether there is a mobile app
- How modern the site is (technologically)

Response format:
## Технологический профиль

### Известные IT-системы
| Система | Тип | Источник |
|---------|-----|----------|

### IT-команда
- Есть ли внутренний отдел: да/нет/неизвестно
- Открытые IT-вакансии: [если есть]

### Опыт с AI
- [что известно]

### Технологическая зрелость
- Оценка: высокая / средняя / низкая / неизвестно
- Обоснование: [почему]

### Источники
[все URL]
```

---

**Agent 5 — STRATEGIST**

```
You are a strategist. Based on the industry and the company's scale, propose which solutions could be valuable and which off-the-shelf alternatives exist.

COMPANY: [name]
INDUSTRY: [industry]
SCALE: [headcount, locations, offices — whatever is known]
CONTEXT: [what we know about the pains from Step 1]
OUR SERVICES: [from Context/Company/about.md]

Your tasks:

1. Based on the industry, formulate 5-7 typical problems a business of this type and scale usually has (general industry knowledge).

2. For each problem determine:
   - Is there an off-the-shelf SaaS solution on the market? (name, price)
   - Can the client do it themselves (no-code, ChatGPT)?
   - Is custom development needed?

3. Pick out the 2-3 problems where our services deliver the most value (no ready-made solutions OR the ready-made ones do not cover the specifics).

Use WebSearch to look for ready-made solutions.

Response format:
## Стратегия

### Типовые проблемы индустрии
| # | Проблема | Готовое решение | Цена | Нужна кастомная разработка? |
|---|----------|----------------|------|-----------------------------|

### ТОП-3 точки входа для нас
1. [решение] — почему именно мы, а не SaaS
2. [решение] — ...
3. [решение] — ...

### Источники
[все URL]
```

---

### Step 3: Verification and synthesis

Once all 5 reports are in:

1. **Check for discrepancies** — if agents give different data (rating, headcount, CEO) — flag it and re-check where possible.

2. **Assemble the company profile** — merge data from Agents 1, 2, 4.

3. **Formulate problem hypotheses** — based on Agent 3 (pains) + Agent 2 (market context) + Agent 5 (typical industry problems). For each hypothesis state:
   - Description and evidence
   - Estimated cost of the problem to the business (in RUB/year)
   - Боль (1-5)
   - Стоимость (1-5)
   - Продаваемость (1-5)
   - Risks: ready-made solutions, DIY, not our territory

4. **Form the proposals** — from Agent 5, tied to the hypotheses.

5. **Assemble the «Что можно закрыть без нас» block** — from Agent 5, with service names and prices.

### Step 4: Document

Create **one file**: `Projects/исследование-[компания]-[дата].md`

Files in `Projects/` are versioned in git. If the repository is public, the names, contacts and quotes from reviews collected below will be visible to everyone — decide what to write down.

Full analysis for internal use:
- Verified company profile (table)
- Business structure
- Decision-maker contacts (if found)
- All problem hypotheses with scores: Боль (1-5) × Стоимость (1-5) × Продаваемость (1-5)
- For each hypothesis: risks (ready-made solutions, DIY, not our territory)
- Proposal matrix with priorities and budgets
- «Что можно закрыть без нас» block (SaaS alternatives with prices)
- Questions for the first meeting

### Step 5: Summary

Post a **short summary** in chat (no more than 15-20 lines):
- Company profile (3-4 lines)
- TOP-3 problem hypotheses (1 line each)
- TOP-3 proposals (1 line each)
- Where the file is saved

---

## IMPORTANT RULES

1. **Always WebSearch.** Every agent MUST search the internet. Do not generate data from your head.
2. **Parallel launch.** All 5 agents are launched at the same time.
3. **Verification.** If data from different agents disagrees — re-check and flag it.
4. **Honesty.** If there is no data for some parameter — write «не найдено», do not make it up.
5. **Write-first.** The result always goes into a file. Chat gets only the summary.
6. **Do not oversell.** In the «Что можно закрыть без нас» section, honestly list off-the-shelf SaaS alternatives.
7. **Freshness.** When searching, add the current year to get fresh data.

---

Company to research:

$ARGUMENTS
