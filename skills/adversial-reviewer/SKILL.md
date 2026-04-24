---
name: adversarial-review
description: Runs an adversarial multi-model code/content review using Claude Sonnet 4.6, Claude Opus 4.6, GPT-5.4, and GPT-5.3-Codex. Each model independently reviews the input, then all models critique each other's findings, and a final synthesis produces a definitive list of improvement suggestions.
---

## Adversarial Multi-Model Review Skill

When the user asks for an **adversarial review**, a **multi-model review**, or invokes this skill with something like:
- "adversarial review this"
- "run a multi-model review on ..."
- "get all models to review ..."
- "adversarial review [file or content]"

You will execute the following workflow:

---

### Phase 1 — Independent Reviews (run in parallel)

Launch **4 background sub-agents simultaneously**, each using a different model, with identical instructions:

> "You are an expert reviewer. Review the following [code / content / design] provided by the user. Be critical, thorough, and precise. Identify real issues only — bugs, logic errors, security vulnerabilities, maintainability problems, architectural concerns, missing error handling, duplication, or incorrect behaviour. Do NOT comment on formatting or style unless it causes functional problems. Return a numbered list of findings, each with: (1) a short title, (2) the specific location or snippet, (3) why it is a problem, (4) a concrete suggestion to fix it."

Models to use:
- Agent A: `claude-sonnet-4.6`
- Agent B: `claude-opus-4.6`
- Agent C: `gpt-5.4`
- Agent D: `gpt-5.3-codex`

Wait for all 4 agents to complete before proceeding.

---

### Phase 2 — Cross-Model Adversarial Critique (run in parallel)

Launch **4 new background sub-agents** (one per model), each receiving ALL 4 reviews from Phase 1. Each agent's instruction:

> "You have been given 4 independent reviews of the same [code / content]. Your job is adversarial critique:
> 1. Identify findings that appear in multiple reviews — these are HIGH CONFIDENCE issues.
> 2. Challenge findings that appear in only one review — are they valid, overstated, or wrong?
> 3. Identify anything important that ALL reviewers missed.
> 4. Rank the findings by severity and confidence.
> Return: (a) a CONFIRMED list (appeared in 2+ reviews), (b) a DISPUTED list (only 1 reviewer, you think it's debatable), (c) a MISSED list (none of the reviewers caught this but you spotted it)."

Models to use (same 4, adversarial roles):
- Agent A: `claude-sonnet-4.6`
- Agent B: `claude-opus-4.6`
- Agent C: `gpt-5.4`
- Agent D: `gpt-5.3-codex`

Wait for all 4 critique agents to complete before proceeding.

---

### Phase 3 — Definitive Synthesis

Using your own reasoning (no additional sub-agent needed), synthesize all Phase 1 reviews and Phase 2 critiques into a **definitive, ranked list of improvement suggestions**.

Format the final output as:

```
## 🔴 Critical (must fix)
1. [Title] — [Location] — [Why] — [Fix]

## 🟠 High (strongly recommended)
...

## 🟡 Medium (good to address)
...

## 🟢 Low / Optional
...

## ℹ️ Notes
- Disputed findings (only raised by 1 model, uncertain validity): ...
- Consensus findings (raised by 3+ models): ...
```

Close with: **"Adversarial review complete. X models agreed on Y critical issues."**

---

### Important Rules

- Always run Phase 1 agents in **parallel** (do not run sequentially).
- Always run Phase 2 agents in **parallel** (do not run sequentially).
- If the user provides a file path, read the file first before launching agents.
- If the user provides code inline, pass it directly to all agents.
- Do not skip any model — all 4 must participate in both phases.
- If a sub-agent fails, note it and proceed with the remaining results.
