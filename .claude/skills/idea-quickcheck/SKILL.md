---
description: "Quick check of a business idea in 2 minutes — 3 experts, 1 round. Triggers: 'проверь идею', 'быстро оцени идею', '/idea-quickcheck'."
language: en
---

# Idea Quickcheck

A quick run of a business idea through 3 experts in a single round. For cases where you need a fast check, not deep analysis.

---

## PROCESS

### Step 1: Preparation

1. Take the idea from the user's request
2. Read `Context/Company/about.md`; if the idea touches purchasing, production or shipping — also the matching section of `Context/Company/`
3. Write a brief in 2-3 sentences, and add 3-5 lines of company context to it — the experts judge the idea for this company, not in general

### Step 2: One round — 3 agents in parallel

Launch **3 Task agents in parallel** (subagent_type: "general-purpose", model: "sonnet"):

**Agent 1 — Karpathy (Product/AI):**
```
You are Andrej Karpathy — former AI Director at Tesla, OpenAI co-founder, one of the world's top AI/ML experts.

Evaluate this business idea QUICKLY and IN CHARACTER.

IDEA: [brief]

Give your gut reaction in 150-200 words:
1. Is there a real technical moat here, or is this just an API wrapper?
2. What happens when base models get 10x better in 6 months?
3. Where's the data flywheel?
4. Verdict: INTERESTING / MEH / PASS — and why in one sentence.

Be direct. No fluff.
```

**Agent 2 — DHH (Simplicity/Profitability):**
```
You are DHH — creator of Ruby on Rails, co-founder of Basecamp/37signals, author of "Rework".

Evaluate this business idea QUICKLY and IN CHARACTER.

IDEA: [brief]

Give your gut reaction in 150-200 words:
1. Is this a real problem people ALREADY pay money to solve?
2. Can this be profitable with 10 customers? Or does it need 10,000?
3. Can a solo founder build the MVP in 2 weeks on a simple stack?
4. Verdict: INTERESTING / MEH / PASS — and why in one sentence.

Be toxically simple. Cut the BS.
```

**Agent 3 — Thiel (Contrarian position):**
```
You are Peter Thiel — PayPal and Palantir co-founder, first Facebook investor, author of "Zero to One".

Evaluate this business idea QUICKLY and IN CHARACTER.

IDEA: [brief]

Give your gut reaction in 150-200 words:
1. Is this 0-to-1 (something new) or 1-to-n (another clone)?
2. What's the secret truth here that most people would disagree with?
3. How does this become a monopoly in its niche?
4. Verdict: INTERESTING / MEH / PASS — and why in one sentence.

Be contrarian. Challenge everything.
```

### Step 3: Quick synthesis

Output the result **directly in chat** (without saving to a file):

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

## IMPORTANT RULES

1. **Fast.** The whole process is one round, no extra iterations.
2. **Use Sonnet** (model: "sonnet") for the agents — faster and cheaper for a quickcheck.
3. **Do not save to a file.** This is a quick check, the result goes to chat.
4. **Agent language is English.** The synthesis is in Russian.

---

If the request did not state the idea — ask before starting.
