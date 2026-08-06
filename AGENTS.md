# AGENTS.md

## Project Overview

A browser-based Minesweeper implementation, built as a first test project for a local-model + Cline coding setup. The goal is a working, playable game — not a polished product. Keep every change small enough to review in full.

## Tech Stack

- Framework: none
- Language: HTML, CSS, JavaScript (vanilla, no TypeScript)
- Package manager: none — no npm, no dependencies, no build step
- Styling: plain CSS, no framework
- Database: none
- Test framework: none (manual browser verification only, see Validation below)

## File Structure

This is intentionally a **single HTML file** (`index.html`) with inline `<style>` and `<script>` tags. Do not split this into separate `.css`/`.js` files, and do not introduce a bundler, framework, or `node_modules` — the whole point of this project is to stay small enough to review in full and open directly in a browser with no setup step.

## Common Commands

There is no install step and no build step.

```bash
# browser verification — do not start a server for this
# use Cline's browser tool with a file:// URL directly, e.g.:
# file:///Users/bemoy/Developer/Minesweeper/index.html

# fallback only if the browser tool cannot load a file:// URL:
python3 -m http.server 8000

# stop that fallback server (do not use "kill %1" — it relies on
# shell job-table state that may not persist across tool calls):
lsof -ti:8000 | xargs kill
```

**Browser verification default:** navigate directly to the file with an absolute `file://` path (see above). Do not start a local server for this project unless the `file://` approach genuinely fails — this is a static file with no server-side logic, so a server adds a process to manage for no benefit. If a server was started for any reason, stop it with the `lsof` command above before finishing the task, not `kill %1`.

There is no single-test-file command for this project since there is no test framework — see Validation below for how to check correctness instead.

## Working Rules

- Work in small, reviewable changes — one feature per task (grid rendering, then mine placement, then reveal logic, then flags/win/loss, then polish). Do not combine two of these into one change.
- Explain the plan before editing.
- Do not rewrite unrelated parts of the file.
- Do not add a framework, build tool, or external library "to make this easier." If something feels like it needs one, say so and stop rather than adding it.
- Ask before installing anything at all — nothing in this project should require an install.
- Ask before adding new files. This project should stay as one HTML file unless we explicitly decide otherwise.
- If a command or convention isn't covered here, ask rather than assume.

## Code Style

- Prefer clear, readable vanilla JS over clever one-liners.
- Use plain functions, not classes, unless there's a specific reason a class helps.
- Name grid/cell variables consistently (e.g. `row`, `col`, `cellIndex`) rather than mixing naming schemes.
- Comment the flood-fill reveal logic specifically — it's the one non-obvious algorithm in this project, and it should be possible to follow without re-deriving it.
- Keep CSS selectors simple and scoped to this file; no CSS-in-JS, no utility-class framework.

## Validation

There is no automated test suite for this project. Before considering a change complete:

- Use the browser tool to load `index.html` and take a screenshot to confirm it renders as expected.
- For logic changes (mine placement, adjacency counts, flood-fill, win/loss), use the browser tool to click through a specific scenario and confirm the visible result is correct — describe which scenario you tested.
- Specifically verify the flood-fill does not reveal mines and does not cross into cells that already show a number — this is the most likely place for a subtle bug.
- If a check cannot be run (e.g. the browser tool isn't available), say so explicitly rather than assuming it works.

## Sensitive Areas

There isn't much here that's sensitive in the usual sense (no auth, no payments, no real data), but keep these boundaries:

- Do not add any network calls, analytics, or external script tags — this stays fully offline and self-contained.
- Do not add a server, build step, or dependency without asking first, even a small one.
- Do not delete or restructure the file layout without asking.

## Keeping This File Current

If you learn something about this project that isn't captured above, propose an update to this file before finishing the task rather than letting the same context get re-explained next session.
