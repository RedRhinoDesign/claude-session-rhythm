---
name: docs-init
description: Scaffold the Session Rhythm project doc bundle in a new or existing project — LESSONS.md (non-obvious learnings), SESSION_LOG.md (compact session history), HANDOFF.md (current-state snapshot for breaks and parallel agents), DECISIONS.md (lightweight ADRs — the *why* behind structural choices), BACKLOG.md (deferred work), GOTCHAS.md (traps to avoid), and a project-level CLAUDE.md if missing. Use this skill whenever the user wants to set up project documentation, mentions "init docs," "scaffold project docs," "set up documentation," "add the standard files," "I'm starting a new project," "audit which docs this project has," or migrates an existing project to standardized docs. Trigger PROACTIVELY whenever entering a project folder that's missing core docs (e.g., no LESSONS.md, no SESSION_LOG.md). Always check what already exists before writing — never overwrite without explicit confirmation.
---

# Doc-Bundle Scaffolder

Sets up the Session Rhythm doc bundle in a project — a consistent, opinionated set of markdown files so future you and parallel Claude Code agents can pick up context fast.

The bundle (seven files):

- **`CLAUDE.md`** — project-level instructions for Claude Code (tech stack, run commands, conventions). Only created if missing.
- **`LESSONS.md`** — non-obvious learnings, newest at top.
- **`SESSION_LOG.md`** — compact history of focused work sessions.
- **`HANDOFF.md`** — single current-state snapshot. Overwritten each wrap; read at session start.
- **`DECISIONS.md`** — lightweight ADRs. The *why* behind structural choices.
- **`BACKLOG.md`** — deferred work, bucketed Now / Next / Later / Blocked.
- **`GOTCHAS.md`** — traps to read when something acts weird.

## Process

This skill writes files into the **current working directory** — the project root of the active Claude Code session. Always operate on that folder, never on a sibling or parent.

1. **For each of the seven standard files** (CLAUDE.md, LESSONS.md, SESSION_LOG.md, HANDOFF.md, DECISIONS.md, BACKLOG.md, GOTCHAS.md): check if it already exists in the project root. If it exists, skip it — never overwrite.

2. **Ask about HANDOFF.md commit behavior** (skip if HANDOFF.md already exists). One question: **"Solo workflow or parallel-agent / team workflow?"**
   - **Solo (default):** HANDOFF.md is personal scratch — overwritten every wrap. Add `HANDOFF.md` to `.gitignore` (create the file if missing; append the line if it exists and the line isn't already there). The other six docs stay committed.
   - **Parallel-agent / team:** HANDOFF.md is the literal medium of handoff between agents/teammates — leave it committed alongside the rest. Do NOT touch `.gitignore`.

   Rationale captured in DECISIONS.md (2026-05-11): HANDOFF.md is ephemeral and overwritten on every wrap, so committing creates churn for solo users — but it's the literal medium of handoff for parallel agents, so it must be committed there.

3. **Write only the missing files** using the templates below. These are seeds, not finished docs — keep them short. Apply the `.gitignore` change from step 2 if solo.

4. **Confirm.** Tell the user the list of files created, which were skipped (already existed), and whether HANDOFF.md was gitignored.

## Templates

### LESSONS.md
```
# Lessons Learned

Non-obvious things discovered while building this project. Newest at the top.

---
```

### SESSION_LOG.md
```
# Session Log

Compact record of focused work sessions. Newest at the top.

---
```

### HANDOFF.md
```
# Handoff

State snapshot. Overwritten each time — only the LATEST handoff lives here.

(Run the `handoff` skill to populate.)
```

### DECISIONS.md
```
# Decisions

Lightweight ADR (Architecture Decision Record). Captures the WHY behind choices that are hard to reverse. Newest at the top.

---
```

### BACKLOG.md
```
# Backlog

## Now (this week)
- [ ] 

## Next (this month)
- [ ] 

## Later (someday)
- [ ] 

## Blocked
- [ ] 
```

### GOTCHAS.md
```
# Gotchas

Non-obvious traps. Read this when something is acting weird.

---
```

### CLAUDE.md (only if missing)
Minimal project-level template. Defer to the global `~/.claude/CLAUDE.md` for personal preferences. Project CLAUDE.md should ONLY include:
- Tech stack (one section)
- How to run dev server / tests / deploy (one section)
- Project-specific conventions (one section)
- Pointer to GOTCHAS.md

Keep under 60 lines.

## Quality checklist

- [ ] Did NOT overwrite any existing file without explicit go-ahead
- [ ] All templates are committed-friendly (no secrets, no fake placeholder data)
- [ ] Asked solo vs parallel-agent before writing HANDOFF.md (skipped if HANDOFF.md already existed)
- [ ] If solo: appended `HANDOFF.md` to `.gitignore` exactly once (didn't duplicate an existing line)
- [ ] Used ISO date format (YYYY-MM-DD) wherever dates appear
- [ ] Confirmed creation list and gitignore status to the user at the end
