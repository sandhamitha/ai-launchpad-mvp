# Decision — Streaming backend: FastAPI + WebSockets, prototyped early

**Date:** 2026-08-17 · **Status:** accepted (mentor sync)
**Source:** `../meetings/2026-08-17-prototype-ux-alignment.md` (54:00)

## Context
Real-time streaming is critical for interactive chat, incremental multi-agent results, and low-latency updates. Mentor recommends de-risking it first.

## Decision
Backend streaming via **FastAPI + WebSockets** (or similar low-latency channel). **Prototype the streaming mechanics early** — start with a simple chat UI connected to the Python backend before layering agents on top. Parallel workstreams: UI prototype ∥ streaming backend. Mentor to supply a technical guide (incl. Swift streaming considerations).

## Why
Streaming is the highest-risk integration; validating it first prevents a demo-day surprise. Matches the walking-skeleton principle already in the spec.

## Consequences
- ⚠️ Spec assumed SSE-style streaming — WebSockets vs SSE to be settled at the architecture session (WebSockets favored by mentor; SSE simpler for one-way streams).
- S1 (skeleton) explicitly becomes: chat UI ↔ FastAPI streaming round-trip FIRST, stubbed agent second.
