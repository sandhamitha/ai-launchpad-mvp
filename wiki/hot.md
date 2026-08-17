# AI Launchpad MVP — Active Context

_Rewritten each session. ~300 words max. Start here._

## What this project is
A **brainstorming/discovery companion** for Sri Lanka trips — dynamic multi-agent system (supervisor spawns Discovery/RAG, Weather, Safety, Web-Search on KB miss). Adaptive persona · generative UI (OpenUI) · provenance badges (`guide-reviewed`/`web-sourced`/`live`). Working title "Weli AI" (#13 not final). MCP later (#12).

## 🎯 NEW TIMELINE (mentor sync 2026-08-17)
- **Aug 18 evening — clickable index-HTML prototype (flow skeleton) ← URGENT, TODAY**
- **Aug 19** — design + prototype finalized
- **Fri Aug 21** — prototype PoC for mentor review
- **Aug 29** — presentation submission
- **Daily 15–20 min mentor calls** · parallel workstreams (UI ∥ streaming backend)

## Mentor sync outcomes (see `meetings/2026-08-17-…` + 4 decisions)
- Vision validated: hidden gems, provenance UI, persona learning, guide validation, supervisor+sub-agents, streaming.
- **Mobile-first UI** (supersedes desktop-first) — wireframes need v4 pass; components carry over.
- **Landing ↔ canvas app split** — anonymous landing captures first query; canvas = interactive trip space (maps deferred).
- **FastAPI + WebSockets streaming, prototyped FIRST** (chat↔backend round-trip before agents). WebSockets-vs-SSE to settle.
- Swift/native = exploration only, post-PoC. Daily Reddit/FB monitoring feeds KB.
- Mentor owes: index-HTML template + streaming tech guide (tonight).

## ⛔ Blockers (user)
**Anthropic key** (`~/.anthropic-key`) · **Docker running**

## Board sync needed
Move PoC milestone Aug 22→21 · add prototype ticket (Aug 18–19) · mobile-first wireframe v4 ticket · canvas scope.

## Gotchas
`/save-mvp` · `/log-meeting` · mobile Obsidian pull-only · safety copy: never "all-clear" · Fireflies "April" dates = August (transcription).
