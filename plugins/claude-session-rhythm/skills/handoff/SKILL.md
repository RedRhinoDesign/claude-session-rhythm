---
name: handoff
description: Generate or update HANDOFF.md — a state snapshot for picking up clean after a break, or for handing off to a parallel Claude Code agent (Conductor worktree). Two modes: FULL (the default — comprehensive snapshot) and QUICK (5-line minimal snapshot for small wraps). Use FULL when the user says "create a handoff," "write handoff," "wrap up before break," "save state," "prep for parallel agent," or any variant of capturing project state before a real context switch. Use QUICK when the user says "quick handoff," "tiny handoff," "fast snapshot," "just the essentials," or signals a minor wrap (under 30 min away, simple state). Trigger PROACTIVELY before the user spawns a Conductor worktree or starts a long break (>1 day). Produces a single HANDOFF.md that overwrites the previous one — only the LATEST handoff lives, no history.
---

# Handoff

Captures project state in a single, current HANDOFF.md. Unlike SESSION_LOG.md (history) or LESSONS.md (wisdom), HANDOFF is the *one* current state document. Overwrite, don't append.

## Process

This skill has **two modes**. Default to FULL unless the user explicitly asks for "quick." If unsure, ask once: "Full handoff or quick?"

### FULL mode

1. **Gather state.** Pull from:
   - Latest commit hash + message
   - Current branch
   - Last file edited (and any uncommitted changes)
   - Most recent SESSION_LOG.md entry if it exists
   - Whether the dev server is running
   - Open Conductor worktrees, if applicable

2. **Write HANDOFF.md** using this template:

```
# Handoff — [YYYY-MM-DD HH:MM]

**Project:** [name]
**Branch:** [branch name]
**Last commit:** [hash] — [message]

## Current focus
[One paragraph: what's the active task and where it stands]

## What works right now
- 

## What's broken or in-progress
- 

## Failed approaches (DO NOT REPEAT)
[The most valuable section. List things that were tried and didn't work, with WHY they failed. Saves the next session/agent from re-running the same dead ends.]
- Tried [X]. Failed because [Y]. Don't retry unless [condition changes].
- (Or: "(none this session)")

## Where I left off (be specific)
- File: `path/to/file.ext` near line N
- Last command run: `[command]`
- Last test result: [passing / failing / not run]

## Uncommitted changes
[Diff summary or "(none)"]

## Next steps (in order)
1. 
2. 
3. 

## Run to resume working state
```bash
cd ~/Code/[project]
[commands to restart dev server, etc.]
```

## Notes for the next agent / next-me
[Anything weird, fragile, or in-flight that someone resuming wouldn't know]
```

3. **Save it,** overwriting any existing HANDOFF.md.

4. **If a Conductor worktree is being created**: offer to copy this handoff into the new worktree's `.conductor/HANDOFF.md` so the parallel agent starts with full context.

5. **Confirm:** "Handoff saved. Resume command at the top of HANDOFF.md."

### QUICK mode

For minor wraps (under 30 min away, simple state). Same file (HANDOFF.md), much smaller template:

```
# Handoff — [YYYY-MM-DD HH:MM] (quick)

**Branch:** [branch]
**Where I left off:** [one specific sentence]
**Next step:** [one specific sentence]
**Resume with:** `[one command]`
```

Five lines. No "what works," no "what's broken," no detailed file paths. If the situation needs more, it's not actually a quick handoff — switch to FULL.

## Voice

- Specific over polished — file paths and commands, not prose
- Written for a stranger who knows the project but not today's session
- Honest about what's broken — broken things are MORE important to flag than working things
- **Failed approaches are mandatory** — even "(none)" is better than skipping the section. The whole point is preventing the next agent from rerunning the same dead ends.

## Quality checklist

- [ ] Resume commands actually work as written
- [ ] Names specific files/lines, not vague areas
- [ ] Calls out anything fragile or in-flight
- [ ] Failed approaches section is filled in (even with "(none)")
- [ ] Date+time stamped at top
- [ ] OVERWROTE previous handoff, did not append
