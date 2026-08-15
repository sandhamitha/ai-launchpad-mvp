# AI Launchpad MVP — Active Context

_Rewritten each session. ~300 words max. Start here._

## What this project is
A **brainstorming/discovery companion** for planning a Sri Lanka trip — a **dynamic multi-agent system**. Supervisor spawns specialized sub-agents at runtime (Discovery/RAG, Weather, Safety KB+live, Web-Search on KB miss). Light adaptive persona; generative UI; provenance always visible (`guide-reviewed` vs `web-sourced`). Future: registry exposed over **MCP** to Claude/ChatGPT/Gemini ("keep your agent — we supply the accuracy", #12).

## 🎯 PoC **Aug 22** · pitch **Aug 29**

## Status (2026-08-16)
- Planning complete: spec approved (`specs/2026-08-15-poc-scope-design.md`), board fully wired — 17 issues, 2 milestones, roadmap timeline.
- **Audited:** `wiki/risk-audit-2026-08-15.md` (technical — parked for architecture session) + `wiki/idea-phase-findings.md` (story-layer F1–F7 → tickets #13–#17).
- Build NOT started — gated on user's architecture session + blockers below.

## ⛔ Blockers (user)
1. **Anthropic API key** → `~/.anthropic-key` · 2. **Docker Desktop running**

## Active now
- **#3 wireframes** — drafted (`wireframes/poc-hero-flow.html`), awaiting user review → then close
- **#13 name the product** (Aug 16–17) · **#15 landing page + waitlist** (Aug 17–19)
- User decisions pending: name · pitch narrative (Visit SL milestone vs standalone) · guide review vs softened claim (#14)
- Comms email reply drafted — attach headshot + send (reply-all)

## On architecture session, apply audit fixes
Code home = `ai-launchpad-mvp/poc/` (board-commit linking) · split S1 → backend/frontend · KB 60–100 chunks · smoke-eval right after KB seed (Aug 18) · re-cut tail: MCP Aug 23–25, deck + recorded demo Aug 26–28.

## Gotchas
`/save-mvp` to save · `/log-meeting` for Fireflies · mobile Obsidian pull-only · judge story: dynamic spawn on KB miss = the demo moment · mentor sync date unconfirmed.
