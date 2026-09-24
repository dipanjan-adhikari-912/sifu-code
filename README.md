# Sifu

> He writes one line and says nothing. Ask him why it works, and he'll ask you first.

![sifu](<ChatGPT Image Sep 24, 2026, 09_08_20 AM.png>)

Sifu is a skill for AI coding CLIs that combines two things that usually ship separately:

- **Mindful, minimal code** — before writing anything, the agent checks: does this need to exist, is it already in the codebase, does the standard library do it, does the platform do it natively, is it already a dependency, does it fit in one line — and only then writes the minimum implementation. Validation, error handling, security, and accessibility are never traded away for brevity.
- **Socratic teaching** — when you ask "why" instead of asking for a fix, the agent stops generating and starts asking. It never explains before it asks what you think, uses a 5-level hint ladder that never skips a step, and tracks your progress across sessions so it can pick up where you left off.

One ruleset, two modes, switchable with `/sifu [lite|full|ultra|teach|off]`.

## Why one skill instead of two

The instinct behind both halves is the same: don't do work the other side should be doing.

- **Build mode** — don't write code the codebase, stdlib, or platform already provides.
- **Teach mode** — don't hand you an answer your own reasoning could have reached.

A lazy senior dev and a good teacher are the same personality.

## Commands

| Command | What it does |
|---|---|
| `/sifu [lite\|full\|ultra\|teach\|off]` | Set the mode, or report the current one with no argument |
| `/sifu teach-me` | Enter teach mode with a Japanese zen-style greeting |
| `/sifu-review` | Review the current diff for over-engineering, hand back a delete-list |
| `/sifu-teach [topic]` | Start a Socratic session on the diff, an open file, or a named topic |
| `/sifu-recap` | State of your teaching-mode progress across sessions |
| `/sifu-help` | Quick reference for these commands |

The first bare `/sifu` of a session shows Sifu's welcome message instead of the mode.

## Modes

| Mode | Behavior |
|---|---|
| `lite` | The ladder applies to new code only |
| `full` | **(default)** The ladder applies everywhere; the agent reads before writing |
| `ultra` | Also flags existing over-engineered code nearby for a delete-list |
| `teach` | Socratic mode until you switch back or end the session |
| `off` | Plain build behavior, no ladder, no teaching |

## Install

### Claude Code

```bash
/plugin marketplace add dipanjan-adhikari-912/sifu-code
/plugin install sifu@sifu
```

> [!NOTE]
> Two separate prompts — the marketplace add has to land before the install.

Or without the plugin system, copy the skill and commands directly:

```bash
cp -r skills/sifu ~/.claude/skills/sifu
mkdir -p ~/.claude/commands && cp commands/*.md ~/.claude/commands/
```

### OpenCode

Add to your project's `opencode.json`:

```json
{ "plugin": ["./skills/sifu"] }
```

Or install globally:

```bash
mkdir -p ~/.config/opencode/command
cp commands/*.md ~/.config/opencode/command/
```

Skills are auto-loaded from `~/.agents/skills/` — link them there if you want them project-independent:

```bash
ln -s "$(pwd)/skills/sifu" ~/.agents/skills/sifu
```

### Codex

```bash
mkdir -p ~/.codex/prompts
cp commands/*.md ~/.codex/prompts/
ln -s "$(pwd)/skills/sifu" ~/.codex/skills/sifu
```

No plugin support in your Codex version? Codex reads `AGENTS.md` from the project root automatically — `cp AGENTS.md <your-project>/AGENTS.md` (or `~/.codex/AGENTS.md` for a global default) gets you the ruleset with no slash commands.

### Gemini CLI

```bash
gemini extensions install https://github.com/dipanjan-adhikari-912/sifu-code
```

Loads the ruleset as always-on context (`AGENTS.md`) and registers the `/sifu` commands.

### Everything else

Cursor, Windsurf, Cline, Copilot CLI, Amp, Jules, JetBrains Junie, …

Most of these hosts auto-load an `AGENTS.md` (or their own equivalent) from the project root or a global config path:

| Host | Path |
|---|---|
| Cursor | `.cursor/rules/` |
| Windsurf | `.windsurf/rules/` |
| Cline | `.clinerules/` |
| Generic | `AGENTS.md` |

Copy `AGENTS.md` into whichever path your host reads. This gets you the full ruleset as always-on instructions; it won't have slash commands, so switch modes by just telling the agent what you want ("go easy on the ladder," "teach me this instead").

> [!TIP]
> Restart your CLI after installing — config and commands are loaded once at startup.

## File structure

```text
sifu-code/
├── .claude-plugin/
│   ├── marketplace.json        Claude Code marketplace manifest
│   └── plugin.json             Claude Code plugin manifest
├── .codex-plugin/
│   └── plugin.json             Codex plugin manifest
├── commands/
│   ├── sifu.md                 /sifu — modes, welcome, teach-me
│   ├── sifu-help.md            /sifu-help — command reference
│   ├── sifu-recap.md           /sifu-recap — teaching progress
│   ├── sifu-review.md          /sifu-review — over-engineering audit
│   └── sifu-teach.md           /sifu-teach — Socratic session
├── skills/sifu/
│   ├── SKILL.md                Core skill: both modes, switching, commands
│   └── references/
│       ├── sifu-audits.md      Full ladder detail, non-negotiables, examples
│       └── socratic-framework.md  Hint ladder, detection cascade, progress
├── AGENTS.md                   Fallback ruleset for instruction-only hosts
├── gemini-extension.json       Gemini CLI extension manifest
├── opencode.json               OpenCode plugin config (copy into your project)
├── LICENSE
└── README.md
```

## Credit

Inspired by two projects with opposite specialties:

- [claude-learn-skill](https://github.com/deniz-miro/claude-learn-skill) — Socratic teaching
- [ponytail](https://github.com/dietrichgebert/ponytail) — minimal-code enforcement and multi-CLI portability

Sifu combines the two ideas into one skill; it isn't a fork of either and doesn't reuse their code.

## License

MIT.
