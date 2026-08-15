# AI Launchpad MVP — Active Context

_Rewritten each session. ~300 words max. Start here._

## What this project is
A **brainstorming/discovery companion** for planning a Sri Lanka trip — a **dynamic multi-agent system**. Supervisor spawns sub-agents at runtime (Discovery/RAG, Weather, Safety KB+live, Web-Search on KB miss). Light adaptive persona; generative UI via **OpenUI** (components from our library, props-only streaming); provenance always visible (`guide-reviewed` / `web-sourced` / `live+timestamp`). Future: registry over **MCP** (#12). Working title "Weli AI" — **name NOT final** (#13 deferred).

## 🎯 PoC **Aug 22** · pitch **Aug 29**

## Status (2026-08-16)
- Mentor's 3 board items ALL DONE: idea ✅ features ✅ **wireframes ✅ (#3 closed)**.
- Wireframes locked: desktop, inline collapsible **AgentTrace** (auto-open while running), hero empty state + resume cards, constraint chips + pin/👍/👎. Polished **Claude Design** set imported: `wireframes/weli-ai-wireframes.dc.html` (+ support.js) — mentor: pull & open in browser.
- Wireframe details folded into spec + tickets #5/#7 (spelling choice, side-by-side provenance, skeleton cards, re-check, safety copy rule).
- Build NOT started — awaiting architecture session.

## ⛔ Blockers (user)
1. **Anthropic API key** → `~/.anthropic-key` · 2. **Docker Desktop running**

## Active now
- **#15 landing page + waitlist** (Aug 17–19) · #14 guide-review decision (Aug 18–21)
- **Architecture session (user)** → then apply audit fixes: code home `poc/` in this repo · split S1 backend/frontend · KB 60–100 chunks · smoke-eval after KB seed · re-cut tail (MCP Aug 23–25, deck + demo video Aug 26–28)
- Pitch narrative decision (Visit SL milestone vs standalone) still open.

## Gotchas
`/save-mvp` to save · `/log-meeting` for Fireflies · mobile Obsidian pull-only · judge moment: blue spawn-row on KB miss · safety copy: never "all-clear" · mentor sync date unconfirmed.
