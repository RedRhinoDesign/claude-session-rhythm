# claude-session-rhythm

A Claude Code plugin providing 9 session management skills and slash commands for maintaining project rhythm across sessions, context switches, and parallel agents.

## Structure

This repo is both a marketplace and a single-plugin catalog (canonical Anthropic layout, cf. `agent-sdk-dev`).

- `.claude-plugin/marketplace.json` — marketplace manifest (root). Points at the plugin via `"source": "./plugins/claude-session-rhythm"`.
- `plugins/claude-session-rhythm/.claude-plugin/plugin.json` — plugin manifest
- `plugins/claude-session-rhythm/skills/` — one folder per skill, each containing a `SKILL.md`
- `plugins/claude-session-rhythm/commands/` — slash commands organized by namespace (`session/`, `handoff/`, `lessons/`, `gotchas/`, `decision/`, `docs/`)

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

Copy-pasting `plugins/claude-session-rhythm/skills/` and `plugins/claude-session-rhythm/commands/` contents into `~/.claude/` still works, but is not the recommended path — `/claude-session-rhythm:session:guide` assumes marketplace-style installation when classifying plugin vs. personal skills (see `GOTCHAS.md`).

## Conventions

- Skill names: kebab-case, matching the folder name
- Command namespaces: match the folder name (e.g., `plugins/claude-session-rhythm/commands/session/start.md` → `/claude-session-rhythm:session:start`)
- Skill `description:` frontmatter appears in Claude Code autocomplete — keep it specific and under 200 characters
- See GOTCHAS.md if something isn't triggering as expected
