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

# context-mode — MANDATORY routing rules

You have context-mode MCP tools available. These rules are NOT optional — they protect your context window from flooding. A single unrouted command can dump 56 KB into context and waste the entire session.

## BLOCKED commands — do NOT attempt these

### curl / wget — BLOCKED
Any Bash command containing `curl` or `wget` is intercepted and replaced with an error message. Do NOT retry.
Instead use:
- `ctx_fetch_and_index(url, source)` to fetch and index web pages
- `ctx_execute(language: "javascript", code: "const r = await fetch(...)")` to run HTTP calls in sandbox

### Inline HTTP — BLOCKED
Any Bash command containing `fetch('http`, `requests.get(`, `requests.post(`, `http.get(`, or `http.request(` is intercepted and replaced with an error message. Do NOT retry with Bash.
Instead use:
- `ctx_execute(language, code)` to run HTTP calls in sandbox — only stdout enters context

### WebFetch — BLOCKED
WebFetch calls are denied entirely. The URL is extracted and you are told to use `ctx_fetch_and_index` instead.
Instead use:
- `ctx_fetch_and_index(url, source)` then `ctx_search(queries)` to query the indexed content

## REDIRECTED tools — use sandbox equivalents

### Bash (>20 lines output)
Bash is ONLY for: `git`, `mkdir`, `rm`, `mv`, `cd`, `ls`, `npm install`, `pip install`, and other short-output commands.
For everything else, use:
- `ctx_batch_execute(commands, queries)` — run multiple commands + search in ONE call
- `ctx_execute(language: "shell", code: "...")` — run in sandbox, only stdout enters context

### Read (for analysis)
If you are reading a file to **Edit** it → Read is correct (Edit needs content in context).
If you are reading to **analyze, explore, or summarize** → use `ctx_execute_file(path, language, code)` instead. Only your printed summary enters context. The raw file content stays in the sandbox.

### Grep (large results)
Grep results can flood context. Use `ctx_execute(language: "shell", code: "grep ...")` to run searches in sandbox. Only your printed summary enters context.

## Tool selection hierarchy

1. **GATHER**: `ctx_batch_execute(commands, queries)` — Primary tool. Runs all commands, auto-indexes output, returns search results. ONE call replaces 30+ individual calls.
2. **FOLLOW-UP**: `ctx_search(queries: ["q1", "q2", ...])` — Query indexed content. Pass ALL questions as array in ONE call.
3. **PROCESSING**: `ctx_execute(language, code)` | `ctx_execute_file(path, language, code)` — Sandbox execution. Only stdout enters context.
4. **WEB**: `ctx_fetch_and_index(url, source)` then `ctx_search(queries)` — Fetch, chunk, index, query. Raw HTML never enters context.
5. **INDEX**: `ctx_index(content, source)` — Store content in FTS5 knowledge base for later search.

## Subagent routing

When spawning subagents (Agent/Task tool), the routing block is automatically injected into their prompt. Bash-type subagents are upgraded to general-purpose so they have access to MCP tools. You do NOT need to manually instruct subagents about context-mode.

## Output constraints

- Keep responses under 500 words.
- Write artifacts (code, configs, PRDs) to FILES — never return them as inline text. Return only: file path + 1-line description.
- When indexing content, use descriptive source labels so others can `ctx_search(source: "label")` later.

## ctx commands

| Command | Action |
|---------|--------|
| `ctx stats` | Call the `ctx_stats` MCP tool and display the full output verbatim |
| `ctx doctor` | Call the `ctx_doctor` MCP tool, run the returned shell command, display as checklist |
| `ctx upgrade` | Call the `ctx_upgrade` MCP tool, run the returned shell command, display as checklist |
