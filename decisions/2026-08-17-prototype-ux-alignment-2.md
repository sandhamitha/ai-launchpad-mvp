# Decision — Landing page ↔ main app (canvas) split

**Date:** 2026-08-17 · **Status:** accepted (mentor sync)
**Source:** `../meetings/2026-08-17-prototype-ux-alignment.md` (12:40, 29:00)

## Context
One surface was serving both marketing and product. Mentor sync separated the concerns.

## Decision
- **Landing page:** anonymous users, captures the first query, generic suggestions, marketing/onboarding — funnels into the app.
- **Main app = dynamic canvas:** the interactive trip-planning space — plan, view, update trips with real-time status and contextual nudges (weather alerts, transport info) through the journey.
- Maps/globe integrations **deferred** (device compatibility) — canvas starts simple.

## Why
Balances marketing needs with functional usability; enables personalization *after* initial engagement; gives the episodic "resume your trip" behavior a proper home.

## Consequences
- New UX surface ("canvas") needs a wireframe frame + a thin-slice scope for the PoC.
- Landing page ticket #15 gains a second purpose: it IS the top of this funnel (waitlist + first-query capture).
