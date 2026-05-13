---
name: gotchas-add
description: Quickly append a gotcha to GOTCHAS.md in the current project. Use this skill whenever the user describes a reproducible trap — specific surface symptoms paired with a non-obvious fix — and says things like "watch out for X," "footgun," "fell into trap," "if you see X it actually means Y," "this had me confused for an hour because…," "took me forever to figure out." Trigger PROACTIVELY when the user describes a confusing failure that finally resolved with a specific fix worth recording for future-you. **Distinguishes from `lessons-add`:** gotchas have a "symptom → fix" shape (use this skill); lessons have a "principle → apply" shape (use `lessons-add`). When in doubt, prefer gotchas-add if the entry would help someone *recognize* the trap by its surface symptoms ("if you see error E, check X first"). Project-scoped only — no global GOTCHAS file. Pairs with the `/gotchas:add` slash command for explicit invocation.
---

# Gotchas Add

Captures a reproducible trap to `./GOTCHAS.md` so future-you (or a
collaborator) recognizes the symptom and skips straight to the fix.

The distinguishing shape is **symptom → fix**. If the entry would
read as "if you see X, the real cause is Y, here's how to escape" —
it's a gotcha, log it here. If the entry reads as "I learned that
in general, X" — it's a lesson, use `lessons-add` instead.

## Process

1. **Get the gotcha.** If the user hasn't already stated it clearly,
   ask one question: *"What's the trap, what's the surface symptom
   that should jog memory next time, and what's the fix?"*

2. **Find or create `./GOTCHAS.md`** at the project root. If missing,
   create with this header:

   ```
   # Gotchas

   Non-obvious traps. Read this when something is acting weird.

   ---
   ```

3. **Format the entry** (matches the format observed in existing
   GOTCHAS.md files in this plugin's repo):

   ```
   *YYYY-MM-DD* — One-line title that names the symptom

   2–4 sentences: what the trap is, the surface symptom that flags
   it, and why it's non-obvious (what wrong hypothesis it invites).

   **Fix:** specific steps to escape — not "be aware," actual escape route.

   ---
   ```

   Include verbatim error messages when they exist — they're what
   the searcher's future-self will be googling.

4. **Insert at the top** (just under the header, above the most
   recent existing entry). Show the diff before saving.

5. **Save and confirm:** *"Logged. GOTCHAS.md is now [N] entries."*

## Voice

- Specific symptoms, not vague advice — "if you see error code 401
  with a valid token, X" beats "be careful with auth"
- Verbatim error messages where they exist
- The **Fix:** line should *escape* the trap, not just acknowledge it
- Surface the "wrong hypothesis it invites" explicitly — that's the
  thing that wastes the next hour if the gotcha isn't recorded

## Examples

### Good (symptom + fix)

```
*2026-05-12* — `/plugin update` silently does nothing in the same session as `/plugin install`

If you `/plugin install <name>`, push code changes to the plugin source,
then `/plugin update <name>` in the same Claude Code session — the
update may produce no visible output and no actual file change. The
running Claude Code process holds plugin state in memory from the
install; `/plugin update` doesn't invalidate that in-session.

**Fix:** Fully quit Claude Code (`pkill -f claude` or `⌘Q`), open a
fresh terminal, launch Claude Code, then run `/plugin update <name>`.
Don't retry in the original session — it'll keep being a no-op.

---
```

### Bad (vague — this should go in LESSONS.md if anywhere)

```
*2026-05-12* — Plugin updates can be tricky
Sometimes plugin updates don't work the way you'd expect. Be careful
when testing them and pay attention to caching.
```

(No symptom, no fix — this is "be aware" advice, not a gotcha. If
it's a real generalizable insight, it belongs in LESSONS.md.)

## Quality checklist

- [ ] Specific symptom present (verbatim error / observable behavior)
- [ ] Surface symptom comes first — that's the search trigger
- [ ] **Fix:** line actually escapes the trap (not just "be aware")
- [ ] Date is ISO format (YYYY-MM-DD)
- [ ] One trap per entry
- [ ] Inserted at top, not bottom
- [ ] Showed diff before saving
