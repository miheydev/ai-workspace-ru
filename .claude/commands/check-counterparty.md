---
description: "Counterparty due diligence from open sources: legal status, finances, courts and debts, real activity, industry compliance — with a traffic-light verdict and payment-terms recommendation. Triggers: 'проверь контрагента', 'проверь компанию перед отгрузкой', 'что за фирма', '/check-counterparty'."
language: en
---

# Counterparty Check

You orchestrate a due-diligence check of a counterparty — a buyer, a supplier, or a partner — from open sources. The result is a verdict and a recommendation on terms of work.

The check answers one question: **can we work with this company, and on what terms.**

---

## INPUT

Extract from $ARGUMENTS:
- **Company name** (required)
- **ИНН** — if known. Raises accuracy a lot: everything is found by it
- **Website** — if there is one
- **Who they are to us** (optional): buyer, supplier, transport company, landlord
- **What is at stake** (optional): shipment on deferred payment, prepayment to a supplier, a long contract

No name and no ИНН — ask. One of the two is enough to start.

---

## PROCESS

### Step 1: Identify the company

1. Find the legal entity by name or ИНН — `egrul.nalog.ru`, `checko.ru`, `rusprofile.ru`
2. Several entities with similar names — show the list and ask which one. Do not pick the first one yourself: shipping to the wrong ИНН is how the whole check becomes pointless
3. Read `Context/Company/about.md` and `Context/Company/products.md` — so the check is done from our position: what we ship, on what terms, what a failure costs us.
   Empty — the check still works, but say so: without it there is no recommendation on terms, only the facts

### Step 2: Parallel check (5 agents)

Launch **5 Agent agents in parallel** (subagent_type: "general-purpose"). Give each the name, the ИНН and the website.

---

**Agent 1 — LEGAL STATUS**

```
You check whether the company legally exists and is in good standing.

COMPANY: [name]
ИНН: [inn]

What to look for:
1. Status in ЕГРЮЛ: действующее / ликвидируется / в стадии реорганизации / исключено
2. ИНН, ОГРН, КПП, date of registration, ОКВЭД (main and additional)
3. CEO — full name, since when, and whether they run many other companies
4. Founders, shares, whether there have been recent changes
5. Legal address — and whether there is a mark of «недостоверность сведений» about it or about the CEO
6. Registered capital
7. Whether the address is a mass-registration address, whether the CEO is a mass director

Sources you can actually fetch: checko.ru, rusprofile.ru, list-org.com — they carry ЕГРЮЛ data
and the «недостоверность» marks. egrul.nalog.ru and pb.nalog.ru are search forms: you will not get
a result out of them by fetching, so do not claim you checked there.
Search queries:
- "[название] ИНН ОГРН ЕГРЮЛ"
- "[название] генеральный директор учредители"
- site:checko.ru "[инн]"
- site:rusprofile.ru "[инн]"

Response format:
## Юридический статус

| Параметр | Значение | Источник |
|---|---|---|
| Статус в ЕГРЮЛ | ... | ... |
| ИНН / ОГРН | ... | ... |
| Дата регистрации | ... | ... |
| Основной ОКВЭД | ... | ... |
| Гендиректор | ... | ... |
| Учредители | ... | ... |
| Уставный капитал | ... | ... |
| Юридический адрес | ... | ... |

## Отметки о недостоверности
[есть / нет — если есть, что именно и с какой даты]

## Красные флаги
[массовый адрес, массовый директор, смена директора за последний месяц, возраст компании меньше года, уставный капитал 10 000 ₽ — перечислить только то, что реально нашлось]

## Источники
[все URL]
```

---

**Agent 2 — FINANCES**

```
You assess whether the company is solvent.

COMPANY: [name]
ИНН: [inn]

What to look for:
1. Revenue for the last 3 years — the figure and the trend
2. Profit or loss for the same years
3. Net assets — and whether they are negative
4. Headcount by years
5. Tax debt, suspended bank accounts
6. Taxes paid — the amount and which ones
7. Which tax regime (ОСНО / УСН) — it affects whether they can take our НДС

Sources you can actually fetch: checko.ru, rusprofile.ru — they publish the financial statements
and the tax data. bo.nalog.gov.ru and pb.nalog.ru are forms; do not claim you read them.
Search queries:
- "[название] выручка прибыль по годам"
- "[название] бухгалтерская отчётность"
- site:checko.ru "[инн]" финансы
- "[название] задолженность по налогам"

Response format:
## Финансы

| Год | Выручка | Прибыль | Чистые активы | Сотрудников |
|---|---|---|---|---|

## Налоги
- Режим: [ОСНО / УСН / не найдено]
- Уплачено за последний год: [сумма]
- Задолженность: [сумма или «нет»]
- Приостановки по счетам: [есть / нет]

## Что видно из цифр
[2-4 строки: растёт или падает, есть ли убыток, сходится ли выручка с заявленным масштабом]

## Красные флаги
[убыток два года подряд, отрицательные чистые активы, выручка ноль при действующем статусе, задолженность по налогам, приостановка по счетам]

## Источники
[все URL]
```

---

**Agent 3 — COURTS AND DEBTS**

```
You look for litigation, enforcement proceedings and bankruptcy signs.

COMPANY: [name]
ИНН: [inn]

What to look for:
1. Arbitration cases — how many, for what amounts, as claimant or as defendant
2. What the cases are about: unpaid deliveries, quality, contract termination, tax
3. The trend — are there more of them in the last year
4. Enforcement proceedings (ФССП) — how many, what amounts, on what grounds
5. Bankruptcy: any filings, any notices, any statements of intent to file
6. Registry of unscrupulous suppliers (РНП), if they take part in public procurement

**The primary registries are not reachable for you.** kad.arbitr.ru gives its results through
JavaScript, fedresurs.ru answers 401, zakupki.gov.ru fails on a certificate, fssp.gov.ru asks for a captcha.

So: take the litigation, the enforcement proceedings and the bankruptcy marks from checko.ru and
rusprofile.ru — they aggregate exactly this — plus a plain web search. Then put the direct links into
the report for a person to confirm by hand. Write down which of the two you did: aggregator data or
a confirmed primary source. For a large shipment that difference matters.
Search queries:
- "[название] арбитраж суд иск"
- site:kad.arbitr.ru "[название]"
- "[название] банкротство"
- "[инн]" исполнительное производство
- "[название] реестр недобросовестных поставщиков

Response format:
## Суды

| Год | Дел | Как истец | Как ответчик | Сумма требований |
|---|---|---|---|---|

### О чём дела
[типы споров — особо выделить иски о взыскании за поставленный товар: это прямой сигнал, платят ли они поставщикам]

## Исполнительные производства
- Открыто: [N на сумму X]
- Основания: [что именно]

## Банкротство
[заявления, уведомления о намерении, сообщения на Федресурсе — или «не найдено»]

## Красные флаги
[ответчик по искам о взыскании долга за товар, рост числа дел за последний год, открытые исполнительные производства, любое упоминание банкротства]

## Источники
[все URL]
```

---

**Agent 4 — REAL ACTIVITY**

```
You check whether the company actually operates — or exists only on paper.

COMPANY: [name]
WEBSITE: [url]
ИНН: [inn]

What to look for:
1. Website: does it work, when was it last updated, is there a real product range, real contacts
2. Physical presence: production, warehouses, offices, shops — on Яндекс.Карты and 2ГИС, with photos and reviews
3. Customer reviews — Яндекс.Карты, 2ГИС, industry sites. Rating, what they complain about
4. Employee reviews — DreamJob, otzovik. Especially complaints about delayed wages
5. Job openings on hh.ru — how many, which roles. Zero openings for years at a claimed large scale is a signal, and so is mass hiring
6. Media mentions over the last 2 years
7. How long they have been on the market and under this name

Sources: Яндекс.Карты, 2ГИС, hh.ru, dreamjob.ru, WebFetch of the site itself
Search queries:
- "[название] отзывы клиентов"
- "[название] отзывы сотрудников зарплата"
- site:hh.ru "[название]"
- "[название] новости"

Response format:
## Реальность деятельности

### Сайт
- Работает: [да/нет], последнее обновление: [когда видно]
- Контакты: [реальные / шаблонные плейсхолдеры / нет]

### Физическое присутствие
[что найдено на картах: адреса, фото, отзывы]

### Отзывы клиентов
- Рейтинг: [X/5, N отзывов]
- На что жалуются: [цитаты]

### Отзывы сотрудников
- Рейтинг: [X/5]
- Задержки зарплаты: [упоминаются / нет]

### Вакансии
- Открыто: [N], роли: [какие]

## Красные флаги
[сайт не обновлялся больше года, контакты — незаменённые шаблоны конструктора, адреса нет на картах, нулевая активность при заявленном масштабе, жалобы на задержки зарплаты]

## Источники
[все URL]
```

---

**Agent 5 — INDUSTRY COMPLIANCE**

```
You check what is specific to our industry and cannot be seen in the registries.

COMPANY: [name]
WEBSITE: [url]
ИНН: [inn]
WHO THEY ARE TO US: [buyer / supplier / transport / other]
WHAT WE SHIP OR BUY: [from Context/Company/products.md]

If the company is a SUPPLIER of raw materials or products, look for:
1. Declarations of conformity and certificates — реестр Росаккредитации, pub.fsa.gov.ru by ИНН
2. Registration in ВетИС / «Меркурий» — required for products of animal origin
3. «Честный ЗНАК» — whether they are registered, if their goods are subject to labeling
4. Product recalls, Роспотребнадзор findings, суды по качеству
5. Whether they have their own production or are a reseller

If the company is a BUYER (a chain, a distributor, HoReCa), look for:
1. Their supplier requirements — often published: shelf life, packaging, EDI, labeling
2. Whether they work through EDI and which operator
3. Payment terms they usually work on — from suppliers' reviews and arbitration cases
4. Whether they have their own private label — that is both an opportunity and a risk

**Reachable:** the company website, a plain web search, checko.ru and rusprofile.ru (they list
declarations of conformity by ИНН).

**Not reachable — hand these to a person as links:** pub.fsa.gov.ru (Росаккредитация, a JS form),
«Меркурий» (mercury.vetrf.ru — registration required, there is no ИНН lookup on fsvps.gov.ru at all),
честныйзнак.рф, inspect.rospotrebnadzor.ru. Do not write that you checked them.
Search queries:
- "[название] декларация соответствия"
- "[инн]" site:pub.fsa.gov.ru
- "[название] требования к поставщикам"
- "[название] Роспотребнадзор нарушение"
- "[название] отзыв продукции"

Response format:
## Отраслевая проверка

### Разрешительные документы
| Документ | Есть | Срок действия | Источник |
|---|---|---|---|

### Требования и условия работы
[что известно про их условия — для покупателя; про их документы — для поставщика]

### Претензии по качеству
[отзывы продукции, предписания, суды — или «не найдено»]

## Красные флаги
[просроченные декларации, отсутствие регистрации в ВетИС при работе с животноводческой продукцией, отзывы продукции, предписания надзора]

## Источники
[все URL]
```

---

### Step 3: Verification and synthesis

Once all 5 reports are in:

1. **Check for discrepancies.** Agents disagree on revenue, headcount or the CEO — flag it and re-check. A discrepancy between registries is itself a finding.

2. **Collect all red flags in one list.** Each one with a source. Nothing gets softened at this stage.

3. **Weigh them.** Not every red flag is equal:
   - **Stop signals** — исключение из ЕГРЮЛ, банкротство, отметка о недостоверности, массовый директор при свежей регистрации
   - **Serious** — ответчик по искам о взыскании за товар, отрицательные чистые активы, задолженность по налогам, приостановка счетов, жалобы на задержки зарплаты
   - **To keep in mind** — молодая компания, маленький уставный капитал, падение выручки, слабый сайт

4. **Produce the verdict** — one of three:
   - 🟢 **Работаем** — стоп-сигналов нет, серьёзных нет или они объяснимы
   - 🟡 **Работаем осторожно** — есть серьёзные флаги; назвать, какие именно условия их закрывают
   - 🔴 **Не работаем** — есть стоп-сигнал; назвать, какой

5. **Recommend the terms.** This is what the check exists for. Tie it to what is at stake:
   - Prepayment / partial prepayment / deferred payment and for how many days
   - Shipment limit in rubles
   - What to ask for before the first shipment: карточка предприятия, копии деклараций, доверенность подписанта
   - What to re-check and when

**Do not recommend a decision when the data is thin.** Fewer than three of the five agents found anything — say so directly: this is not a green light, this is an absence of data.

### Step 4: Document

Create **one file**: `Projects/контрагент-[компания]-[дата].md`

Files in `Projects/` are versioned in git. If the repository is public, everything collected here will be visible to everyone — decide what to write down.

Contents:
- Verdict and recommended terms — at the very top, first screen
- All red flags with sources
- The five sections from the agents
- What was not found — a separate list
- **«Проверить руками»** — a checklist of direct links to the registries you could not open, with what
  to look for in each. For a large shipment or a long contract a person walks this list; for a small
  trial batch the aggregator data is usually enough. Say which case this is.
- Date of the check and when to repeat it

### Step 5: Summary

Post a **short summary** in chat (no more than 12-15 lines):
- Verdict: 🟢 / 🟡 / 🔴 — and the one main reason
- 3 key red flags, one line each
- Recommended terms of work
- Where the file is saved

---

## IMPORTANT RULES

1. **Always WebSearch.** Every agent MUST search. A counterparty check made up from memory is worse than no check at all.
2. **Parallel launch.** All 5 agents at the same time.
3. **A source next to every fact.** Not a list at the end — a link next to the statement. In a month nobody will remember where the figure came from.
4. **Never claim a source you did not open.** Half of the state registries do not answer a fetch:
   some need a captcha, some a certificate, some a login. Write down what you actually read.
   «Проверено по ЕГРЮЛ» when you only read an aggregator is the one lie that makes the whole check worthless.

5. **«Не найдено» is a valid answer.** No data — write it that way. Never fill a gap with a plausible guess: a made-up clean record is exactly the failure that costs a shipment.
6. **Absence of red flags is not a green light.** A company registered a month ago has a clean record because it has no history. Say that out loud.
7. **Two aggregators are not two sources.** checko, rusprofile and «За честный бизнес» are one ЕГРЮЛ in three wrappers. For confirmation, look for a source of a different kind.
8. **Freshness.** Add the current year to the query. Registry data ages: a check from six months ago says nothing about today.
9. **The verdict is a recommendation, not a decision.** The decision to ship is made by a person.

---

Company to check:

$ARGUMENTS
