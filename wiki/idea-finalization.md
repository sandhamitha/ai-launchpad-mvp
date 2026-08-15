# Idea Finalization Checklist — Agentic SL Travel Companion

_Filling mentor's Issue 1 checklist. Drafted 2026-08-15, mostly from existing Visit SL research. Legend: ✅ answered · ✍️ draft, confirm · ⚠️ NEEDS YOUR INPUT (can't invent) · 🔬 TODO experiment._

---

## Problem & hypothesis

**✍️ Falsifiable hypothesis**
> Western independent leisure travelers (UK / Germany / Australia / France, ~25–45, mid-to-high budget) planning a 1–3 week Sri Lanka trip currently assemble their plans from generic AI (ChatGPT/Gemini), Reddit (r/srilanka, r/solotravel), TripAdvisor forums, and travel blogs — evidenced by the high recurring Q&A volume in those forums — and it costs them many hours of fragmented research plus real on-trip friction (missed train bookings, safety uncertainty, no real-time local awareness).

- **✅ Workaround people use today:** generic ChatGPT/Gemini, r/srilanka + TripAdvisor SL forum, travel blogs/YouTube, WhatsApp groups, and paid DMCs/tour agents.
- **✅ Talked to real users (behavior-based):** Yes — informal interviews done. Findings:
  - Core pain = **accuracy + timing**: info exists (internet, scattered APIs) but *none of it is accurate or well-timed*.
  - Unmet moment = the **brainstorming / ideation stage** — they want a dedicated space to think through a trip *without* defaulting to a generalist AI (which lacks proper SL access/tooling).
  - What they need surfaced: **exact places at the right time, hidden gems, specific venues, live weather/conditions** — accurate, on-time.
- **✅ Named competitors:** Layla, Mindtrip, GuideGeek (generalists, no SL depth); SLTDA official app (static). Not a green-field-with-no-competitors red flag — good.

## Problem–model fit
- **🔬 Real prompt test on messy data:** run the **15 recurring tourist questions** (from the Visit SL spec) against Claude *with* vs *without* SL-RAG; evaluate on messy/edge cases. → This is the Day-2 eval; proves the model can actually do it.
- **✅ Why it should hold:** these are factual, retrievable, SL-specific queries — a good fit for RAG + tool-calling, not open-ended judgment.

## Agent-native fit — CORE (locked 2026-08-15)

**Core identity:** a **brainstorming / discovery companion** for planning a Sri Lanka trip — NOT an itinerary generator, and NOT "a SaaS with a chatbot." It's a **multi-agent intelligent system**.

- **The core responsibility:** own the **ideation/brainstorming moment** — surface *accurate, on-time* SL knowledge (exact places, hidden gems, specific venues, live weather/economy/conditions) that generalist AI can't, so travelers brainstorm inside it instead of defaulting to Gemini/ChatGPT.
- **Multi-agent architecture (your articulation):** a **generalist/supervisor agent** that dynamically **spawns & allocates specialized, tool-using sub-agents** for discrete tasks (discovery, weather, transport, safety/emergency, deals). *"Why 5 agents, not 1"* is the story judges will probe (confirmed in transcript).
- **Adaptive persona (your addition):** the system **reads what kind of user it's talking to and adapts tone + depth of info** accordingly.
- **Intelligent live companion (from transcript):** background agents that keep up with the traveler during the trip — reroute on floods/roadblocks, surface deals, handle SL-specific emergencies from the guide-reviewed KB.
- **Generative UI (from transcript):** cached interactive components (images, graphs, follow-up questions) — richer than plain chat, low token cost.
- **✅ Remove-the-AI test:** without the agents it's just a static guidebook → passes.
- **✅ Task category:** retrieval + tool-use + orchestration — feasible, not open-ended judgment.

## AI-native design (Visible × Understandable × Controllable × Recoverable)
- **✍️ Visible:** companion surfaces suggestions at the planning moment + live during the trip (not buried in a menu).
- **✍️ Understandable:** every answer shows a one-line rationale + source (Weli/guide-reviewed KB) one click away.
- **✍️ Controllable:** user edits/overrides the itinerary, accepts/dismisses alerts, without leaving the flow.
- **✍️ Recoverable:** on a wrong/low-confidence answer → graceful fallback + "guide-reviewed" flag, never a broken screen.

## Market & demography
- **✅ Specific user:** Western independent traveler, 25–45, mid-high budget, English-first researcher, mobile-heavy, **moderate-to-high automation trust** (books own trips already).
- **✍️ One-sentence value prop:** *"A Sri Lanka trip-planning companion for the brainstorming stage — accurate, on-time local knowledge (exact places, hidden gems, live weather/conditions) that generalist AI can't give you."*
  - _Wedge (from interviews): own the **ideation/brainstorming moment** with accuracy + timing — the exact place generalist AI fails._
- **✍️ Exact search query a user types:** "best 2 week sri lanka itinerary", "is sri lanka safe for solo female travel", "how to book kandy to ella train".
- **✅ Pitch segment:** inbound Western independent travelers (confirmed) — the researched moat; no domestic split.

## Regional & local fit
- **✅ Local factor / moat:** SL-specific, guide-reviewed knowledge (Weli, 30-yr tour guide) + SL real-time conditions (weather/transport/safety) + local emergency/cultural nuance — data generalists don't have.
- **✅ "What wins if Google copies it tomorrow?":** the local **data relationship + guide-reviewed trust + SL-specific real-time integrations** — not replicable by a generalist quickly.

## Distribution & targeting
- **✅ First-20-users channel:** r/srilanka + SL travel Facebook groups — post in-community, answer real trip questions, invite to try.
- **⚠️ First champion user:** none yet → near-term action: recruit one from r/srilanka / FB groups (an active answerer or a small SL-travel creator).

## Longevity & business viability
- **✍️ Unit economics at 10×:** per-query LLM cost is the risk (agent = per-execution cost). Mitigations already in plan: **Haiku routing + response caching + $100/mo cap.** Needs to survive scale.
- **⚠️ Weekly return? — HONEST flag:** travel is **episodic (per-trip), not weekly** like SaaS. That's a real retention question the mentor/judges may raise. Answers: monetize per-trip (affiliate/booking) + B2B whitelabel for DMCs (recurring) — not weekly-active-user logic.
- **✅ Build-buy:** BUY foundation models, auth, vector infra; BUILD the SL KB + agentic flows (the differentiator). Correct split.
- **✅ Six-month durability:** moat is the KB + guide relationship, not a thin LLM wrapper → durable against commoditization.

## Commitment gate
- **Done:** interviews ✅ · distribution channel ✅ · pitch segment ✅ · hypothesis / value-prop / agent-fit / regional moat / build-buy ✅
- **Remaining (non-blocking for the PoC build — close in parallel):**
  1. Recruit a **champion** (from r/srilanka / FB groups)
  2. Gather a **willingness-to-act** signal (small waitlist / DM interest via those channels)
  3. Run the **problem-model-fit experiment** (15 questions ± RAG) — Day 2 of the build
- **Verdict:** idea is **committed enough to start building the PoC.** ✅
