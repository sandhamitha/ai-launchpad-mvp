# ai-launchpad-mvp — vault schema (for Claude Code)

This is a **standalone, shared research memory vault** for the AI Launchpad MVP project. It is intentionally SEPARATE from `~/Development/_memory/` (the personal `second-brain` repo) — it has its own private GitHub repo and is shared with my mentor. **Never** pull content, links, or references from other projects into this vault. Keep everything here self-contained and professional (a mentor reads it).

## On session start (when working in this folder)
1. Read `wiki/hot.md` for active context.
2. Read `wiki/index.md` to see what pages exist.
3. Silently load context — don't announce unless asked.

## On session end / checkpoint
1. Append a 3-5 bullet summary to `wiki/log.md` (newest at top, with date).
2. Rewrite `wiki/hot.md` with cumulative ~300-word active context.
3. Summarize changes and **wait for my approval before committing** (see Git rules).

## Structure
- `wiki/hot.md` — active context, rewritten each session (~300 words)
- `wiki/log.md` — append-only session history, newest first
- `wiki/index.md` — index of wiki/topic pages
- `meetings/` — one note per mentor sync (use the template)
- `decisions/` — one file per key decision + rationale
- `experiments/` — research runs (hypothesis → setup → result → takeaway)
- `weekly-logs/` — weekly progress summaries for the mentor

## Git rules
- Commit messages: clean, concise, no AI/Claude/Co-Authored-By attribution.
- ALWAYS summarize what changed and wait for my explicit approval before committing.
- Convert relative dates to absolute in notes.
- `.obsidian/workspace*.json` is gitignored (per-device UI state) — never force-add it.

## Sharing constraints (important)
- This repo is shared with a mentor. Do NOT write anything personal or unrelated to this project here.
- A deletion-guard workflow (`.github/workflows/guard.yml`) fails any push deleting >5 files. Don't disable it.
