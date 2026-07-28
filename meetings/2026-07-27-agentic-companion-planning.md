# Agentic Travel Companion — Planning Session

**Date:** 2026-07-27, 19:31
**Participants:** Speaker 1, Speaker 2  *(confirm names — likely you + co-founder/lead; the mentor review with Shazni is scheduled separately for Saturday)*
**Source:** Fireflies — transcript + summary in `../transcripts/2026-07-27-agentic-companion-*.md`
**Purpose:** Define the AI Launchpad MVP concept, scope, UX direction, tech approach, and timeline.

---

## TL;DR
The MVP is now defined: an **agentic travel companion for Sri Lanka** — autonomous agents (weather, traffic, emergencies, personalized advice) over SL-specific local data (RAG), with an interactive/generative UI. Differentiator vs Google/Gemini = **local knowledge + real-time autonomous assistance**. Pitch deadline **Aug 29** (50 participants). Judges weight **agentic-workflow sophistication + technical depth**, not just idea novelty.

## Decisions made (see `../decisions/` for detail)
1. **Product = agentic SL travel companion** — multi-agent, dynamic planning, real-time adaptation. → `2026-07-27-product-agentic-sl-companion.md`
2. **Differentiation = SL-local data + autonomous agents** vs generalist competitors. → `2026-07-27-differentiation-local-plus-agents.md`
3. **UX = interactive/generative UI blended with chat** (cached generative components to cut token cost). → `2026-07-27-ux-generative-interactive-ui.md`
4. **Tech = incremental build, RAG knowledge, simple stack first, eval/guardrails as a first-class concern.** → `2026-07-27-tech-incremental-rag-simple-stack.md`
5. **Scope discipline** — prioritize core agentic workflows over peripheral features; fallback plan if agentic work slips.

## Key discussion points
- Competitors regressed from asking follow-up planning questions → opening for better interaction/accuracy.
- Real-time contextual alerts (traffic, flooding) triggered by the user's set trip plan (extends Google-Maps-style capability).
- Observability + automated eval to benchmark responses against Gemini and prove technical superiority.
- Vision framing: AI as an "industrial revolution"; build for real user pain, not tech for its own sake.

## Timeline / milestones
- **Aug 29** — AI Launchpad pitch (hard deadline)
- **This Thu/Fri** — refined feature list + product flows
- **Saturday 9:00 AM** — mentor review session (wireframes + scope readiness)
- Daily async check-ins through the week; Sun/Mon reserved for refinement

## Action items
**Speaker 1**
- [ ] Share detailed doc with prompts + initial code formats (feature/product considerations) — by next morning
- [ ] Create GitHub project connected to the repo; set milestones + tasks
- [ ] Daily morning check-ins with Speaker 2
- [ ] Architectural planning + technical references for agentic workflows
- [ ] Supply session notes/transcript/decisions for the record *(← now done, in this vault)*
- [ ] Curate design-system inspirations/references
- [ ] Schedule + hold mentor review — Saturday 9:00 AM

**Speaker 2**
- [ ] Feature-level mind map + product flow (entry points, core functions)
- [ ] Basic wireframes/sketches (web/mobile)
- [ ] Make the code repo public + connect to the GitHub project
- [ ] Daily comms for idea refinement/validation
- [ ] Prep consolidated feature list + tech-approach overview for the mentor session

## ⚠️ Open flag — overlap with Visit SL / Weli
This concept is nearly identical to the **Visit Sri Lanka / Weli** project (AI SL travel companion, RAG on SL-specific data, agentic flows). Decide whether the Launchpad MVP **reuses** that spec/plan/`weli-backend` or is a fresh build — potentially a big time-saver. *(Raise with the team / mentor.)*
