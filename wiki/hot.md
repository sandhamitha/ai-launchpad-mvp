# AI Launchpad MVP — Active Context

_Rewritten each session. ~300 words max. Start here._

## What this project is
"Weli AI" (working title) — Sri Lanka trip companion. Capped **mini-interview criteria engine → trip-timeline planner centerpiece**. **Flat supervisor + capped dynamic fan-out + verifier** (full pattern: `wiki/agentic-pattern.md`, #25 — reconciled w/ research, ready for mentor). SwiftUI client, **MV pattern** (`wiki/research-swiftui-architecture.md`); backend LangGraph on StateGraph + `Send` (`wiki/research-agentic-patterns.md`).

## 🎯 Aug 29: ≤20-min video + slides + diagram + accessible repo + Teamtailor (#29–#33) · elimination → top-10 live demo · **Sunday sync**

## Status (2026-08-22)
- ✅ **iOS scaffold running** — `mvp/ios/WeliAI/` (SwiftUI, iOS-only; xcuserdata ignored). Code home = `mvp/{ios,backend}` in THIS repo.
- ✅ Architecture locked both sides (MV client · hand-rolled supervisor backend).
- ✅ #25 pattern doc finalized → **send to mentor before Sunday**.
- Guardrails = build requirement (`wiki/agent-guardrails.md`, #34). Submission reqs filed (`wiki/submission-guidelines.md`).

## ⛔ Blockers (user)
**Anthropic key** (`~/.anthropic-key`) · **Docker running** — gate #23/#28 backend work.

## Build queue (board dates)
1. **#21** design tokens + `UIComponent` enum/view registry into scaffold (next up)
2. **#22** Supabase login · **#28** cost tests · **#25** send doc ✉️
3. **#23** SSE pipe (Aug 23–24) → **#24** LangGraph + **#26** criteria engine (Aug 24–26) → **#27** timeline planner (Aug 25–27)
4. #31 diagram (Aug 25–26) → #29/#30 video+slides (Aug 26–28) → #32/#33 (Aug 27–28)

## Gotchas
`/save-mvp` · `/log-meeting` · MV not MVVM-everywhere · `bytes.lines` drops SSE blank lines · never `stream_mode="values"` (state leak) · guardrails checklist at every step · safety copy: never "all-clear" · xcuserdata/DerivedData ignored.
