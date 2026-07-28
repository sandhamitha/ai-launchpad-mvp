# Decision — Incremental build, RAG knowledge, simple stack, eval-first

**Date:** 2026-07-27
**Status:** accepted
**Source:** Planning session `../meetings/2026-07-27-agentic-companion-planning.md`

## Context
Hard deadline (Aug 29) and a solo/small team. Judges care about agentic-workflow quality and demonstrable technical depth, not peripheral polish.

## Decision
- **Build incrementally:** first define feature scope + flows, then develop agentic workflows with clear guardrails and evaluation metrics.
- **RAG** over SL-specific data as the knowledge backbone, integrated with cloud AI pipelines.
- **Keep the stack simple initially** — minimal auth / no fancy features — prioritize agentic-flow quality.
- **Eval + observability as first-class:** automated evaluation to benchmark responses against Gemini and prove superiority.

## Why
- Time-boxed to Aug 29 → concentrate effort where judges score (agentic depth + guardrails), not on auth/peripherals.
- Eval/observability turns "we're better" into a demonstrable, defensible claim for the pitch.
- Incremental flow-first approach de-risks the agentic build.

## Consequences / trade-offs
- Deliberately deferring auth and non-core features — acceptable for a competition MVP, revisit post-pitch.
- Contingency: if agentic implementation slips, fall back to a reduced-but-complete flow to stay demo-ready.
