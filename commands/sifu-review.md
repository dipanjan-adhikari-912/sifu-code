---
name: sifu-review
description: Review the current diff for over-engineering and hand back a delete-list.
---

Look at the current uncommitted diff (or the most recent commit if the working tree is clean). Walk it against `skills/sifu/references/laziness-ladder.md` and flag anything that skipped a rung it shouldn't have — unnecessary abstractions, unused flexibility, a library where a native feature would do, a class where a function would do.

Output a short delete-list: file, line range, what to remove or simplify, and why. Never edit the files yourself in this command — just report. Do not flag anything in the "never traded away" list (validation, error handling, security, accessibility) even if it adds lines.
