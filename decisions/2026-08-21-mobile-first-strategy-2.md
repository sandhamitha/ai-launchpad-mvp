# Decision — Trip-timeline planner is the product centerpiece

**Date:** 2026-08-21 · **Status:** accepted (mentor sync)
**Source:** `../meetings/2026-08-21-mobile-first-strategy.md` (~27:00–32:30, 50:00–53:00)

## Context
The pitch must prove intelligent orchestration, not list-printing. Judges will probe how the agents work together.

## Decision
Build a **roadmap/timeline planner orchestration**: from the gathered criteria, agents compose a justified day-by-day plan ("arrive Tuesday → rest → Yala Wednesday because Tuesday has heavy rain and crowds; forest first, surf after") with reasons backed by data (weather, crowds, pacing, budget fit). Visualize it as an **animated journey timeline** — curvy path / slightly gamified (Duolingo-esque), clickable stops — NOT a real/3D map (Sri Lanka lacks good 3D data). Timeline must regenerate when plans slip. The **during-trip background agent** (live re-planning on the journey) is deferred past Aug 29 — plan it, don't build it.

## Why
The "personal travel assistant that does the thinking" is the differentiator vs summarization apps; the animated timeline gives the demo a wow-moment; deferring the live agent protects the deadline.

## Consequences
- A dedicated planner/timeline agent joins the orchestration design.
- Google Maps API (free tier ~10k requests — verify) available for supporting data (e.g., busy times).
- Preference-learning across trips (RAG on user decisions) becomes the retention story for the pitch.
