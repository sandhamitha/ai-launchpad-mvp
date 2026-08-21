# AI Launchpad MVP — Active Context

_Rewritten each session. ~300 words max. Start here._

## What this project is
A **brainstorming/discovery companion** for Sri Lanka trips — dynamic multi-agent system. Core defined at the 2026-08-21 sync: a capped **mini-interview criteria engine** feeds a **trip-timeline planner (the centerpiece)** — justified day-by-day plan + animated journey timeline (not a map). During-trip live agent **deferred past Aug 29**. Provenance badges + guide-reviewed KB remain the trust story. Working title "Weli AI" (#13 open).

## 🎯 Aug 29 submission → elimination → top-10 live demo (Trace, TBC) · **next sync Sunday**

## Client & stack (locked 2026-08-21, see decisions/-1…-6)
- **SwiftUI with predefined dynamic components** — NOT generative UI (OpenUI is web-only; A2UI ~3× cost). JSON chunks → view registry.
- **Text-first**; voice = thin stretch demo only (Gemini Live candidate).
- **Build order:** crayon design system → **Supabase basic login** → chat↔LangChain hello-world → streaming → **LangGraph orchestration + LangSmith traces**.
- **Cheap models for dev** (~1,000 runs ≈ $5), multi-model per agent mix.
- Affiliate transparency: separate offers section, neutral recommendations.

## Prototype feedback (#18)
Reviewed positively — agent-count display fine for judges; make consumer copy friendlier.

## My action items (due before Sunday)
- [ ] Mac repair · [ ] crayon design system in Swift · [ ] Supabase login · [ ] hello-world chat round-trip → streaming · [ ] derive agentic pattern from transcript → send mentor · [ ] cost-test cheap providers · [ ] small commits
- Mentor owes: recording-tool link · LangSmith instructions · design-pattern review · (maybe) template prototype.

## Gotchas
`/save-mvp` · `/log-meeting` · diarization unreliable in tonight's transcript (roles attributed only where unambiguous) · watch-notification demo = mock, frame honestly · Google-Maps free-tier figure unverified · safety copy: never "all-clear".
