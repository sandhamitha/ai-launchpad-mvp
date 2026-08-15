# Decision — PoC scope: dynamic multi-agent architecture

**Date:** 2026-08-15
**Status:** accepted
**Source:** scoping session; full detail in `../specs/2026-08-15-poc-scope-design.md`

## Context
1 week to PoC (~Aug 22), pitch Aug 29, solo build. Mentor's feature-breakdown method applied to lock scope.

## Decision
- **Hero flow:** brainstorm/discovery (not itinerary, not live-companion) — matches the locked core idea.
- **Agents:** supervisor + 3 core (Discovery/RAG, Weather, Safety) + **Web-Search fallback spawned dynamically on KB miss** — an extensible registry, spawning decided at runtime by the supervisor.
- **Safety agent is KB + real-time** (live hazard/advisory), strictest guardrails: timestamp + source, always official contacts, never a fabricated all-clear.
- **Graceful fallback UX:** never hard-reject ("KB doesn't cover it") — spawn web search, handle misspellings ("did you mean…"), politely redirect non-SL places. Provenance always visible: `guide-reviewed` vs `web-sourced`.
- **UI:** streaming chat + 2–3 cached generative components. **Persona:** light (2–3 traveler types → tone/depth).
- **Deferred:** transport/deals agents, live-trip triggers, itinerary persistence, auth, deploy.

## Why
- Judges score agentic-workflow sophistication → dynamic spawning IS the demo; KB-miss → live agent spawn shows it happening.
- Provenance split protects the guide-reviewed-KB trust moat while killing dead-end UX.
- Every scope cut keeps a complete demo at each build stage (walking skeleton; mentor's "route B").

## Consequences
- 4 agents + supervisor in a week — no slack day; fallback = ship fewer agents, still complete.
- Timeline on the GitHub board: #3 wireframes Aug 15–16 → … → #11 eval Aug 22.
