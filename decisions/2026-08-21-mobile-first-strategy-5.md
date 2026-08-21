# Decision — Build order + stack picks for the final sprint

**Date:** 2026-08-21 · **Status:** accepted (mentor sync)
**Source:** `../meetings/2026-08-21-mobile-first-strategy.md` (~48:30–50:20, 56:00–58:00, 01:13–01:22)

## Context
Eight days to submission; mobile development is new territory; the judges' weight is on agentic intelligence, with UI/UX as the multiplier.

## Decision
- **Immediate order:** crayon design system into the Swift app (extract reference designs to textual specs via Claude, then implement) → **basic login via Supabase** (built-in auth; non-functional acceptable) → chat ↔ LangChain **hello-world round trip** → **make it streaming** → agentic orchestration.
- **Orchestration:** LangGraph (graph approach); show **LangSmith traces** + an agent-graph simulation diagram in the demo; justify the chosen agentic design pattern explicitly ("why these agents, why this pattern").
- **Models:** develop on the cheapest viable providers (DeepSeek/OpenCode-class; bar: ~1,000 flow runs ≈ $5); plan a **multi-model mix per agent** (e.g., OpenAI-family for the web-research agent); note prompts are not portable across models.
- **Working style:** small chunks, commit ASAP, push continuously; next sync Sunday.
- **Design language:** crayon/cartoon aesthetic (minimalistic, black + vibrant accents) as deliberate contrast to glassy "AI-look" apps.
- **Presentation:** can be built as a web page in the same design system, including a landing page.

## Why
Follows the proven skeleton-first pattern, de-risks the new (Swift) parts with smallest steps, and concentrates effort where judging weight is (orchestration + cost story).

## Consequences
- Supabase enters the stack (auth). iOS-only at launch is accepted.
- Cost-optimization narrative (cheap dev models, per-agent model choice) becomes a pitch slide backed by numbers.
