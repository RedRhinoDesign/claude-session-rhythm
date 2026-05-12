---
name: lessons-add
description: Quickly append a lesson learned to LESSONS.md in the current project. Use this skill whenever the user says "add a lesson," "log this learning," "lesson learned," "remember this," "TIL," "today I learned," "add to lessons," "this was tricky," or describes a "huh, didn't know that" moment in any way that implies it's worth capturing. Trigger PROACTIVELY any time the user expresses surprise, frustration that finally resolved, or a non-obvious insight while working on a project — proactively offer to log it. Appends a clean, timestamped entry to the top of LESSONS.md (creates the file if missing, with a starter header).
---

# Lessons Add

## Process

1. **Get the lesson.** If the user hasn't already stated it clearly, ask one question: "What's the one-line takeaway?"

2. **Find or create LESSONS.md.** Check the project root. If missing, create with this header:
```
# Lessons Learned

Non-obvious things discovered while building this project. Newest at the top.

---
```

3. **Format the entry:**
```
*[YYYY-MM-DD]* — [One-line lesson title]
[2–3 sentences explaining what happened, what was confusing, and what the takeaway is.]

---
```

4. **Insert at the top** (just under the header, above the most recent existing entry). Show the diff before saving.

5. **Save and confirm:** "Logged. LESSONS.md is now [N] entries."

## Voice

- Plainspoken — write the way you would say it to a friend
- Specific over abstract — "I assumed X but Y was actually true" beats "be careful with X"
- No jargon unless the jargon IS the lesson
- One thought per entry — split into multiple if there are multiple lessons

## Examples

**Bad (vague):**
```
*2026-05-05* — Always check things
Sometimes things don't work as expected. Be careful.
```

**Good (specific):**
```
*2026-05-05* — Vercel env vars don't auto-reload locally
Spent 40 min wondering why the new SUPABASE_URL wasn't working. Turns out `vercel dev` caches env vars at boot — you have to restart the dev server after changing .env.local. The browser auto-reload tricked me into thinking the env was hot-reloading too.
```

## Quality checklist

- [ ] Specific, not abstract
- [ ] Date is ISO format (YYYY-MM-DD)
- [ ] One lesson per entry
- [ ] Inserted at top, not bottom
- [ ] Showed diff before saving
