# AI Launchpad MVP — Active Context

_Rewritten each session. ~300 words max. Start here._

## What this project is
A **brainstorming/discovery companion** for planning a Sri Lanka trip — a **dynamic multi-agent system**, not a SaaS with a chatbot. A supervisor agent spawns specialized tool-using sub-agents **at runtime** (Discovery/RAG, Weather, Safety KB+live, Web-Search fallback on KB miss) to surface **accurate, on-time** SL knowledge generalist AI can't. Light adaptive persona; generative UI; provenance always visible (`guide-reviewed` vs `web-sourced`).

## 🎯 Deadlines: PoC **Aug 22** · pitch **Aug 29** (50 participants)

## Status (2026-08-15) — planning DONE, build starts
- ✅ Mentor Step 1 (idea) + Step 2 (feature breakdown) complete, in his exact format; issues #1 #2 closed with evidence.
- ✅ Spec committed: `specs/2026-08-15-poc-scope-design.md` (architecture, 6 INVEST stories w/ G/W/T criteria, MoSCoW, walking skeleton).
- ✅ GitHub board fully wired: milestone *PoC — Aug 22*, issues #4–#11, roadmap timeline.
- ⏳ **Now:** #3 wireframes (Aug 15–16) → then S1 skeleton build.

## Build order (board timeline)
#3 wireframes → #4 S1 chat skeleton → #10 KB seed → #5 S2 discovery+fallback → #6 weather → #7 safety → #8 orchestration → #9 persona → #11 eval → PoC.

## Key facts / gotchas
- Reuse **weli-backend** (FastAPI + pgvector migration + LangChain scaffold + venv ready; local Docker pgvector + fastembed planned — no OpenAI needed).
- Blocked-on-user for build start: **Anthropic API key** (`~/.anthropic-key`) + Docker Desktop running.
- Judge story: "why 5 agents not 1" = distinct tools/data/guardrails; dynamic spawn ON KB miss is the live demo moment.
- Save with **`/save-mvp`** · meetings via `/log-meeting` · mobile Obsidian pull-only.

## Open
- Champion user + willingness-to-act signal (parallel track) · confirm next mentor sync · wireframes to attach on #3.
