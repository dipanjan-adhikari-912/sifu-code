# The Laziness Ladder

Read this before writing non-trivial code in build mode. Walk the ladder top
to bottom. Stop and use the first rung that solves the problem.

## The ladder

**1. Does this need to exist?**
Ask whether the feature, abstraction, or file is actually required by the
task, or whether it's being added because it seems like the "complete"
thing to do. Speculative flexibility (config options nobody asked for,
abstract base classes for one subclass) doesn't get built. If in doubt, ask
the user rather than build it.

**2. Already in this codebase?**
Search before writing. A near-identical helper, component, or pattern two
files away means reuse or lightly extend it, not a parallel implementation.

**3. Does the standard library do it?**
Language/runtime stdlib before a package. `pathlib` before a path-manipulation
helper, `itertools` before hand-rolled loops, `crypto` before a hand-rolled
hash.

**4. Does the native platform do it?**
Before reaching for a component or library, check what the platform already
gives for free: `<input type="date">` before a date-picker library,
`<dialog>` before a modal library, CSS `:has()` before a JS solution,
`Intl` before a formatting library.

**5. Is it already an installed dependency?**
If the project already depends on something that solves this, use it before
adding a new package. Don't add a new dependency for something an existing
one already covers.

**6. Does it fit in one line?**
If the fix is genuinely one line, ship one line. Don't wrap it in a function,
a class, or a config object "for clarity" unless it's reused elsewhere.

**7. Only then: the minimum that works**
Write the smallest implementation that satisfies the actual requirement —
not the generalized version of the requirement.

## Never traded away for brevity

These are not optional at any rung:
- Validation at trust boundaries (user input, API responses, file contents)
- Error handling for anything that can fail (network, disk, parsing)
- Security: auth checks, secret handling, injection prevention
- Accessibility: semantic HTML, keyboard access, labels

A one-line native `<input type="date">` still needs a label. A stdlib call
that can throw still needs a caught error path. Lazy means skipping
unnecessary code, not skipping necessary code.

## Read first, climb second

The ladder runs after understanding the problem, not instead of it. Before
picking a rung: read the code the change actually touches, trace the real
data flow, and understand what currently happens. A one-liner that ignores
how the surrounding code actually behaves is a bug, not a win.

## Worked examples

**Date picker request** → rung 4. `<input type="date">` with a `<label>`.
Not a library, not a wrapper component.

**"Add a cache for this API call"** → rung 1 first (does it need caching —
is it actually called often enough to matter?), then rung 5 (is there
already a cache utility or library in the project?), then rung 7 if nothing
fits — a small in-memory map with a TTL, not a generic cache abstraction.

**"Format this date"** → rung 3. `Intl.DateTimeFormat` / `strftime`, not a
date library, unless the project already has one installed (rung 5 would
then apply instead).

**"Validate this form field"** → the validation itself is never skipped
(non-negotiable list above) — the ladder decides *how much code* the
validation takes, not *whether* it exists. Often rung 4 (native `required`,
`pattern`, `type="email"`) plus a minimal custom check for anything the
native attributes don't cover.

## When ultra mode is on

In `/sifu ultra`, also flag existing code in the area being touched that
overshoots what it needs to do, and offer a delete-list — but never delete
without the user confirming, and never touch code outside the current
task's blast radius uninvited.
