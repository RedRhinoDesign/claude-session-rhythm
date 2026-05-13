# Changelog

All notable changes to claude-session-rhythm are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/);
versioning follows [SemVer](https://semver.org/spec/v2.0.0.html).

## [1.1.0] — 2026-05-12

### Added
- **`lessons-recall` skill** — on-demand consultation of `~/.claude/LESSONS.md`,
  the cross-project lessons knowledge base. Loads only when Claude judges it
  relevant, keeping the global file out of every session's context by default.
  Read-only; surfaces matching entries verbatim with date + source. Gracefully
  reports the missing-file case without auto-creating.
- **`gotchas-add` skill + `/claude-session-rhythm:gotchas:add` command** —
  captures project-local `GOTCHAS.md` entries with the symptom + fix shape,
  paralleling the existing `lessons-add` flow. Closes the gap noted in
  `GOTCHAS.md` ("no `/gotchas:add` slash command exists").

### Changed
- **`lessons-add` skill now prompts for scope at capture time** — project-local
  vs. global. Routes to `./LESSONS.md` (the 2-sentence prose format) or
  `~/.claude/LESSONS.md` (the structured Context / Observation / Rule /
  Confidence / Source template). Pre-seed the scope by starting the lesson
  text with `project:` or `global:` to skip the prompt. The structured global
  template is auto-drafted from free-form input. Defaults to project-local
  when scope is ambiguous.
- Plugin manifest description updated to reflect 9 skills + 12 slash commands
  and the new cross-project lessons knowledge base.

### Notes
- The `lessons-add` change is **additive, not breaking**: existing invocations
  still work; the only difference is that scope-unspecified invocations get a
  one-question prompt before writing. Power users can keep their muscle memory
  by prefixing `project:` to the lesson text.
- Recommended installer path remains marketplace install via
  `/plugin marketplace add RedRhinoDesign/claude-session-rhythm` followed by
  `/plugin install claude-session-rhythm`. See `README.md` for the four-step
  update pipeline (bump → push → marketplace update → plugin update →
  reload-plugins).

## [1.0.0] — 2026-05-12

### Added
- Initial public release. 7 skills + 11 slash commands maintaining the Session
  Rhythm doc bundle (`HANDOFF.md`, `SESSION_LOG.md`, `LESSONS.md`,
  `DECISIONS.md`, `BACKLOG.md`, `GOTCHAS.md`).
- Install via `/plugin marketplace add RedRhinoDesign/claude-session-rhythm`
  and `/plugin install claude-session-rhythm`.
