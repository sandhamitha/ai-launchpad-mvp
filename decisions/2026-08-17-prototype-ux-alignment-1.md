# Decision — Mobile-first UI (supersedes desktop-first orientation)

**Date:** 2026-08-17 · **Status:** accepted (mentor sync)
**Source:** `../meetings/2026-08-17-prototype-ux-alignment.md` (33:45)

## Context
Wireframes v3.1 were locked desktop-first (projector demo logic). Mentor sync refocused on real traveler behavior.

## Decision
Design **mobile-first**: quick on-the-go interactions, sliders/interactive inputs, minimal visuals, clear next steps, light on browser resources. Native mobile (Swift preferred / React Native) to be **explored** for location + real-time features — exploration only, not a PoC commitment.

## Why
Travelers use this on phones mid-trip; adoption depends on real-world usability, not demo-room ergonomics.

## Consequences
- Wireframes need a **mobile-first v4 pass** — AgentTrace, provenance badges, chips, pin/steer all carry over; layout reorients.
- Desktop remains the projection surface for the pitch demo (responsive, not abandoned).
- ⚠️ Swift app by PoC (Aug 22) unrealistic — keep native exploration post-PoC.
