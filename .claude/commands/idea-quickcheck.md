---
description: Быстрая проверка бизнес-идеи за 2 минуты — 3 эксперта, 1 раунд, без файла
---

# Idea Quickcheck

Быстрая прогонка бизнес-идеи через 3 экспертов за один раунд. Для случаев, когда нужна быстрая проверка, а не глубокий анализ.

---

## ПРОЦЕСС

### Шаг 1: Подготовка

1. Прочитай идею из $ARGUMENTS
2. Сформулируй бриф в 2-3 предложениях

### Шаг 2: Один раунд — 3 агента параллельно

Запусти **3 Task-агента параллельно** (subagent_type: "general-purpose", model: "sonnet"):

**Агент 1 — Karpathy (Продукт/AI):**
```
You are Andrej Karpathy — former AI Director at Tesla, OpenAI co-founder, one of the world's top AI/ML experts.

Evaluate this business idea QUICKLY and IN CHARACTER.

IDEA: [бриф]

Give your gut reaction in 150-200 words:
1. Is there a real technical moat here, or is this just an API wrapper?
2. What happens when base models get 10x better in 6 months?
3. Where's the data flywheel?
4. Verdict: INTERESTING / MEH / PASS — and why in one sentence.

Be direct. No fluff.
```

**Агент 2 — DHH (Простота/Прибыльность):**
```
You are DHH — creator of Ruby on Rails, co-founder of Basecamp/37signals, author of "Rework".

Evaluate this business idea QUICKLY and IN CHARACTER.

IDEA: [бриф]

Give your gut reaction in 150-200 words:
1. Is this a real problem people ALREADY pay money to solve?
2. Can this be profitable with 10 customers? Or does it need 10,000?
3. Can a solo founder build the MVP in 2 weeks on a simple stack?
4. Verdict: INTERESTING / MEH / PASS — and why in one sentence.

Be toxically simple. Cut the BS.
```

**Агент 3 — Thiel (Контрарная позиция):**
```
You are Peter Thiel — PayPal and Palantir co-founder, first Facebook investor, author of "Zero to One".

Evaluate this business idea QUICKLY and IN CHARACTER.

IDEA: [бриф]

Give your gut reaction in 150-200 words:
1. Is this 0-to-1 (something new) or 1-to-n (another clone)?
2. What's the secret truth here that most people would disagree with?
3. How does this become a monopoly in its niche?
4. Verdict: INTERESTING / MEH / PASS — and why in one sentence.

Be contrarian. Challenge everything.
```

### Шаг 3: Быстрый синтез

Выдай результат **прямо в чат** (без сохранения в файл):

```
## Quickcheck: [Название идеи]

### Karpathy (Продукт/AI): [INTERESTING/MEH/PASS]
[ключевая мысль в 1-2 предложениях]

### DHH (Простота): [INTERESTING/MEH/PASS]
[ключевая мысль в 1-2 предложениях]

### Thiel (Контрарная): [INTERESTING/MEH/PASS]
[ключевая мысль в 1-2 предложениях]

---

**Общий вердикт: [X из 3 сказали INTERESTING]**

**Главный риск:** [одно предложение]
**Главная возможность:** [одно предложение]
**Стоит ли копать глубже?** [Да / Нет / Только если ...]
```

---

## ВАЖНЫЕ ПРАВИЛА

1. **Быстро.** Весь процесс — один раунд, без дополнительных итераций.
2. **Используй Sonnet** (model: "sonnet") для агентов — быстрее и дешевле для quickcheck.
3. **Не сохраняй в файл.** Это быстрая проверка, результат — в чат.
4. **Язык агентов — английский.** Синтез — на русском.

---

Идея для проверки:

$ARGUMENTS
