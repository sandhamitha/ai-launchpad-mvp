# AI Launchpad MVP — Active Context

_Rewritten each session. ~300 words max. Start here._

## What this project is
A **brainstorming/discovery companion** for Sri Lanka trips — dynamic multi-agent system (supervisor spawns Discovery/RAG, Weather, Safety, Web-Search on KB miss). Generative UI = **streamed JSON component chunks + view registry** (SSE-first per mentor's guide, `wiki/mentor-streaming-guide.md`). Provenance badges (`guide-reviewed`/`web-sourced`/`live`). Working title "Weli AI" (#13 open). MCP later (#12).

## 🎯 Pitch/presentation submission: **Aug 29**

## Status (2026-08-21) — prototype DELIVERED
- **#18 clickable mobile prototype done + pushed** → `prototypes/weli-ai-mobile-prototype.dc.html` (open in browser; flow rail jumps states). Onboarding → Home → Chat ×3 (brainstorm/fallback/safety). CC-licensed photos w/ credits, original sticker SVG, pure-CSS iPhone frame (standalone-safe).
- Mentor board items all Done: #1 idea · #2 features · #3 wireframes · #18 prototype.
- Design assets live in Claude Design project "Weli AI travel companion" (wireframes + prototype); Classical system (Cormorant Garamond/Lora, cream + amber).
- **Build NOT started.**

## ⛔ Blockers (user)
1. **Anthropic API key** → `~/.anthropic-key` · 2. **Docker running**

## Next
- Ping mentor: review prototype (`prototypes/…dc.html`, pull + open in browser) + restart daily calls
- Decide **#19** (mobile-first wireframes) — likely satisfied by the prototype → close?
- **Start the build**: backend SSE pipe first (mentor guide build order), then agents; #10 KB seed (60–100 chunks); #20 canvas scope; #11 smoke eval
- Aug 26–28 reserved: deck + recorded demo + #16 Gemini side-by-side

## Gotchas
`/save-mvp` · `/log-meeting` · mobile Obsidian pull-only · safety copy: never "all-clear" · Commons images carry CC BY-SA credits (keep them) · ios-frame.jsx is Claude-Design-only (CSS frame is the standalone path).
