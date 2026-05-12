---
name: decision-record
description: Append a lightweight Architecture Decision Record (ADR) entry to DECISIONS.md. Use this skill whenever the user makes or wants to capture a structural choice, mentions "log this decision," "decision record," "ADR," "we're going with X," "let's commit to X," "document this choice," "captured why we chose," or whenever a non-trivial tradeoff is being made (tech stack pick, library choice, architecture pattern, hosting choice, build vs. buy, etc.). Trigger PROACTIVELY when the user is weighing options and lands on one — offer to capture the why so future you doesn't have to re-litigate. Appends to DECISIONS.md, never overwrites.
---

# Decision Record

Captures the *why* behind structural choices in DECISIONS.md, so future you doesn't have to recreate the reasoning when something needs to change.

## Process

1. **Confirm there's a decision worth capturing.** A decision belongs here if:
   - It affects multiple files or systems
   - It's hard or expensive to reverse
   - There were reasonable alternatives that were rejected
   - The reasoning isn't obvious from reading the code

   If it's just a naming choice or a small data-structure pick — skip. Not every choice is an ADR.

2. **Gather the four parts.** Ask if any are missing:
   - **Context:** what problem are we solving?
   - **Decision:** what did we choose?
   - **Alternatives:** what did we consider and reject? (At least one.)
   - **Tradeoffs:** what did we give up?

3. **Find or create DECISIONS.md.** Header for new files:
```
# Decisions

Lightweight ADR (Architecture Decision Record). Captures the WHY behind choices that are hard to reverse. Newest at the top.

---
```

4. **Append entry at top** in this format:
```
## [YYYY-MM-DD] — [Decision title in one line]

**Context:** [The problem]

**Decision:** [What we chose]

**Alternatives considered:**
- [Option A] — rejected because [reason]
- [Option B] — rejected because [reason]

**Tradeoffs:** [What we gave up]

**Revisit if:** [What would make us reconsider this?]

---
```

5. **Show the diff** before saving.

## Voice

- Past tense in "Decision" once written
- Honest about tradeoffs — every decision has them
- Specific reasons, not vague ("OpenRouter is cheaper for non-critical paths" beats "cost-effective")

## Example

```
## 2026-05-05 — Use OpenRouter for AI feature suggestions

**Context:** The app needs an LLM for a user-facing feature. Direct Anthropic API is the obvious choice but costs add up for a beta product with light usage.

**Decision:** Route through OpenRouter, defaulting to a cheap model (Llama 3.1 8B) for first pass with fallback to Claude Haiku for low-confidence cases.

**Alternatives considered:**
- Direct Anthropic API only — rejected: ~10x more expensive, kills unit economics during beta
- Self-hosted on VPS — rejected: GPU cost > token savings at this volume

**Tradeoffs:** Slightly higher latency, slightly less consistent quality. Acceptable for beta.

**Revisit if:** Daily token spend exceeds $20/day OR users complain about quality.
```

## Quality checklist

- [ ] Decision is genuinely worth capturing (not too small)
- [ ] Has at least one rejected alternative
- [ ] Tradeoffs are honest, not minimized
- [ ] "Revisit if" gives a real trigger to reconsider
- [ ] Date is ISO format (YYYY-MM-DD)
