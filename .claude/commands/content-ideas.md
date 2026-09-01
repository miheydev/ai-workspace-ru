---
description: Генерация 10 идей для контента по теме — тренды, боли ЦА, неочевидные углы
---

# Content Ideas

Генерация идей для контента по заданной теме. Три параллельных агента ищут идеи с разных сторон, затем синтез в ранжированный список.

---

## ПРОЦЕСС

### Шаг 1: Определи контекст

Прочитай $ARGUMENTS. Определи:
- **Тема / область** — о чём генерируем идеи
- **Канал** (если указан): Telegram, блог, LinkedIn, VK — влияет на формат и глубину
- **ЦА** (если указана) — для кого контент

Прочитай `Контекст/about-me.md` и `Контекст/brand-voice.md` для понимания контекста.

Если канал не указан — генерируй универсальные идеи, пригодные для адаптации.

### Шаг 2: Три агента параллельно

Запусти **3 Task-агента параллельно** (subagent_type: "general-purpose", model: "sonnet"):

**Агент 1 — Trending (что горячего):**
```
You are a trend analyst specializing in AI, tech, and business content.

TOPIC: [тема]

Your task:
1. Use WebSearch to find what's trending RIGHT NOW in this topic area (last 2-4 weeks)
2. Look for: recent news, viral discussions, new tool launches, controversial takes, regulatory changes
3. Generate 3-4 content ideas based on current momentum

For each idea provide:
- **Title** (catchy, specific)
- **Hook** (1 sentence — why someone would click/read NOW)
- **Key angle** (what makes this timely)

Return in Russian. Be specific, not generic.
```

**Агент 2 — Audience Pain (что болит у ЦА):**
```
You are an audience research specialist.

TOPIC: [тема]
TARGET AUDIENCE: [ЦА если указана, иначе: "предприниматели и руководители, интересующиеся AI"]

Your task:
1. Use WebSearch to find:
   - Common questions people ask about this topic (forums, Q&A sites, Reddit, Telegram)
   - Complaints and frustrations related to this topic
   - "How to..." and "Why doesn't..." searches
2. Generate 3-4 content ideas that DIRECTLY address audience pain points

For each idea provide:
- **Title** (specific to the pain point)
- **Pain** (what frustrates the audience)
- **Promise** (what the reader gets from this content)

Return in Russian. Focus on practical, actionable content.
```

**Агент 3 — Contrarian (неочевидные углы):**
```
You are a contrarian content strategist. Your job is to find angles that nobody else is covering.

TOPIC: [тема]

Your task:
1. Think about common beliefs and assumptions in this topic area
2. Find counter-intuitive angles:
   - Popular myths to debunk
   - Unpopular but defensible opinions
   - "Everyone says X, but actually Y"
   - Lessons from adjacent fields applied to this topic
   - Personal experience angles ("I tried X and here's what actually happened")
3. Generate 3-4 provocative content ideas

For each idea provide:
- **Title** (provocative, challenges assumptions)
- **Contrarian take** (the unexpected angle)
- **Why it works** (why this will stand out from generic content)

Return in Russian. Be bold, not safe.
```

### Шаг 3: Синтез и ранжирование

Собери все идеи (9-12 штук) и:

1. **Объедини** похожие / дублирующиеся
2. **Ранжируй по потенциалу** — оцени каждую по 3 критериям (1-5):
   - **Актуальность** — насколько это "горячо" прямо сейчас
   - **Уникальность** — насколько отличается от того, что уже написано
   - **Вовлечение** — насколько вероятна реакция аудитории (лайк, коммент, репост)
3. **Выбери ТОП-10** и отсортируй по суммарному баллу

### Шаг 4: Покажи результат

Выведи в чат:

```
## Идеи для контента: [тема]

### ТОП-10

| # | Идея | Тип | А | У | В | Σ |
|---|------|-----|---|---|---|---|
| 1 | [название] | trending/pain/contrarian | X | X | X | XX |
| 2 | ... | ... | ... | ... | ... | ... |

### Детали по ТОП-3

**1. [Название]**
- Угол: [в чём суть]
- Hook: [первое предложение]
- Источник идеи: [trending / pain / contrarian]

**2. ...**

**3. ...**
```

---

## ВАЖНЫЕ ПРАВИЛА

1. **Используй Sonnet** (model: "sonnet") для агентов — быстрее для генерации идей.
2. **WebSearch обязателен** для агентов Trending и Audience Pain — нужны реальные данные, не выдумки.
3. **Не давай generic идеи.** "10 способов использовать AI" — плохо. "Почему 73% внедрений AI-ботов в продажах проваливаются (и 3 паттерна тех, кто выжил)" — хорошо.
4. **Результат в чат** (не в файл), если пользователь не просит сохранить.

---

Тема для генерации идей:

$ARGUMENTS
