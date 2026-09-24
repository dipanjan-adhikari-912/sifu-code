![sifu-code-logo](sifu-code-logo.png)

# Sifu

*He writes one line and says nothing. Ask him why it works, and he'll ask you first.*

Sifu is a skill for AI coding CLIs that combines two things that usually
ship separately:

- **Lazy, minimal code** — before writing anything, the agent climbs a
  laziness ladder: does this need to exist, is it already in the codebase,
  does the standard library do it, does the platform do it natively, is it
  already a dependency, does it fit in one line — and only then writes the
  minimum implementation. Validation, error handling, security, and
  accessibility are never traded away for brevity.
- **Socratic teaching** — when you ask "why" instead of asking for a fix,
  the agent stops generating and starts asking. It never explains before
  it asks what you think, uses a 5-level hint ladder that never skips a
  step, and tracks your progress across sessions so it can pick up where
  you left off.

One ruleset, two modes, switchable with `/sifu [lite|full|ultra|teach|off]`.

## Why one skill instead of two

The instinct behind both halves is the same: don't do work the other side
should be doing. In build mode, that means not writing code the codebase,
stdlib, or platform already provides. In teach mode, it means not handing
you an answer your own reasoning could have reached. A lazy senior dev and
a good teacher are the same personality.

## Install

### Claude Code

```
/plugin marketplace add dipanjan-adhikari-912/sifu-code
/plugin install sifu@sifu
```
(Two separate prompts — the marketplace add has to land before the install.)

Or without the plugin system, copy the skill directly:
```
cp -r skills/sifu ~/.claude/skills/sifu
```

### Codex

```
codex plugin marketplace add dipanjan-adhikari-912/sifu-code
codex plugin add sifu@sifu
```
Skills show up as `@sifu`, `@sifu-review`, etc. Codex's plugin manifest
format moves fast — check `.codex-plugin/plugin.json` against the current
Codex docs if the install command above doesn't pick it up, and adjust the
manifest to match.

Fallback with no plugin: Codex reads `AGENTS.md` from the project root
automatically, so `cp AGENTS.md <your-project>/AGENTS.md` (or
`~/.codex/AGENTS.md` for a global default) gets you the ruleset with no
mode-switch commands.

### Gemini CLI

```
gemini extensions install https://github.com/dipanjan-adhikari-912/sifu-code
```
Loads the ruleset as always-on context and registers the `/sifu` commands.

### OpenCode

Add to your project's `opencode.json`:
```json
{ "plugin": ["./skills/sifu"] }
```
Or point at an absolute path to a shared checkout if you want one install
to serve multiple projects.

### Everything else (Cursor, Windsurf, Cline, Copilot CLI, Amp, Jules, JetBrains Junie, ...)

Most of these hosts auto-load an `AGENTS.md` (or their own equivalent —
Cursor's `.cursor/rules/`, Windsurf's `.windsurf/rules/`, Cline's
`.clinerules/`) from the project root or a global config path. Copy
`AGENTS.md` into whichever path your host reads. This gets you the full
ruleset as always-on instructions; it won't have slash commands, so switch
modes by just telling the agent what you want ("go easy on the ladder,"
"teach me this instead").

## Commands

| Command | What it does |
|---|---|
| `/sifu [lite\|full\|ultra\|teach\|off]` | Set the mode, or report the current one with no argument |
| `/sifu-review` | Review the current diff for over-engineering, hand back a delete-list |
| `/sifu-teach [topic]` | Start a Socratic session on the diff, an open file, or a named topic |
| `/sifu-recap` | State of your teaching-mode progress across sessions |
| `/sifu-help` | Quick reference for these commands |

## Modes

- `lite` — the ladder applies to new code only
- `full` (default) — the ladder applies everywhere; the agent reads before writing
- `ultra` — also flags existing over-engineered code nearby for a delete-list
- `teach` — Socratic mode until you switch back or end the session
- `off` — plain build behavior, no ladder, no teaching

## File structure

```
sifu-code/
├── AGENTS.md                     Fallback ruleset for instruction-only hosts
├── skills/sifu/
│   ├── SKILL.md                  Core skill: both modes, mode switching, commands
│   └── references/
│       ├── sifu-audits.md             Full ladder detail, non-negotiables, worked examples
│       └── socratic-framework.md Hint ladder, detection cascade, progress tracking
├── commands/                     One file per slash command
├── .claude-plugin/                Claude Code plugin + marketplace manifests
├── .codex-plugin/                 Codex plugin manifest
├── gemini-extension.json         Gemini CLI extension manifest
└── opencode.json                 OpenCode plugin config (copy into your project)
```

## Credit

Inspired by two projects with opposite specialties:
[claude-learn-skill](https://github.com/deniz-miro/claude-learn-skill)
(Socratic teaching) and
[ponytail](https://github.com/dietrichgebert/ponytail) (minimal-code
enforcement and multi-CLI portability). Sifu combines the two ideas into
one skill; it isn't a fork of either and doesn't reuse their code.

## License

MIT.
