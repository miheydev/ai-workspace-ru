---
description: Generate 10 content ideas on a topic — trends, audience pain points, non-obvious angles
---

# Content Ideas

Generating content ideas on a given topic. Three parallel agents look for ideas from different sides, then everything is synthesized into a ranked list.

---

## PROCESS

### Step 1: Determine the context

Read $ARGUMENTS. Determine:
- **Topic / area** — what we are generating ideas about
- **Channel** (if given): Telegram, blog, LinkedIn, VK — affects format and depth
- **Audience** (if given) — who the content is for

Read `Context/Company/about.md` and `Context/tone-of-voice.md` to understand the context.

If no channel is given — generate universal ideas that can be adapted.

### Step 2: Three agents in parallel

Launch **3 Task agents in parallel** (subagent_type: "general-purpose", model: "sonnet"):

**Agent 1 — Trending (what's hot):**
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

**Agent 2 — Audience Pain (what hurts the audience):**
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

**Agent 3 — Contrarian (non-obvious angles):**
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

### Step 3: Synthesis and ranking

Collect all the ideas (9-12 of them) and:

1. **Merge** similar / duplicate ones
2. **Rank by potential** — score each one on 3 criteria (1-5):
   - **Актуальность** — how "hot" it is right now
   - **Уникальность** — how much it differs from what has already been written
   - **Вовлечение** — how likely an audience reaction is (like, comment, repost)
3. **Pick the TOP 10** and sort by total score

### Step 4: Show the result

Output to chat:

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

## IMPORTANT RULES

1. **Use Sonnet** (model: "sonnet") for the agents — faster for generating ideas.
2. **WebSearch is mandatory** for the Trending and Audience Pain agents — real data is needed, not made-up stuff.
3. **Do not give generic ideas.** "10 способов использовать AI" — bad. "Почему 73% внедрений AI-ботов в продажах проваливаются (и 3 паттерна тех, кто выжил)" — good.
4. **Result goes to chat, and to a file** — `Projects/идеи-[тема]-[дата].md`. A list of ten ideas is worth more than one session (CLAUDE.md, rule 2).

---

Topic to generate ideas for:

$ARGUMENTS
