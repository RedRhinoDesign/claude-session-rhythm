---
name: lessons-recall
description: Consult `~/.claude/LESSONS.md` — the cross-project lessons knowledge base — when the user is making an architectural choice, weighing alternatives between approaches, hitting a wall that feels like one hit in a *different* project, or about to take an action with cross-project consequences (release rituals, infra config, dual-use public/private boundaries, plugin lifecycle, etc.). Trigger PROACTIVELY only when you suspect a CROSS-project pattern applies — not for routine debugging, single-project facts, implementation details, or general questions about this codebase. Also trigger when the user explicitly says "have I done this before," "what did I learn last time," "check the global lessons," "is there a global lesson for this," "consult the lessons file," or any variant of recalling cross-project patterns. READ-ONLY — does NOT write or modify the file. Pairs with `lessons-add` (global scope) for capture. If `~/.claude/LESSONS.md` does not exist, the skill reports that gracefully — it does NOT auto-create the file.
---

# Lessons Recall

The read-side companion to `lessons-add`. Consults the cross-project
lessons knowledge base at `~/.claude/LESSONS.md` and surfaces only the
entries that actually apply to what the user is doing right now.

The point is progressive disclosure: the global lessons file is **not**
loaded into every session's context by default. This skill loads it on
demand, so its content shows up exactly when it would change the user's
next move — and stays out of context the rest of the time.

## Process

1. **Existence check.** Look for `~/.claude/LESSONS.md`. If it doesn't
   exist, say so plainly and stop:

   > No global lessons file at `~/.claude/LESSONS.md` yet. To start one,
   > use the entry template documented inside the file once you create
   > it (or see `claude-session-rhythm`'s README under Global Lessons).

   Do **NOT** auto-create the file. Seeding it is a setup-time decision,
   not a recall-time one.

2. **Read the entire file in one call.** It's small by design (≤60 lines
   per its own pruning guidance), so loading the whole thing is cheap.

3. **Identify the user's current context.** Be specific: what decision
   are they making? What's the problem domain? What action is imminent?
   Anchor on what the user just said, not on general guesses about their
   project.

4. **Match entries to the context.** For each entry in the file,
   evaluate semantically: would this entry's **Rule** line inform the
   user's next move? Match on *meaning*, not keyword overlap — a
   lesson about "trust documented intent without verification" might
   apply to a database migration discussion even though neither word
   appears.

5. **Surface matches verbatim.** Quote the **Rule** line (and the
   **Context** if it adds disambiguation), with the date and **Source**
   from the entry. Don't paraphrase — the rule's specific phrasing is
   its value. Format:

   > **Match** — *YYYY-MM-DD* | *category* (source: *project name*)
   > Rule: "*…the exact rule text…*"
   > Why it applies: *one sentence connecting the rule to the current
   > situation.*

6. **If no matches, say so plainly.** "No applicable global lessons (N
   entries scanned)." Don't pad with vaguely related items — false
   matches train the user to ignore the skill's output.

7. **Soft capture prompt.** End with one line, only if a match
   surfaced or a clear new pattern emerged:

   > If this turns into a new cross-project rule, capture it with
   > `lessons-add` and choose the **global** scope.

## Voice

- Concise — the user is consulting mid-decision, not reading prose.
- Verbatim — quote matching rules; don't summarize them.
- Honest — explicit no-match beats padded relevance.
- Read-only — never offer to edit or "improve" the global file from
  this skill. That's `lessons-add`'s job.

## Examples

### Match example

**User:** "I'm about to drop and recreate a table in dev to clean up
some old schema cruft. The migration script's been sitting unused for
weeks — should I just run it?"

**Skill output:**

> Checking `~/.claude/LESSONS.md`…
>
> **Match** — *2026-05-11* | planning (source: claude-session-rhythm Phase 4/5 cleanup)
> Rule: "For any dual-use project, decide and enforce public/private
> file boundaries before drafting starts — verify current file state
> matches stated intent before planning any work. Don't trust
> documented intent without verification."
> Why it applies: the migration script is documented as ready, but
> "documented intent" ≠ "current state" — verify the script against
> the live schema before running it.
>
> If this turns into a new cross-project rule, capture it with
> `lessons-add` and choose the **global** scope.

### No-match example

**User:** "Adding a 'remember me' checkbox to the login form."

**Skill output:**

> Checking `~/.claude/LESSONS.md`…
>
> No applicable global lessons (2 entries scanned).

(No soft capture prompt — nothing new emerged.)

## Quality checklist

- [ ] Existence checked before reading
- [ ] Whole file read in one call (it's small)
- [ ] Matching reasoned semantically, not by keyword grep
- [ ] Quoted matches are verbatim, with date + source
- [ ] No-match cases said plainly, without padding
- [ ] Zero writes attempted — this skill is read-only
