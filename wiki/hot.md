# AI Launchpad MVP — Active Context

_Rewritten each session. ~300 words max. Start here._

## What this project is
A **brainstorming/discovery companion** for Sri Lanka trips — dynamic multi-agent system (supervisor spawns Discovery/RAG, Weather, Safety, Web-Search on KB miss). Adaptive persona · generative UI as **streamed JSON component chunks + view registry** (OpenUI/F5 contract, confirmed by mentor's guide) · provenance badges. Working title "Weli AI" (#13 open). MCP later (#12).

## 🎯 Timeline (re-cut 2026-08-17): prototype Aug 18 → design Aug 19 → **PoC review Aug 21** → presentation **Aug 29**

## ⚠️ FIRST THING NEXT SESSION: ground-truth re-sync
Status unknown for Aug 18–20. Confirm: (1) #18 clickable prototype shipped? (2) any build progress? (3) is the Aug 21 PoC review happening/happened? (4) blockers cleared (Anthropic key `~/.anthropic-key`, Docker)? → then re-plan.

## Latest (2026-08-21)
- **Mentor's streaming guide filed** → `wiki/mentor-streaming-guide.md`. **Transport RESOLVED: SSE-first**; WebSocket only for interrupts/HITL. JSON-chunk generative UI + view registry = our F5 contract, independently confirmed. `astream_events` → AgentTrace.
- **Build order (his + ours agree):** SSE plain-text pipe → JSON chunks + registry → agents → chrome → hardening.
- Board synced to re-cut timeline earlier: milestone *PoC — Aug 21*, #18 prototype (In Progress), #19 mobile-first wireframes v4, #20 canvas thin-slice.

## Key open items
#18 prototype (status?) · #19 v4 wireframes · #20 canvas scope · #14 guide-review decision · #15 landing page · #13 name · pitch narrative (Visit SL vs standalone) · deck + demo video reserved Aug 26–28.

## Gotchas
`/save-mvp` · `/log-meeting` · mobile Obsidian pull-only · safety copy: never "all-clear" · SSE framing + incremental JSON parsing (mentor's pitfalls) · Fireflies "April"=August.
