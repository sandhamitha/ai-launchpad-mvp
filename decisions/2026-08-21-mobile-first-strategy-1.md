# Decision — Context "mini-interview" criteria engine before planning

**Date:** 2026-08-21 · **Status:** accepted (mentor sync)
**Source:** `../meetings/2026-08-21-mobile-first-strategy.md` (~07:00–12:45)

## Context
Different trips need different agent work ("Ella 3 nights" ≠ "Ella day-trip"; budget shapes hotel search). Letting the LLM freely ask questions risks endless loops and token burn.

## Decision
On trip initiation, an initial agent runs a short "mini-interview" to build the trip criteria: a fixed set of common questions (when/where/how long) plus dynamic, situation-based ones — **capped (~5–6 messages, top 5–10 criteria)**. Use tappable suggestion "nudges"/interactive elements so users select rather than type. Generic user input → generic (cheaper, fixed) initial questions; dynamic questioning only where it adds value. Agent spawning then adapts to the built criteria.

## Why
Personalized planning requires context, but judges and users both punish an app that interrogates endlessly; caps + nudges balance intelligence, UX and token cost.

## Consequences
- Supervisor needs a criteria model that downstream agents consume (budget, party size, dates, vibe, constraints).
- Question-cap + fixed-question fallback becomes a prompt/graph design requirement.
