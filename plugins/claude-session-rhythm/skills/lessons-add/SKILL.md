---
name: lessons-add
description: Append a timestamped lesson to either the project's LESSONS.md or the cross-project `~/.claude/LESSONS.md`, prompting for scope at capture time. Use this skill whenever the user says "add a lesson," "log this learning," "lesson learned," "remember this," "TIL," "today I learned," "add to lessons," "this was tricky," or describes a "huh, didn't know that" moment in any way that implies it's worth capturing. Trigger PROACTIVELY any time the user expresses surprise, frustration that finally resolved, or a non-obvious insight — offer to log it. The skill asks **project-local or global?** at capture time; pre-seed by starting `$ARGUMENTS` with `project:` or `global:` to skip the prompt. Project-local writes the existing 2-sentence prose format to `./LESSONS.md`; global writes the structured Context/Observation/Rule/Confidence/Source template to `~/.claude/LESSONS.md` (drafted automatically from free-form input). Pairs with `lessons-recall` for the read side — that skill consults the global file on demand, this skill captures new entries.
---

# Lessons Add

Captures a lesson learned, routing it to either the project's
`./LESSONS.md` or the global `~/.claude/LESSONS.md` based on scope.

The two files have **different formats by design**: project lessons
are dense 2-sentence-prose entries optimized for "remind me what I
figured out in this repo"; global lessons are structured (Context /
Observation / Rule / Confidence / Source) optimized for cross-project
pattern recall via the `lessons-recall` skill. This skill knows both
formats and routes automatically.

## Process

### 0. Determine scope (NEW)

Before getting the lesson body, fix the scope.

**a. Parse `$ARGUMENTS` prefix.** If the user's text starts with
`global:` or `project:` (case-insensitive, with or without the
trailing colon — `global ` or `project ` also work), adopt that
scope and strip the prefix from the body. Skip the prompt.

**b. Otherwise, ask the user.** Use AskUserQuestion with this
question and two options:

- *Question:* "Where should this lesson live — project-local (this
  project only) or global (cross-project pattern)?"
- *Project-local:* "Writes to `./LESSONS.md` — the existing 2-sentence-prose format."
- *Global:* "Writes to `~/.claude/LESSONS.md` — the structured Context/Observation/Rule/Confidence/Source template."

**c. Gut-check on "global" selection.** If scope = global —
**regardless of whether it was set via the `$ARGUMENTS` prefix (0a)
or via the prompt (0b)** — surface this one line before continuing:

> Quick check before going global: does this apply to 2+ unrelated
> projects, is it actionable, and isn't already a CLAUDE.md rule or
> standard practice? If "no" to any → switch to project-local.

The gut-check is the safety net, not the prompt. A user who typed
`global:` made an explicit commitment, but a 1-line reminder is
still cheap insurance against polluting the small curated file. If
the user downgrades to project-local here, proceed with project-local
**without** re-asking the scope question. If they confirm global,
continue.

**d. Default on skip / ambiguity:** **project-local.** A wrong
project-local lesson is cheap to undo by a one-line edit promotion;
a wrong global lesson pollutes the small curated cross-project file.
Bias toward the cheaper-to-fix direction.

### 1. Get the lesson

If the user hasn't already stated the lesson clearly, ask one
question: *"What's the one-line takeaway?"*

For global scope, also note the user's free-form text — Step 3 will
auto-draft the structured fields from it.

### 2. Find or create the target file

**Project-local scope** — `./LESSONS.md` at the project root. If
missing, create with this header:

```
# Lessons Learned

Non-obvious things discovered while building this project. Newest at the top.

---
```

**Global scope** — `~/.claude/LESSONS.md`. **Do NOT auto-create** if
missing — instead, stop and tell the user:

> No global lessons file at `~/.claude/LESSONS.md` yet. Create it
> first (see the entry template in `claude-session-rhythm`'s README
> under Global Lessons), then re-run this skill with `global:`.

Seeding the global file is a deliberate setup step, not something to
do mid-capture. Staying consistent with `lessons-recall`'s
report-and-link behavior.

### 3. Format the entry

**Project-local format** (unchanged from prior behavior):

```
*YYYY-MM-DD* — One-line lesson title
[2–3 sentences explaining what happened, what was confusing, and the takeaway.]

---
```

**Global format** (matches the structured template documented in
`~/.claude/LESSONS.md`):

```
*YYYY-MM-DD* | [category: workflow / tooling / planning / debugging / other]

**Context:** [one sentence: what situation surfaced this]
**Observation:** [what you noticed]
**Rule:** [specific, actionable pattern — "avoid X because Y" or "always do X when Z"]
**Confidence:** high / medium / low
**Source:** [project or session name]

---
```

**For global scope, draft these fields automatically** from the
user's free-form text rather than asking five separate questions:

- **Context** — infer from the lesson body ("what was the situation?")
- **Observation** — infer ("what did you notice?")
- **Rule** — infer the actionable pattern (this is the key field —
  make it crisp; if the lesson body doesn't yield a clear rule, ask
  one targeted follow-up before drafting)
- **Category** — guess from content (workflow / tooling / planning /
  debugging / other)
- **Confidence** — default to "medium"; ask only if the Rule is
  strong enough that "high" seems warranted
- **Source** — default to the current project name (basename of CWD)

### 4. Insert at the top, show the diff

Insert the new entry just under the header section, above the most
recent existing entry. Show the diff before saving.

### 5. Save and confirm

For project-local: *"Logged. ./LESSONS.md is now [N] entries."*
For global: *"Logged globally. ~/.claude/LESSONS.md is now [N]
entries — consult later with `lessons-recall`."*

## Voice

- Plainspoken — write the way you would say it to a friend
- Specific over abstract — "I assumed X but Y was actually true"
  beats "be careful with X"
- No jargon unless the jargon IS the lesson
- One thought per entry — split into multiple if there are
  multiple lessons
- For global Rule lines: prefer imperative shape ("Always X when Y"
  or "Avoid X because Y") — they read as rules, not anecdotes

## Examples

### Project-local

**Bad (vague):**
```
*2026-05-05* — Always check things
Sometimes things don't work as expected. Be careful.
```

**Good (specific):**
```
*2026-05-05* — Vercel env vars don't auto-reload locally
Spent 40 min wondering why the new SUPABASE_URL wasn't working. Turns
out `vercel dev` caches env vars at boot — you have to restart the
dev server after changing .env.local. The browser auto-reload tricked
me into thinking the env was hot-reloading too.
```

### Global

```
*2026-05-12* | workflow

**Context:** Releasing a plugin update; ran `/plugin update` but saw no visible output and no file changes.
**Observation:** The release pipeline is four steps in order (version bump → push → marketplace update → plugin update → reload-plugins); skipping any step makes the next look broken.
**Rule:** When a release "didn't work," check each prior step's exit state before re-running the failing step. Never test step N without verifying step N-1.
**Confidence:** high
**Source:** claude-session-rhythm
```

## Quality checklist

- [ ] Scope determined (prefix-parsed, prompted, or defaulted)
- [ ] Gut-check surfaced if scope = global
- [ ] Specific, not abstract
- [ ] Date is ISO format (YYYY-MM-DD)
- [ ] One lesson per entry
- [ ] For global: Rule line is imperative and actionable
- [ ] For global: Source field defaults to current project name
- [ ] Inserted at top, not bottom
- [ ] Showed diff before saving
