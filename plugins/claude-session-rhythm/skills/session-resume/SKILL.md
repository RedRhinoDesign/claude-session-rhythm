---
name: session-resume
description: Read the most recent SESSION_LOG.md entry and reorient for continuing work. Use this skill at the start of a follow-up session when there's no HANDOFF.md but a SESSION_LOG.md exists, OR whenever the user says "what was I doing last time," "catch me up," "what did I work on yesterday," "session resume," "continue from last session," "where did I leave off," or any variant of picking up after a normal break (overnight, weekend, between focused sessions). Trigger PROACTIVELY when entering a project where SESSION_LOG.md exists and the last entry's date is recent (within 14 days) and the user hasn't stated their intent. Does NOT modify SESSION_LOG.md or start a new entry — that's the `session-log` skill's start mode.
---

# Session Resume

The lighter cousin of `handoff-resume`. Where `handoff-resume` reads a deliberate state snapshot, this reads the most recent session log entry. Use when there's no formal handoff but you want context on what just happened.

## Process

1. **Confirm SESSION_LOG.md exists.** If missing, tell the user and stop. If both HANDOFF.md and SESSION_LOG.md exist, suggest using `handoff-resume` instead — it's denser context.

2. **Read the most recent entry** (top of file). Also scan the prior 1-2 entries for context if the most recent is thin.

3. **Summarize back to the user.** Three short sections:
   - **Last session focus:** one sentence
   - **What shipped / what's open:** condensed bullets
   - **Next session pickup:** what the previous entry said to start with

4. **Offer to start a new session entry** via the `session-log` skill in start mode. Don't auto-start — let the user confirm.

5. **Surface anything stale.** If the last entry is more than 7 days old, mention it: "It's been [N] days — want to do a quick state check before diving back in?"

## Voice

- Brief — this is a recap, not a retelling
- Specific bullets > paragraphs
- If the last entry was vague or empty, say so honestly: "Last entry didn't capture much — want to start fresh?"

## Quality checklist

- [ ] Read the most recent entry fully
- [ ] Summary fits in ≤ 6 lines
- [ ] Flagged staleness if >7 days
- [ ] Offered to start new session entry, didn't auto-start
- [ ] Recommended `handoff-resume` if HANDOFF.md was also present
- [ ] Did NOT modify SESSION_LOG.md

## Example output

```
Last session: 2026-05-07 (2 days ago)

Focus: Auto-import feature from third-party API
Shipped: OAuth working, raw data fetch returning results
Open: Need to map external IDs to internal IDs
Next pickup (per the log): "Start with the ID mapping function in src/lib/import.ts"

Want me to start a new session entry and dive into the mapping function?
```
