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

This agent cannot reliably interact with a live browser window. Do not attempt browser-based verification, and do not claim to have opened, screenshotted, or tested the page in a browser — that claim has proven unreliable and should never be made. If a scratch server or test file is created for any other reason, stop and clean it up before finishing:

```bash
# fallback only if genuinely needed for a non-browser test (e.g. a Node-based script):
python3 -m http.server 8000

# stop that fallback server (do not use "kill %1" — it relies on
# shell job-table state that may not persist across tool calls):
lsof -ti:8000 | xargs kill
```

There is no single-test-file command for this project since there is no test framework — see Validation below for how to check correctness instead.

## Working Rules

- Work in small, reviewable changes — one feature per task. Do not combine two features into one change.
- **Isolate cross-cutting state from self-contained logic.** A rule that must be enforced at every point of interaction (like "stop the game after a mine is revealed") is a different kind of work than an algorithm that lives in one function (like flood-fill). If a step would bundle both, split it: implement the self-contained logic first with the state rule explicitly deferred, then add the state rule as its own small, isolated change afterward, applied everywhere interaction happens.
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

This agent cannot reliably verify visual or interactive behavior on its own — do not claim to have opened a browser, taken a screenshot, or visually confirmed something. That claim has proven unreliable twice already on this project and should never be made again.

**Manual testing by the user is the default for interaction and state.** For anything about how the game behaves when played — does a click reveal correctly, does a state guard (game-over, win, flag) actually stop further interaction, does restart reset everything — do not write a test script. Ask the user to test a specific scenario directly in the browser, stating exactly what to do and what to expect. This is not a fallback; it is usually the *better* check, because it tests the real running page rather than a script that might reimplement the same logic separately and drift from what the file actually does. For any change to a state guard specifically, ask the user to test it from more than one entry point, not just the one that sets it — this is where the two real bugs on this project happened.

**Write a test script only when verification requires checking something exhaustively or numerically that isn't practical to eyeball** — for example, confirming all 81 cells have the correct adjacency count, or confirming an exact count (like total mines) across the whole board. If a script is written, it must exercise the actual code in `index.html`, not a separate reimplementation of the same logic — state clearly which one it does. Delete any scratch test file before finishing the task (see Common Commands).

If a check cannot be run, explain why.

## Sensitive Areas

There isn't much here that's sensitive in the usual sense (no auth, no payments, no real data), but keep these boundaries:

- Do not add any network calls, analytics, or external script tags — this stays fully offline and self-contained.
- Do not add a server, build step, or dependency without asking first, even a small one.
- Do not delete or restructure the file layout without asking.

## Keeping This File Current

If you learn something about this project that isn't captured above, propose an update to this file before finishing the task rather than letting the same context get re-explained next session.
