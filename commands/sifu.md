---
name: sifu
description: Set sifu's mode (lite/full/ultra/teach/off) or report the current one.
---

Parse the argument, if any:
- `lite` / `full` / `ultra` — set build-mode intensity as described in `skills/sifu/SKILL.md`. Confirm the new mode in one short line.
- `teach` — switch to teach mode for the rest of the session (see `skills/sifu/references/socratic-framework.md`).
- `off` — disable the ladder and teaching entirely; write code normally.
- no argument — report the current mode and nothing else.

Persist the chosen mode for the rest of the session (and across sessions if the host provides a place to store it, e.g. a small config file next to the host's own settings).
