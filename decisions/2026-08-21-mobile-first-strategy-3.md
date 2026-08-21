# Decision — SwiftUI dynamic components (predefined), not generative UI

**Date:** 2026-08-21 · **Status:** accepted (mentor sync)
**Source:** `../meetings/2026-08-21-mobile-first-strategy.md` (~35:00–45:20)

## Context
OpenUI (used for web demos) is confirmed web-first — unusable in Swift. Google's A2UI exists for Swift but benchmarks at roughly triple the API cost vs OpenUI; premium models (Claude/Opus) for UI generation are unaffordable for this build.

## Decision
Go with **"dynamic UIs, not generative UI"**: hardcode a library of SwiftUI components; the backend decides **which component to render with which props** per response. Generative behavior is reserved for the interaction layer (follow-up question forms, suggestion nudges — the pattern demonstrated in the Thesys/OpenUI demo).

## Why
Keeps the live, composed-UI feel (backend-driven rendering) at near-zero incremental token cost, fits SwiftUI's strengths, and matches the budget reality.

## Consequences
- A fixed component library + a backend "component + props" protocol becomes the client contract.
- Web surfaces can still use OpenUI later off the same backend protocol.
