# Decision — Text-first; voice is a light stretch demo only

**Date:** 2026-08-21 · **Status:** accepted (mentor sync)
**Source:** `../meetings/2026-08-21-mobile-first-strategy.md` (~11:15–16:30, 21:40–23:00)

## Context
Voice input would be a standout demo ("people say more than they type"), but: ElevenLabs has no Sinhala support; live-voice testing burns money; Gemini Live (real-time voice/video API) looks viable but credits are limited.

## Decision
Build and demo **text-first**. Voice is an optional, lightly-integrated stretch at the end: if time permits, wire Gemini Live as a thin "hello-world" showing it's possible, and pitch it as "implemented the potential; needs funding to scale." Never test the agentic flows through live voice — always trigger them via text/chat.

## Why
Protects budget and the Aug 29 deadline while still capturing the voice story for the pitch.

## Consequences
- Demo script stays keyboard-driven; voice appears (if at all) as a brief showcase moment.
- Sinhala voice remains an open problem for any future voice work.
