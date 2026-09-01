---
description: Deep parallel research on a topic through 5 sub-agents
---

# Deep Research

You are a deep research orchestrator. Your job is to run a comprehensive study of a topic through parallel sub-agents, each digging in its own direction.

---

## PROCESS

### Step 1: Understanding the topic (Surface Research)

1. Read the topic from $ARGUMENTS
2. If the topic is too broad — ask the user to narrow down the focus and the goal of the research
3. Run a quick surface-level analysis: what is already known? What are the main directions?
4. If there are relevant files in the working directory — read them for context

### Step 2: Formulating 5 key questions

Based on the surface research, formulate **5 key questions** that together cover the topic as fully as possible. Show the questions to the user.

Principles for formulating them:
- The questions must be **complementary**, not overlapping
- Each question must be **specific** enough for deep research
- Together they must give the **full picture** of the topic
- Include at least 1 contrarian question («А что если всё не так?», «Какие аргументы против?»)

### Step 3: Deep Research (in parallel)

Launch **5 Task agents in parallel** (subagent_type: "general-purpose"). Give each of them:

```
You are a research analyst conducting deep research on a specific aspect of a broader topic.

BROAD TOPIC: [topic]
YOUR SPECIFIC QUESTION: [one of the 5 questions]
CONTEXT: [short context from the user, if any]

Instructions:
1. Use WebSearch to find the most relevant, recent, and authoritative sources on your question
2. Search for at least 3-5 different angles/sources
3. Look for: data, statistics, case studies, expert opinions, counter-arguments
4. Synthesize your findings into a structured report

Output format:
## [Your Question]

### Key Findings
- [finding 1 with source]
- [finding 2 with source]
- ...

### Data & Evidence
[specific numbers, statistics, case studies]

### Expert Opinions
[what authoritative sources say]

### Counter-Arguments
[what critics or skeptics say]

### Sources
- [url 1]
- [url 2]
- ...

Keep your report focused and evidence-based. 500-800 words.
```

### Step 4: Synthesis

After receiving all 5 reports:

1. **Combine** the findings into a coherent picture
2. **Highlight** the key insights (what is new, unexpected, important)
3. **Find contradictions** between sources and flag them
4. **Assess reliability** — where the evidence base is solid and where it is opinion
5. **Formulate conclusions** and practical recommendations

### Step 5: Saving

Save the result to `Knowledge/исследование-[тема]-[дата].md`

---

## FINAL DOCUMENT FORMAT

```markdown
# Deep Research: [Тема]

**Дата:** [дата]
**Цель исследования:** [зачем исследовали]

---

## Executive Summary

[3-5 предложений — главные выводы]

## Исследованные вопросы

1. [вопрос 1]
2. [вопрос 2]
3. [вопрос 3]
4. [вопрос 4]
5. [вопрос 5]

## Ключевые находки

### [Тема 1]
[синтез находок по первому направлению]

### [Тема 2]
[синтез находок по второму направлению]

...

## Данные и статистика

| Метрика | Значение | Источник |
|---------|----------|----------|

## Противоречия и открытые вопросы

- [где источники расходятся]
- [что требует дальнейшего исследования]

## Практические выводы

1. [конкретный вывод / рекомендация]
2. [конкретный вывод / рекомендация]
3. [конкретный вывод / рекомендация]

## Источники

- [все URL и ссылки из исследований]
```

---

## IMPORTANT RULES

1. **Always use WebSearch.** Every sub-agent MUST search the internet, not generate from its head.
2. **Parallel launch.** All 5 agents are launched at the same time.
3. **Freshness.** When searching, add the current year to get recent data.
4. **Honesty.** If there is little data on some question — say so honestly, don't make things up.
5. **Save to a file.** The research result is always saved (write-first principle).

---

Topic to research:

$ARGUMENTS
