# claude-session-rhythm

A Claude Code plugin for picking up exactly where you left off — every session, every break, every parallel agent.

Adds a small, opinionated set of project docs (`HANDOFF.md`, `SESSION_LOG.md`, `LESSONS.md`, `DECISIONS.md`, `GOTCHAS.md`, `BACKLOG.md`) and the skills + slash commands that maintain them as a rhythm. Designed so Claude Code can re-orient in a project in under a minute, without you re-explaining context you already explained yesterday.

## Why?

Claude Code sessions die. Context windows compact. You switch projects, take overnight breaks, spawn a Conductor worktree for a parallel agent. Without disciplined state files, you re-onboard Claude from scratch every single time — and the things you learned last week quietly evaporate.

Session Rhythm is a set of conventions that fix this with the lowest-friction artifacts possible:

- **`HANDOFF.md`** — one *current* state snapshot. Overwritten, never appended. Read at session start, written before a break.
- **`SESSION_LOG.md`** — compact history of what happened, when. One entry per focused session.
- **`LESSONS.md`** — non-obvious things discovered, newest at top. Saves you from re-debugging the same wall twice.
- **`DECISIONS.md`** — lightweight ADRs. The *why* behind structural choices, so future you doesn't re-litigate them.
- **`GOTCHAS.md`** — traps. Read this when something is acting weird.
- **`BACKLOG.md`** — things you've deliberately deferred.

The skills and commands in this plugin make maintaining these files take seconds, not minutes — so the rhythm actually sticks.

## Install

The plugin is published as a Claude Code plugin. Install via the marketplace:

```text
/plugin marketplace add RedRhinoDesign/claude-session-rhythm
/plugin install claude-session-rhythm
```

Or, while developing locally, point the marketplace at a clone:

```text
/plugin marketplace add ~/Code/claude-session-rhythm
/plugin install claude-session-rhythm
```

That's it. Restart Claude Code if a slash command doesn't appear in autocomplete immediately.

> **Note:** copy-pasting the `plugins/claude-session-rhythm/skills/` and `plugins/claude-session-rhythm/commands/` folders into `~/.claude/` also works, but the marketplace install is the recommended path. `/session:guide` assumes marketplace-style installation when categorizing plugin vs. personal skills — see `GOTCHAS.md` for the edge case.

## Slash commands

All commands are namespaced. Type `/` in Claude Code and they'll autocomplete with descriptions.

| Command | What it does |
| --- | --- |
| `/session:start [focus]` | Begin a new `SESSION_LOG.md` entry for today's work. |
| `/session:end` | Close out the current session entry — what shipped, what's next. |
| `/session:resume` | Read the most recent `SESSION_LOG.md` entry and reorient. |
| `/session:guide` | Show the full Session Rhythm workflow guide, grouped by stage. |
| `/handoff:create` | Write a full `HANDOFF.md` snapshot for a real context switch. |
| `/handoff:quick` | Five-line `HANDOFF.md` for a small wrap (<30 min away). |
| `/handoff:resume` | Read `HANDOFF.md`, drift-check, and orient for continued work. |
| `/lessons:add` | Append a timestamped lesson to `LESSONS.md`. |
| `/decision:add` | Append a lightweight ADR entry to `DECISIONS.md`. |
| `/docs:init` | Scaffold the Session Rhythm doc bundle in this project — `LESSONS.md`, `SESSION_LOG.md`, `HANDOFF.md`, `DECISIONS.md`, `BACKLOG.md`, `GOTCHAS.md`, and a project-level `CLAUDE.md` if missing. Confirms before writing; never overwrites. |
| `/docs:audit` | Audit doc-bundle coverage across a directory of projects — shows which of the seven standard docs each project has and what's missing. Read-only. |

## Skills

The commands above are thin wrappers around skills. The skills also trigger proactively when Claude detects a relevant moment in a conversation (e.g., you express a "huh, didn't know that" — `lessons-add` offers to capture it).

| Skill | Trigger gist |
| --- | --- |
| `session-log` | Start or end a focused session entry. |
| `session-resume` | Catch me up — what was I doing last time? |
| `handoff` | Write `HANDOFF.md` before a real break or parallel-agent spawn. |
| `handoff-resume` | Read `HANDOFF.md` and re-orient cleanly. |
| `lessons-add` | Capture a lesson learned the moment it lands. |
| `decision-record` | Capture the *why* of a structural choice. |
| `docs-init` | Scaffold the seven-file doc bundle (`CLAUDE`, `LESSONS`, `SESSION_LOG`, `HANDOFF`, `DECISIONS`, `BACKLOG`, `GOTCHAS`) in a new or existing project. |

Full triggering criteria and process are in each skill's `SKILL.md`.

## Usage

A typical project rhythm looks like this:

### First time in a project

```text
/docs:init
```

Scaffolds the doc bundle in the current project root: `LESSONS.md`, `SESSION_LOG.md`, `HANDOFF.md`, `DECISIONS.md`, `BACKLOG.md`, `GOTCHAS.md`, and a project-level `CLAUDE.md` if one doesn't already exist. Each file is created with a minimal seed header — they're meant to be lived in, not finished documents. The skill skips any file that already exists (never overwrites) and asks one question along the way: solo workflow or parallel-agent/team workflow? That determines whether `HANDOFF.md` is gitignored (solo) or committed (parallel-agent — it's the literal medium of handoff).

### Starting a focused session

```text
/session:start "wiring stripe checkout"
```

Adds a dated entry at the top of `SESSION_LOG.md`. If a `HANDOFF.md` already exists, Claude will offer to `/handoff:resume` first — take it; that's the cheapest way to load context.

### Mid-session

Just work. Claude will *offer* to log a lesson, capture a decision, or update the handoff at the right moments — you don't have to remember the commands.

If you want to drive it explicitly:

```text
/lessons:add "vercel env vars don't hot-reload — restart dev server after editing .env.local"
/decision:add
```

### Before a break, context switch, or Conductor worktree

```text
/handoff:create
```

Writes a full `HANDOFF.md`: goal, what works, what's broken, failed approaches (loudly — so they don't get retried), next steps, resume commands. For tiny wraps (<30 min away), use `/handoff:quick` instead.

### Picking up next time

Open Claude Code in the project. If `HANDOFF.md` exists, Claude will proactively offer to read it. Or:

```text
/handoff:resume
```

This reads the handoff, drift-checks against the current working tree, surfaces failed approaches loudly, and asks before running any resume commands.

## What the files look like

### `HANDOFF.md` (excerpt)

```markdown
# Handoff — 2026-05-12 14:00

**Project:** claude-session-rhythm
**Branch:** none — repo not git-initialized yet

## Current focus

Plugin manifest is now in place. Remaining work before public release:
README, local install test, then `git init` + push.

## What works right now

- `.claude-plugin/plugin.json` — spec-pure metadata-only manifest
- 7 skills synced to `~/.claude/skills/`
- 11 commands across 5 namespaces

## What's broken or in-progress

- No README — highest-priority remaining item
- No git repo yet — `git init` not run
- No local install test

## Failed approaches (DO NOT REPEAT)

- Enumerating skill/command names in plugin.json — not in the spec
- Hardcoding a plugin-skill allow-list in /session:guide

## Next steps

1. Write README (in progress)
2. Local install test
3. git init + push

## Run to resume working state

cd ~/Code/claude-session-rhythm
```

### `LESSONS.md` (excerpt)

```markdown
# Lessons Learned

Non-obvious things discovered while building this project. Newest at the top.

---

*2026-05-08* — Vercel env vars don't hot-reload in local dev

Spent 40 minutes wondering why a newly added DATABASE_URL wasn't being
picked up. `vercel dev` caches env vars at startup — restart the dev
server after editing .env.local.

---

*2026-05-05* — Sandbox a scaffolding tool before releasing it

Ran the docs-init skill against a fresh empty project before shipping.
That dry-run immediately exposed a design question that would have
shipped as a silent wrong default.

---
```

See `LESSONS.md.example` and `DECISIONS.md.example` in the repo for full templates.

## Opinions baked in

A few choices that are deliberate, not accidental:

- **`HANDOFF.md` is one file, overwritten every time.** History lives in `SESSION_LOG.md`. Two files, two jobs — don't merge them.
- **Newest at the top, always.** Dated entries, no chronological hunting.
- **Gitignore the doc bundle by default.** This project does — see `.gitignore`. The doc bundle is most useful as personal/team operational notes, not a public-facing artifact. Reverse it per-project if your team wants them committed; just pick one and stay consistent. (`/docs:init`'s solo-vs-parallel-agent question handles `HANDOFF.md` specifically, since it's the one file whose role flips between the two modes.)
- **One lesson = one entry.** Don't pile three unrelated takeaways into one timestamp. Future-you scans by date+headline.
- **Capture the *why* of a decision, not the *what*.** The diff already shows the what. ADR entries that skip the alternatives-considered and tradeoffs sections are worth less than nothing.
- **Failed approaches go in the handoff, loudly.** The most expensive context loss is re-trying something that already didn't work. `/handoff:create` always asks about this section.
- **Skills are proactive.** They offer at the right moments. Don't suppress them — that's the whole point.

## Project conventions (for contributors)

- Plugin files live under `plugins/claude-session-rhythm/` — the canonical Anthropic marketplace layout (cf. `agent-sdk-dev`). The marketplace manifest stays at the repo root (`.claude-plugin/marketplace.json`); the plugin manifest is `plugins/claude-session-rhythm/.claude-plugin/plugin.json`.
- Skill names: kebab-case, matching the folder name. Each skill lives in `plugins/claude-session-rhythm/skills/<name>/SKILL.md`.
- Command namespaces: match the folder name. `plugins/claude-session-rhythm/commands/session/start.md` → `/session:start`.
- Skill `description:` frontmatter shows up in Claude Code autocomplete — keep it specific and concrete.
- Slash commands are intentionally thin — most logic lives in the skill they invoke.

The plugin manifest at `plugins/claude-session-rhythm/.claude-plugin/plugin.json` is spec-pure metadata-only — it does *not* enumerate skill or command names. Skills and commands are auto-discovered from their folders. If you add or remove a command, you don't need to touch the manifest.

## Compatibility

Tested on Claude Code (CLI). The plugin uses only standard plugin features (`skills/`, `commands/`, and `.claude-plugin/plugin.json` inside its plugin subdirectory) — no platform-specific assumptions.

## License

MIT. See `LICENSE`.

Designed with reference to [willseltzer/claude-handoff](https://github.com/willseltzer/claude-handoff).
