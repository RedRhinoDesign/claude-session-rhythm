---
name: handoff-resume
description: Read the existing HANDOFF.md and reorient the agent to continue the work cleanly. Use this skill at the START of any Claude Code session in a project that has a HANDOFF.md file, OR whenever the user says "resume," "pick up where I left off," "continue from handoff," "read the handoff," "what was I doing," "load handoff," "where am I in this project," or any variant of resuming after a break or context switch. Trigger PROACTIVELY when entering a project folder that has a HANDOFF.md present and the user hasn't yet stated their intent — offer to load it. Does NOT write or modify HANDOFF.md; that's the `handoff` skill's job.
---

# Handoff Resume

The inverse of the `handoff` skill. Where `handoff` writes state, this reads state and gets the agent (and you) back to productive work fast.

## Process

1. **Confirm a handoff exists.** Check for HANDOFF.md in the project root. If missing, tell the user and stop — there's nothing to resume from.

2. **Read it fully.** Don't just skim the first section.

3. **Drift check.** Compare the handoff against current state:
   - Is the working tree clean, or are there uncommitted changes?
   - Is the branch the same as what HANDOFF.md says?
   - Is the latest commit the same hash, or has work happened since?
   - If drift detected, flag it: "The handoff says branch X but you're on Y" — let the user decide whether to switch or override.

4. **Summarize back to the user.** Three short sections:
   - **Goal:** one sentence on what we were working toward
   - **Where it stands:** what's working, what's broken, what failed approaches to avoid
   - **Next step:** the single most important thing to do first

5. **Offer to run the resume commands.** If HANDOFF.md has a `Run to resume working state` block (e.g., `cd ~/Code/x && npm run dev`), offer to execute it. Always confirm before running.

6. **Hand control to the user.** End with: "Ready when you are. Want to start with [first next step], or pivot?"

## Voice

- Calm and oriented, not breathless — the handoff did the work, just relay it
- Concrete: name files, branches, commits, errors verbatim
- Surface failed approaches LOUDLY so they're not retried
- If the handoff is thin or unclear, say so — don't paper over gaps

## Quality checklist

- [ ] Read the entire HANDOFF.md, not just the top
- [ ] Performed drift check between handoff and current state
- [ ] Summary fits in ≤ 8 lines
- [ ] Failed approaches surfaced explicitly
- [ ] Asked before running any resume commands
- [ ] Did NOT modify HANDOFF.md (that's `handoff` skill's job)

## Example output

```
Handoff loaded — last saved 2026-05-08 16:42.

Goal: Add Stripe checkout to the app's purchase flow.

Where it stands:
  ✅ Checkout button renders, posts to /api/checkout
  ✅ Stripe test keys wired into .env
  ❌ Webhook handler returning 401 — signature verification failing
  ⚠️ Don't retry: tried regenerating webhook secret in Stripe dashboard, made it worse

Next step: debug webhook signature mismatch in src/api/stripe-webhook.ts:34.

Branch matches (feature/checkout). No drift.

Run to resume:
  cd ~/Code/my-project && npm run dev

Want me to run that and start on the webhook bug?
```
