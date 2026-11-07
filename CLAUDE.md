# TAB Scroller

A self-scrolling PDF viewer for playing fingerstyle guitar from tabs. Press play and the PDF scrolls at a set speed. Tap space to jump to the next page, hold space for 2x speed. PDFs you open are saved to a quick-select library.

**Setup:** Ian plays from a laptop, with an external monitor as the main screen. Input is the laptop keyboard (no touch, no pedal yet).

## Stack

- **Vite + TypeScript**, plain DOM, no UI framework
- **pdfjs-dist** renders pages to canvas
- **idb-keyval** stores the PDF library (as Blobs in IndexedDB)
- **Vitest** for unit tests (scroll math, key tap/hold logic, library store)
- Dev server: `npm run dev` on http://localhost:5173

## Checks (what "done" means)

Run all four. A check that could not run counts as NOT RUN, never as a pass.

```
npx tsc --noEmit        # types
npx vitest run          # tests
npx vite build --outDir "$(mktemp -d)"   # build without touching dist/
npm audit --omit=dev    # only when package-lock.json changed
```

Browser behaviour (scrolling feel, spacebar, rendering on the external monitor) is verified by the orchestrator in Chrome, never by the refuter. Until it has been checked there, list it under "Not verified".

## Where things live

| What | Where |
|---|---|
| Long-term memory: current state + build log | `docs/TAB_Scroller_Documentary.md` |
| Specs (what to build) | `docs/superpowers/specs/` |
| Plans (how to build it) | `docs/superpowers/plans/` |
| Handoffs (where a session stopped) | `docs/superpowers/handoffs/` |
| Project agents | `.claude/agents/` (committed) |
| Code graph | `graphify-out/` (`GRAPH_REPORT.md` is committed, cache and graph.json are not) |

## The documentary

- Updating it is part of the task, in the same session. The orchestrator writes it. Never delegate it.
- **Part I: Current State** is rewritten in place. **Part II: Build Log** is append-only, one dated entry per change. Each entry records why it was built, what was rejected, and what a passing check did not prove.
- Fix stale claims where they sit, not with a new contradicting entry.
- Read it by section (`grep -n "^## "`) once it grows past ~500 lines.
- Specs and plans go in their own folders. The documentary links to them and never inlines them.

## Graphify

Once `src/` exists: run `graphify update .` after code changes (AST-only, free). For code questions, try `graphify query "<q>"` / `graphify explain "<concept>"` before grep. Its LLM steps need the workaround from the BIOMS memory file `reference_graphify_llm_backend`.

## Agents

| Role | Agent | Model |
|---|---|---|
| Scout | `honey:hive-scout` | haiku |
| Researcher (facts tagged VERIFIED/UNVERIFIED) | `researcher` | sonnet |
| Builder (2 files or fewer) | `honey:hive-builder` | sonnet |
| Refuter (reruns the checks, ACCEPT/REJECT) | `refuter` | opus |
| Debugger (escalation only) | `debugger` | opus |

- Trivial tasks are done inline. Agents are for work that would otherwise flood the main context.
- Every builder run is followed by a refuter run.
- Only one agent edits at a time.
- Every brief has seven parts: goal, files, what may change, what to verify, what not to do, output shape, what is already known.
- `gsd-*` agents are not used in this project.
- External CLIs (`gemini`, `opencode`) are consultants only. Run them from outside the repo with stdin redirected (`</dev/null`). They never write to the repo, and their output is a hypothesis, not a verdict.

## Workflow rules

- superpowers skills (brainstorming, writing-plans, TDD, systematic-debugging, verification-before-completion) are mandatory when they apply.
- For multi-task work, write a handoff early and rewrite it after every task. Every handoff has a **STOP** block (exactly where to resume) and a required **Not verified** field.
- **Commits:** Claude never runs `git commit`. It writes the message for Ian, with no apostrophes and no double quotes.
- honey terse mode must never compress: the documentary, cross-session messages, agent briefs, WHY-comments, or failure reports.
