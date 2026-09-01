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
7. Whether the address is a mass-registration address, whether the CEO is a mass director.
   ФНС thresholds: more than 5 legal entities per director, more than 10 per founder, more than 5 companies at one address
8. Whether the CEO or the founders are in the реестр дисквалифицированных лиц — service.nalog.ru, search by ФИО
9. **Restart check.** Look at the other companies of the same founders: is there one that was liquidated,
   struck off or went bankrupt shortly before this one appeared. A fresh entity with the same owners after
   a previous one left debts behind is a standard scheme, and you already have the data for it

**If the counterparty is an ИП, not a legal entity** — it is in ЕГРИП, not ЕГРЮЛ. There is no КПП,
no founders, no registered capital and no published accounts. Check instead: status in ЕГРИП, date of
registration, ОКВЭД, absence of a termination record, personal bankruptcy (ЕФРСБ by ФИО), enforcement
proceedings by ФИО. Do not report empty fields as «не найдено» — say that data of that kind does not exist for an ИП.

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
[массовый адрес, массовый директор, дисквалификация руководителя, отметка о недостоверности, решение о предстоящем исключении, стадия ликвидации, смена директора за последний месяц, возраст компании меньше года, признаки перезапуска — перечислить только то, что реально нашлось.
Уставный капитал 10 000 ₽ красным флагом НЕ считается: это законный минимум для ООО и он у большинства нормальных компаний. Это довод к размеру лимита отгрузки, а не к отказу]

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
7. Tax regime **and VAT rate**. The old shortcut «УСН значит без НДС» stopped being true in 2025:
   simplified-regime companies above the income threshold are VAT payers, and the threshold keeps dropping.
   What actually decides whether they can deduct our VAT is the rate: on the reduced rates there is no
   input deduction, on the general one there is. Suppliers of meat raw materials are often on ЕСХН — there
   VAT applies by default with an exemption under ст. 145 НК below the income limit.
   So record three things, not one: режим, ставка НДС, наличие освобождения. Thresholds change every year —
   check the current ones instead of relying on remembered numbers.

Sources you can actually fetch: checko.ru, rusprofile.ru — they publish the financial statements
and the tax data. bo.nalog.gov.ru and pb.nalog.ru are forms; do not claim you read them.

**Account suspensions have their own free service:** `service.nalog.ru/bi.do` — a query by ИНН plus the
bank's БИК returns current decisions to suspend operations and the ground for each. The БИК comes from
the карточка предприятия you ask for before the first shipment. Aggregator data on suspensions lags,
and for a shipment on deferred payment it is today's picture that matters. Put this into the
«Проверить руками» checklist — you cannot query it yourself.
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
- "[название] реестр недобросовестных поставщиков"

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
[ответчик по искам о взыскании долга за товар, рост числа дел за последний год, открытые исполнительные производства]

**Always state the company's role in a bankruptcy case.** A wholesaler routinely appears in ЕФРСБ and
КАД as a creditor in its own customers' bankruptcies — that is normal receivables work, not a risk sign.
A red flag is bankruptcy **of this company**: a case opened against it, a creditor's notice of intent to
file, наблюдение introduced. «Компания как должник» and «компания как кредитор» must never be merged
into one line — otherwise the check returns 🔴 on a neutral fact and kills a normal shipment.

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
1. Declarations of conformity — реестр Росаккредитации, `pub.fsa.gov.ru`, by ИНН. Which technical
   regulations they cover: ТР ТС 021/2011 and 022/2011 apply to all food, ТР ТС 034/2013 to meat,
   029/2012 when additives are used. A declaration that names the wrong regulation is a finding
2. Registration in ВетИС — public registries live on `cerberus.vetrf.ru` and open without a login
3. Product non-conformities — ГИР ЗПП, `zpp.rospotrebnadzor.ru/badproducts/violations`: a public registry
   with a filter by manufacturer. This is the right address, not the Роспотребнадзор home page
4. Whether they produce themselves or resell — it decides who answers for quality

If the company is a BUYER (a chain, a distributor, HoReCa), the two checks below are **blocking
conditions for the first shipment**, not background information. Our product is meat and poultry
cooked sous-vide — chilled, vacuum-packed, of animal origin:

1. **ВетИС.** The buyer must be registered as a хозяйствующий субъект, have a площадка in «Цербер» at the
   actual address of the warehouse, and someone with the right to гасить входящие эВСД. Not registered —
   the shipment cannot legally happen at all, no commercial terms will fix it. Check it from our own
   «Меркурий» account: when issuing an эВСД the recipient is looked up by ИНН. Put this first in the
   «Проверить руками» checklist
2. **«Честный ЗНАК».** Marking of meat products is being introduced in stages, and participants of the
   turnover must be registered. Look for them in the public list of participants —
   `честныйзнак.рф/business/spisokuot/`, it opens without a login — and check whether they accept УПД with
   codes over ЭДО and through which operator. **Check the dates in force today**: the schedule shifts,
   and a stale date in this file is worse than no date

Then the commercial part:

3. Their supplier requirements — often published: остаточный срок годности при приёмке, packaging, EDI, labeling
4. Payment terms they actually work on — from suppliers' reviews and from arbitration cases
5. **Cold chain.** Sous-vide is chilled. Does the buyer have refrigerated storage and transport, and is the
   «Меркурий» площадка registered at that warehouse. Without it the returns and the quality claims are ours
6. **Who actually pays.** For franchise chains and HoReCa the brand on the sign and the legal entity on the
   contract are routinely different companies. Deferred payment is granted to an ИНН, not to a brand —
   confirm that the ИНН we checked is the one that will sign
7. Whether they have their own private label — that is both an opportunity and a risk

**Reachable:** the company website, a plain web search, `cerberus.vetrf.ru` (public ВетИС registries,
no login), `честныйзнак.рф/business/spisokuot/` (public list of participants, no login),
`zpp.rospotrebnadzor.ru/badproducts/violations` (public registry of non-conforming products).

**Not reachable — hand these to a person as links:** `pub.fsa.gov.ru` (a JS form), «Меркурий» itself
(`mercury.vetrf.ru`, needs an account — but the check is done from our own account, see above).
There is no ИНН lookup on `fsvps.gov.ru` at all; do not send anyone there.

**Neither checko.ru nor rusprofile.ru publish declarations of conformity.** Do not look for them there
and do not claim you did.
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
   - **Stop signals** — банкротство самой компании, стадия ликвидации, решение о предстоящем исключении
     недействующего юрлица, отметка о недостоверности сведений, дисквалификация руководителя,
     массовый директор при свежей регистрации, признаки перезапуска после брошенного юрлица с долгами.
     («Исключение из ЕГРЮЛ» в этот список не входит: договор подписывать уже не с кем, до вердикта не дойдёт)
   - **Serious** — ответчик по искам о взыскании за товар, отрицательные чистые активы, задолженность по налогам, приостановка счетов, жалобы на задержки зарплаты
   - **To keep in mind** — молодая компания, маленький уставный капитал, падение выручки, слабый сайт

4. **Produce the verdict** — one of three:
   - 🟢 **Работаем** — стоп-сигналов нет, серьёзных нет или они объяснимы
   - 🟡 **Работаем осторожно** — есть серьёзные флаги; назвать, какие именно условия их закрывают
   - 🔴 **Не работаем** — есть стоп-сигнал; назвать, какой

5. **Recommend the terms.** This is what the check exists for. Tie it to what is at stake:
   - Prepayment / partial prepayment / deferred payment and for how many days
   - **The deferral has a legal ceiling for food, tied to shelf life** — ч. 7 ст. 9 ФЗ-381
     («О торговле»). The shorter the shelf life, the shorter the maximum deferral the law allows,
     and sous-vide is chilled, so the short end is ours. Look up the current wording and name the limit
     for our specific shelf life: agreeing a longer deferral than the law permits makes that clause void
   - Shipment limit in rubles — tie it to the revenue and the net assets, not to a feeling
   - Security, when the verdict is 🟡 and the volume is worth it: личное поручительство собственника,
     банковская гарантия, страхование дебиторки, факторинг, or a stepped scheme — prepayment first,
     deferral after N clean shipments
   - What to ask for before the first shipment: свежая выписка из ЕГРЮЛ с ЭЦП (`egrul.nalog.ru` — the only
     authoritative source on who may sign), карточка предприятия with the БИК, копии деклараций,
     доверенность подписанта if the contract is not signed by the director named in the выписка
   - For a chain or a distributor — read their draft contract, not only their reputation: retro bonuses and
     marketing fees for food are capped by ст. 9 ФЗ-381, and there will be penalties for short delivery
     and requirements on the remaining shelf life at acceptance
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
