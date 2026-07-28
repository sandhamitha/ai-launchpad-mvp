# Decision — Interactive / generative UI blended with chat

**Date:** 2026-07-27
**Status:** accepted
**Source:** Planning session `../meetings/2026-07-27-agentic-companion-planning.md`

## Context
Pure text chat is a crowded, commoditized experience. We need a UX that feels responsive, informative, and "sticky" — and that controls LLM cost.

## Decision
Blend chat with **interactive, generative UI components** (images, graphs, cards tailored to the query). Use **cached generative elements** to reduce token cost while raising perceived responsiveness. Add real-time contextual alerts (traffic, flooding) triggered by the user's set trip plan.

## Why
- Interactive visuals differentiate in a crowded AI-travel-assistant market and deepen engagement beyond text.
- Speaker 2 already has a **framework for dynamic UI generation** (images/graphs per query) — a demonstrable asset for the pitch.
- Caching generative components lowers operational (token) cost — matters under the cost constraints.

## Consequences / trade-offs
- UI generation framework becomes a core dependency to harden before the demo.
- Wireframes/user-journey sketches needed early (Saturday mentor session) to lock the UX before deep build.
