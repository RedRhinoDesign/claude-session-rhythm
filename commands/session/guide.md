---
name: guide
description: Session Rhythm workflow guide — shows when to reach for each skill, grouped by stage (setup, starting, capturing, wrapping up, resuming). Separates plugin skills from personal skills.
---

Print a workflow-organized guide to the installed session-rhythm skills and commands. The goal is to show WHEN to reach for each tool — autocomplete already shows the inventory.

## Steps

1. List all folders under `~/.claude/skills/` and all files/folders under `~/.claude/commands/`.

2. For each skill, read the `~/.claude/skills/<name>/SKILL.md` file and extract the first sentence of the `description:` frontmatter field.

3. Treat every folder in `~/.claude/skills/` as a **personal skill**. Under the Claude Code plugin install model, plugin skills live under the plugin's own install directory — not under `~/.claude/skills/` — so no filtering against a plugin allow-list is needed. (If a user copy-pasted plugin skills into `~/.claude/skills/` instead of using `/plugin marketplace add`, they'll show up in the personal-skills section; that's a setup quirk, not a bug.)

4. Print the guide using the format below.

## Output format

```
# Session Rhythm Guide

## SETUP — First time in a new project
  /docs:init        — scaffold the Session Rhythm doc bundle (LESSONS, SESSION_LOG, HANDOFF, DECISIONS, BACKLOG, GOTCHAS, and a project CLAUDE.md if missing)

## STARTING — Entering a session with existing context
  /handoff:resume   — when HANDOFF.md exists: reorient from the last state snapshot
  /session:resume   — when no HANDOFF.md but SESSION_LOG.md exists: catch up from the last session entry
  /session:start    — open a new SESSION_LOG entry for this session

## CAPTURING — Mid-session insights, decisions, lessons
  /lessons:add      — when something surprised you, frustrated you, or finally clicked
  /decision:add     — when you land on a structural or architectural choice worth preserving

## WRAPPING UP — Before a break, context switch, or parallel agent spawn
  /handoff:create   — full state snapshot: what works, what's broken, what failed, next step
  /handoff:quick    — 5-line minimal snapshot for short breaks
  /session:end      — close the current SESSION_LOG entry

## RESUMING — Picking up after a break
  /handoff:resume   — reads HANDOFF.md and gets you back to productive work fast
  /session:resume   — reads the latest SESSION_LOG entry if no HANDOFF.md

---
Tip: type / in Claude Code to see all commands. This guide shows the rhythm, not the inventory.
```

If any skills exist under `~/.claude/skills/`, append this section before the tip line:

```
## Personal Skills (yours only — not in the plugin)
  /<name>   — <first sentence of description>
  ...
```

Keep each description to one line focused on WHEN, not WHAT.
