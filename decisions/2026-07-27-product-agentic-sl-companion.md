# Decision — MVP is an agentic travel companion for Sri Lanka

**Date:** 2026-07-27
**Status:** accepted
**Source:** Planning session `../meetings/2026-07-27-agentic-companion-planning.md`

## Context
The AI Launchpad MVP concept was undefined. Needed a product that is both idea-strong and technically deep enough to stand out among 50 participants and convince judges/investors.

## Decision
Build an **agentic travel planner/companion for Sri Lanka**: multiple autonomous agents (weather, traffic, emergencies, personalized travel advice) doing dynamic planning that adapts to real-world changes — not a static recommendation engine.

## Why
- Judges weight **agentic-workflow sophistication + technical depth**, not just idea novelty — agents are where we can show depth.
- Sri Lanka focus unlocks **unique local data** unavailable to generalist tools, addressing real traveler pain (emergencies, culturally relevant advice) → stickiness + trust.
- Vision (Speaker 1): build genuinely autonomous systems now; "companion that actively manages plans."

## Consequences / trade-offs
- Core engineering effort concentrates on robust, explainable agentic workflows with guardrails.
- Higher complexity than a chatbot → needs scope discipline + fallback plan (see tech decision).
