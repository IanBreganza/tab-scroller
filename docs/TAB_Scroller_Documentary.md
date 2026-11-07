# TAB Scroller Documentary

## How to maintain this file

- **Part I: Current State** describes what is true now. Rewrite it in place whenever something changes. Never append contradicting notes.
- **Part II: Build Log** is append-only. Add one dated entry per change: what was built, why, what was rejected, and what the checks did **not** prove.
- If a Part I claim turns out wrong, fix it where it sits and mention the correction in that day's build-log entry.
- Specs, plans and handoffs live in `docs/superpowers/`. Link to them, never paste them in here.
- No secrets in this file.

---

# PART I: CURRENT STATE

## Project overview

A browser-based PDF viewer for playing fingerstyle guitar from tabs, with no hands needed once it's playing.

- **Play/pause** starts automatic scrolling at a speed the user sets.
- **Spacebar tap** while playing jumps to the next page.
- **Spacebar hold** while playing doubles the speed until it is released.
- **Quick-select library** keeps every opened PDF for one-click reopening.

**Usage environment:** a laptop driving an external monitor that is the main screen. The keyboard is the laptop's. Spacebar input only reaches the app while its window has focus, so a focus warning is planned.

## Stack

Vite + TypeScript (plain DOM), pdfjs-dist, idb-keyval, Vitest. Runs in Chrome. See `CLAUDE.md` for the check commands.

## Progress tracker

| Feature | Status | Spec / plan |
|---|---|---|
| Project scaffold (Vite + TS) | Not started | |
| PDF rendering (all pages, fit to width) | Not started | |
| Auto-scroll with speed control | Not started | |
| Spacebar tap = next page / hold = 2x | Not started | |
| Quick-select library (IndexedDB) | Not started | |
| Focus-lost warning | Idea | |

## Deferred / next up

- 📌 Next: run brainstorming on the full feature set, then write a spec in `docs/superpowers/specs/`.

## Known tech debt

None yet.

## Key files

None yet. The workflow files are `CLAUDE.md`, this documentary, and `.claude/agents/`.

---

# PART II: BUILD LOG

## 2026-11-07: Project setup

**Built:** `git init` (branch `main`), `CLAUDE.md`, this documentary, the `docs/superpowers/{specs,plans,handoffs}/` folders, `.gitignore`, and three project agents in `.claude/agents/` (researcher, refuter, debugger).

**Why:** this is a lightweight version of the BIOMS workflow. Kept: the documentary, handoffs, the builder-then-refuter loop, and graphify. The agents were rewritten for this stack. The BIOMS refuter ran Laravel/Pint/composer checks, which mean nothing here. This one runs `tsc`, `vitest`, `vite build` and `npm audit`.

**Rejected:**
- *Full BIOMS mirror.* The Opus/Sonnet request-file banner and per-feature worktrees cost more than they save on an app expected to be 500 to 1000 lines.
- *Electron/Tauri/native.* A browser app covers keyboard input, local file storage and offline use. Nothing here needs native APIs.
- *React.* The app is one screen with a few controls, so a framework adds build weight and nothing else.

**Agents committed, not gitignored** (unlike BIOMS). This is a personal repo, so committing them gives a backup and history.

**Not verified:** no code exists yet. Graphify has not been run, because there is nothing to graph.
