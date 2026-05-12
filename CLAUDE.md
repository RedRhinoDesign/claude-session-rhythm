# claude-session-rhythm

A Claude Code plugin providing 7 session management skills and slash commands for maintaining project rhythm across sessions, context switches, and parallel agents.

## Structure

- `skills/` — one folder per skill, each containing a `SKILL.md`
- `commands/` — slash commands organized by namespace (`session/`, `handoff/`, `lessons/`, `decision/`, `docs/`)

## Install

Recommended: install via the Claude Code plugin marketplace.

```text
/plugin marketplace add RedRhinoDesign/claude-session-rhythm
/plugin install claude-session-rhythm
```

For local development, point the marketplace at a clone:

```text
/plugin marketplace add ~/Code/claude-session-rhythm
/plugin install claude-session-rhythm
```

Copy-pasting `skills/` and `commands/` contents into `~/.claude/` still works, but is not the recommended path — `/session:guide` assumes marketplace-style installation when classifying plugin vs. personal skills (see `GOTCHAS.md`).

## Conventions

- Skill names: kebab-case, matching the folder name
- Command namespaces: match the folder name (e.g., `session/start.md` → `/session:start`)
- Skill `description:` frontmatter appears in Claude Code autocomplete — keep it specific and under 200 characters
- See GOTCHAS.md if something isn't triggering as expected
