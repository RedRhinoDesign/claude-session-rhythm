---
name: session-log
description: Append a session entry to SESSION_LOG.md at the start or end of a focused work session. Use this skill whenever the user says "start session," "starting work on X," "log this session," "end session," "wrap session," "session done," "log session ending," or otherwise signals starting or ending focused project work. Trigger PROACTIVELY at the start of any project-focused conversation that lasts more than a few exchanges (offer to start a session entry) and at the natural end of a session (offer to wrap it). Maintains SESSION_LOG.md as a compact, scannable journal of work cadence.
---

# Session Log

Maintains a compact session journal so future you (or a parallel agent) can see what's been happening day-to-day in a project.

## Two modes

### Start mode

Triggered by: "start session," "starting work," "kicking off," etc.

1. Find or create SESSION_LOG.md in project root.
2. Add a new entry at the top:
```
## [YYYY-MM-DD] — [pending: focus]

**Started:** [HH:MM]
**Worked on:** 
**Shipped:** 
**Blocked / open questions:** 
**Next session pickup:** 

---
```
3. Ask: "What's the focus for this session in 5 words or less?"
4. Update the placeholder with the answer.
5. Confirm: "Session logged. I'll fill in the rest when we wrap."

### End mode

Triggered by: "wrap session," "end session," "we're done," etc.

1. Find the most recent entry (top of file).
2. Review what happened in the session.
3. Fill in:
   - **Worked on:** 1–3 focus areas
   - **Shipped:** concrete outputs (commits, features, fixes) — empty bullet "(none)" is fine
   - **Blocked / open questions:** anything unresolved
   - **Next session pickup:** 1–3 bullets of where to start next time
4. Add **Ended:** [HH:MM] to the entry.
5. Show the completed entry. After approval, save it.
6. **Cross-link**: if entries were also added to LESSONS.md or DECISIONS.md during this session, mention them: "See also: LESSONS.md, DECISIONS.md".

## Voice

- Compact — bullets, not paragraphs
- Specific commits/files/decisions over vague summaries
- Honest — empty "Shipped" is fine when nothing actually shipped
- Past tense for completed entries

## Quality checklist

- [ ] Date format is ISO (YYYY-MM-DD)
- [ ] Each section has at least one bullet OR explicit "(none)" — never blank
- [ ] No conversation transcripts — distill to wins/blockers/next
- [ ] Entry stays ≤ 15 lines
