---
name: audit
description: Audit Session Rhythm doc-bundle coverage across all projects in a directory — shows which standard docs (CLAUDE, LESSONS, SESSION_LOG, HANDOFF, DECISIONS, BACKLOG, GOTCHAS) each project has and recommends next actions. Read-only, no files created or modified.
---

Audit Session Rhythm doc-bundle coverage across the projects in the specified directory. If the user hasn't provided a path, ask: "Which directory should I scan? (e.g. `~/Code` or `~/projects`)"

For each subdirectory that looks like a project (contains any of: `.git/`, `package.json`, `CLAUDE.md`, `pyproject.toml`, `Gemfile`, `Cargo.toml`, or a clear app structure):

1. List the project name (folder name).
2. Check the project root for these seven standard doc-bundle files:
   - CLAUDE.md
   - LESSONS.md
   - SESSION_LOG.md
   - HANDOFF.md
   - DECISIONS.md
   - BACKLOG.md
   - GOTCHAS.md
3. Mark each as ✅ present or ❌ missing.

Output a markdown table:

| Project | CLAUDE | LESSONS | SESSION_LOG | HANDOFF | DECISIONS | BACKLOG | GOTCHAS |
|---|---|---|---|---|---|---|---|

After the table, add a section **"Recommended action per project"**:
- Projects missing 3+ files: suggest running `/claude-session-rhythm:docs:init` in that folder.
- Projects missing 1–2 files: list the specific files and offer to create them.
- Projects with all seven: mark ✅ fully covered.

Do NOT create or modify any files. This is read-only — just report.
