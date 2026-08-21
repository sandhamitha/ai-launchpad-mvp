# Mobile-First App Development Strategy — Mentor Sync

**Date:** 2026-08-21, 19:13 (~83 min)
**Participants:** Speaker 1, Speaker 2 — ⚠️ *Fireflies diarization is unreliable in this transcript (labels swap mid-call). Roles below are attributed as Mentee/Mentor only where the content makes it unambiguous.*
**Source:** `../transcripts/2026-08-21-mobile-first-strategy-transcript.md` (no Fireflies AI summary this time — distilled directly from transcript)

---

## TL;DR
The clickable prototype was screen-shared and reviewed positively. The session went deep on **how the agentic side must work**: a capped "mini-interview" context engine builds trip criteria up front; a **trip-timeline planner** (day-by-day, justified, weather/crowd-aware) becomes the product's centerpiece, visualized as an animated journey timeline (not a real map). For Swift, **predefined dynamic UI components** (backend picks which to render) replace true generative UI — OpenUI confirmed web-only. **Voice is a stretch demo only** (Gemini Live candidate); build stays text-first. Build order confirmed: crayon design system → Supabase basic login → chat ↔ LangChain hello-world → make it streaming → agent orchestration (LangGraph + LangSmith traces in the demo). Use cheap models during dev; multi-model per agent later. **Submission (presentation + demo) Aug 29 → elimination → top-10 live demo day. Next sync Sunday.**

## Decisions (see `../decisions/`)
1. **Context "mini-interview" criteria engine** — capped follow-up questions + tappable nudges build trip criteria before planning. → `2026-08-21-mobile-first-strategy-1.md`
2. **Trip-timeline planner is the centerpiece** — justified day-by-day roadmap + animated journey visualization; during-trip agent deferred past Aug 29. → `-2.md`
3. **SwiftUI = predefined dynamic components, not generative UI** — backend selects component + props; OpenUI is web-only. → `-3.md`
4. **Text-first; voice = light stretch demo only** (Gemini Live API candidate; ElevenLabs lacks Sinhala). → `-4.md`
5. **Build order + stack picks** — crayon design system, Supabase auth (basic), streaming hello-world first, LangGraph + LangSmith observability, cheap LLMs for dev / multi-model per agent, small commits pushed often. → `-5.md`
6. **Affiliate transparency model** — offers live in a separate "special offers/community" section; core recommendations stay neutral. → `-6.md`

## Key discussion points
- **Prototype feedback (positive):** flow praised; agent-count display fine for judges/MVP, but consumer copy should be friendlier ("finding the best ways to get there…") rather than agent internals. Thread-history dropdown idea noted for home screen.
- **Criteria building example:** "Ella, 3 nights" vs "Ella day-trip" must spawn different agents; budget (e.g., 50k LKR/night) shapes the hotel agent's search. Fixed common questions (when/where) + dynamic ones, capped (~5–6 messages / top 5–10 criteria) to control tokens and avoid endless questioning.
- **Timeline planner depth:** recommendations must carry justifications (weather, crowds, pacing — "forest first, then beach"); regenerate timeline when the trip slips. Stats/trade-off views (spend on stay vs transport) discussed as a differentiator.
- **Journey visualization:** animated, slightly gamified timeline (Duolingo-esque; curvy path, clickable stops). Real/3D maps rejected (SL lacks good 3D data); Google Maps API free tier (~10k requests) available for supporting data like busy-times.
- **Demo assets:** LangSmith traces + an agent-graph simulation diagram in the presentation; screen-recording tools exchanged (Recordly.dev); presentation itself can be a web page in the design system, incl. a landing page.
- **Watch (watchOS) hidden-gem notifications:** compelling story ("passing a hidden gem" ping while riding) — **mock/demo only**, no implementation commitment.
- **Preference-learning RAG pitch:** app learns user preferences across trips (food, budget style, e.g. halal checks) → retention story; "initially Sri Lanka, potentially global planner."
- **Model strategy:** research/web agent may use OpenAI-family models (stronger retrieval); dev on the cheapest viable models (DeepSeek/OpenCode mentioned) — cost test: "1,000 flow runs ≈ $5". Prompts aren't portable across models — pick early.
- **Submission criteria email reviewed:** problem → development workflow, demonstration/system walkthrough, architecture, cost/token optimization, testing/validation, future improvements. Validation idea: run a real mini-trip (e.g., Port City) and document it.
- **Process:** submit by Aug 29 → elimination round → top ~10 demo live at an event (Trace, TBC) with prep time; go live with recorded fallback.

## Timeline / milestones
- **Now → Sunday:** design system + Swift app shell, Supabase basic login, chat ↔ LangChain hello-world, then streaming; small commits pushed continuously.
- **Sunday (Aug 23) or Monday:** next mentor sync — bring progress + chosen agentic design patterns.
- **Aug 29:** presentation + demo submission. Later: top-10 live demo day (venue TBC).

## Action items
**Mentee**
- [ ] Mac screen repair visit (hardware down — working from external monitor/iPad)
- [ ] Set up the crayon design system in the Swift app (extract visual elements from reference designs to text via Claude, then implement)
- [ ] Basic login via Supabase (non-functional acceptable for demo)
- [ ] Connect chat → LangChain backend: hello-world round trip, then make it streaming
- [ ] Derive the agentic design pattern from this transcript; send candidate patterns to mentor for feedback
- [ ] Cost-test cheap model providers ("1,000 runs ≈ $5" bar); plan per-agent model mix
- [ ] Commit small chunks, push often; bring progress to Sunday sync

**Mentor**
- [ ] Send screen-recording tool link + observability (LangSmith/LangChain) instructions and feedback
- [ ] Review the design patterns the mentee sends
- [ ] Template prototype if time permits (explicitly no promise)
- [ ] Post remaining question on chat

## ⚠️ Open flags
1. **Diarization unreliable** — Speaker 1/2 labels swap mid-transcript; role attributions above are content-based.
2. **Voice cost risk** — if attempted, test via text-triggered flows, never by burning live-voice calls.
3. **Watch demo is a mock** — must be framed honestly in the pitch ("possible, needs funding").
4. **Judging process (top-10 at Trace) is assumption**, not confirmed.
5. **Google Maps free-tier figure (~10k)** — verify before relying on it.
6. Affiliate framing must stay transparent in the pitch (separate offers section) to avoid neutrality questions from judges.
