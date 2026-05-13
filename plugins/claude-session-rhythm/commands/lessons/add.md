---
description: Append a timestamped lesson to LESSONS.md (project-local or global ~/.claude/LESSONS.md, asked at capture time).
---

Run the `lessons-add` skill. The skill will ask whether this lesson is
project-local (writes to `./LESSONS.md`) or global (writes to
`~/.claude/LESSONS.md`); skip the prompt by prefixing the lesson text
with `project:` or `global:`. If I haven't stated the lesson yet, ask
me for it in one line. Insert at the top of the chosen file. Show the
diff before saving.

Lesson context (optional): $ARGUMENTS
