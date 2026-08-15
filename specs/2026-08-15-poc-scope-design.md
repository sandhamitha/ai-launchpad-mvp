# PoC Scope & Design — SL Brainstorming/Discovery Companion

**Date:** 2026-08-15 · **Status:** draft for review · **Deadline:** PoC ~Aug 22, pitch Aug 29
**Follows:** mentor board Issue 2 (feature breakdown) · builds on `wiki/idea-finalization.md`

---

## 1. What we're building (recap of the locked idea)
A **brainstorming/discovery companion** for planning a Sri Lanka trip — a **multi-agent intelligent system**, not a SaaS with a chatbot. A supervisor agent **dynamically spawns** specialized tool-using sub-agents to surface **accurate, on-time** SL knowledge (exact places, hidden gems, live conditions) that generalist AI can't. Light adaptive persona. Generative UI. Target user: inbound Western independent travelers.

## 2. Architecture — dynamic multi-agent orchestration

```
User ──► Chat UI (streaming + generative components)
              │
              ▼
       SUPERVISOR agent
       · classifies intent + light persona (backpacker / comfort / family)
       · decides AT RUNTIME which agents to spawn (0..n, parallel)
       · synthesizes results → streamed answer + UI components
              │
    ┌─────────┼──────────┐        AGENT REGISTRY (extensible)
    ▼         ▼          ▼        each agent = {purpose, tools, guardrails}
 DISCOVERY  WEATHER    SAFETY     future: Hotels/Affiliate, Transport, Deals —
 (RAG, SL KB) (live API) (KB+live)  register & go, no re-architecture
```

**Why multiple agents (the judge answer):** each has distinct tools, data, and guardrails —

| Agent | Tools | Data | Guardrail |
|---|---|---|---|
| Discovery | vector search | guide-reviewed SL KB (pgvector) | KB-grounded only; cite source; on miss → report to supervisor (no hallucination) |
| **Web-Search (fallback)** | web search tool | live web | **spawned only on KB miss**; results always tagged `web-sourced`, never `guide-reviewed`; handles misspellings ("did you mean…") and non-SL places (polite correction + SL alternatives) |
| Weather | live weather API | real-time | always timestamp + source |
| Safety | KB lookup **+ live hazard/advisory APIs** | SL emergency/cultural **+ real-time conditions** | timestamp + cite; always show official contacts; **never fabricate an all-clear** |

**Why dynamic:** the supervisor spawns only what the query needs — "hidden southern beaches, safe now?" → Discovery + Safety (+ Weather); "best time for Yala?" → Discovery + Weather; **obscure place not in the KB → Web-Search agent spawned on the spot** (dynamic spawning, visible live in the demo). Extensibility demo: registering a Hotels/Affiliate agent shows the pattern scales.

## 3. Full feature list (MECE — all sources)

From the validated problem, user interviews, competitor audit, the 2026-07-27 planning transcript, and the Visit SL research (15 recurring questions):

**A. Conversation & input:** free-text trip describing · clarifying/follow-up questions · follow-up question chips · multi-turn context memory
**B. Discovery & knowledge:** exact-place suggestions (KB-grounded) · hidden gems · seasonal/timing advice · source/review citations · budget-aware suggestions
**C. Real-time:** live weather · hazard/flood alerts · travel-advisory levels · road/transport disruptions · local deals/offers
**D. Safety:** area safety check · official emergency contacts · cultural do's/don'ts · solo-female-safety guidance
**E. Personalization:** traveler-type detection · tone/depth adaptation · persistent preference profile
**F. Orchestration (the differentiator):** supervisor intent routing · dynamic agent spawning · parallel execution + synthesis · agent registry extensibility · visible agent activity (for judges) · guardrails per agent
**G. UI:** streaming chat · place cards (image/region/why-now) · weather card · interactive graphs · trip mood-board
**H. Planning (adjacent):** day-by-day itinerary builder · itinerary persistence/sharing · calendar view
**I. Live companion (adjacent):** trip-plan tracking · proactive notifications · en-route rerouting · emergency assistance flow
**J. Commerce:** hotel/places agent with affiliate links · booking handoffs
**K. Platform:** auth · mobile app · deploy · eval harness · observability/traces

*(A–G feed the PoC; H–K are the deferred pools — see §4 MoSCoW.)*

## 3b. User story map (features stacked under activities)

| **Describe idea** | **Discover / Brainstorm** | **Refine** | *Plan (deferred)* | *Live trip (deferred)* |
|---|---|---|---|---|
| free-text input | KB place suggestions + citations | follow-up chips | itinerary builder | plan tracking |
| clarifying questions | hidden gems | budget re-filtering | persistence/share | proactive alerts |
| persona detection | live weather in answers | tone/depth adaptation | calendar view | rerouting |
| context memory | safety check + contacts | visible agent activity | booking handoff | emergency flow |
| | place/weather cards | interactive graphs | | deals en route |

**── MVP line:** everything in the first three columns *above* this table's plain-text features is in play; the exact cut is §4. Deferred columns ship after the PoC. **──**

## 4. MoSCoW scope

**MUST (the PoC):**
- Streaming chat + 2–3 cached generative components (place card w/ image, weather card)
- Supervisor with dynamic parallel agent selection over an extensible registry
- Discovery (RAG over seeded SL KB), Weather (live), Safety (KB + live hazard/advisory), **Web-Search fallback (spawn-on-KB-miss — the live dynamic-spawning demo)**
- Light persona (2–3 traveler types → tone/depth)
- Seeded SL KB (~20–30 chunks from the 15 recurring tourist questions) · local pgvector · local embeddings
- LangSmith traces · runs locally · Anthropic API

**SHOULD (if time):** follow-up question chips · 15-question eval harness (problem-model-fit proof) · Hotels/Places+affiliate agent as the live extensibility demo

**COULD (defer):** Transport agent · Deals agent · live-trip triggers · itinerary persistence

**WON'T (this round):** real auth (fake login OK per mentor) · Modal deploy · Sanity pipeline · whitelabel · MCP/REST · mobile

## 5. User stories + acceptance criteria (MUSTs)

**S1 — Describe a trip idea.** As a traveler, I want to describe a vague trip idea in chat, so that I can start brainstorming without a rigid form.
- Given the app is open, When I type "2 weeks in December, beaches + wildlife, mid budget", Then I get a streamed response within ~5s referencing my constraints.
- Given an empty/gibberish message, When I send it, Then I get a friendly clarifying question, not an error.
- Given the backend is down, When I send a message, Then the UI shows a clear retry state, not a blank screen.

**S2 — Grounded discovery with graceful fallback.** As a traveler, I want suggestions of exact places incl. hidden gems — and a smooth answer even when I ask about something obscure or misspelled — so that I'm never hard-rejected back to a generalist AI.
- Given a query about places, When Discovery runs, Then every KB suggestion carries a visible `guide-reviewed` source tag.
- Given a query with no good KB match, When Discovery reports the miss, Then the supervisor **spawns the Web-Search agent** and answers from live search — clearly tagged `web-sourced` (never disguised as guide-reviewed, never a bare "I don't know").
- Given a misspelled place name (e.g. "Sigirya"), When Discovery/search resolves it, Then the answer offers "did you mean **Sigiriya**?" and proceeds with the correction.
- Given a place that isn't in Sri Lanka, When search reveals that, Then the answer politely notes it's outside Sri Lanka and pivots to comparable SL alternatives (error path — no dead end, no hallucinated SL location).
- Given a place suggestion, When rendered, Then it appears as a place card (image, region, why-now, provenance tag) — not a text wall.

**S3 — Real-time conditions.** As a traveler, I want current weather context, so that timing advice is accurate today.
- Given a query naming a region/season, When Weather runs, Then the response includes live data with timestamp + source.
- Given the weather API fails, When Weather runs, Then the supervisor still answers from KB seasonal knowledge and marks live data unavailable.
- Given a query about a future month (e.g. "December"), When Weather runs, Then it gives seasonal/monsoon guidance from KB — clearly distinguished from today's live reading (no pretending a forecast reaches that far).

**S4 — Safety check.** As a traveler, I want to know if an area is OK right now, so that I can plan with confidence.
- Given "is X safe right now?", When Safety runs, Then the answer combines KB guidance + live hazard/advisory, with timestamp + official emergency contacts.
- Given uncertain live data, When Safety runs, Then it says so and links the official advisory — never an unqualified all-clear.
- Given any safety-related query, When the final answer is rendered, Then SL official emergency contacts are visibly included.
- Given both live sources are unreachable, When Safety runs, Then the answer serves KB guidance clearly marked "live status unavailable — verify via official advisory" (error path).

**S5 — Dynamic orchestration (demo-visible).** As a judge, I want to see which agents ran and why, so that I can evaluate the agentic workflow.
- Given any query, When it is answered, Then the UI/trace shows which agents the supervisor spawned and why.
- Given a simple factual query, When the supervisor routes it, Then only the needed agent(s) run — not all of them.
- Given a compound query, When the supervisor routes it, Then multiple agents run in parallel and results merge into one coherent answer.
- Given one agent fails or times out mid-parallel, When results are synthesized, Then the answer still delivers the successful agents' content and names what's missing — never a total failure (error path).

**S6 — Light persona.** As a traveler, I want the tone/depth to match my style, so that answers feel personal.
- Given budget-backpacker phrasing, When I ask for suggestions, Then suggestions and tone skew budget (and vice versa for comfort/luxury).
- Given no persona signal, When I ask anything, Then a neutral default is used — never a wrong loud assumption.
- Given mixed/contradictory signals (e.g. "cheap luxury honeymoon"), When persona is detected, Then the system asks one light clarifying question instead of guessing.

### INVEST check (all six stories)
| Story | I | N | V | E | S | T | Note |
|---|---|---|---|---|---|---|---|
| S1 chat input | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | skeleton story — builds first |
| S2 discovery | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | depends only on seeded KB |
| S3 weather | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | independent tool + fallback |
| S4 safety | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | KB + live, strictest guardrails |
| S5 orchestration | ✅ | ✅ | ✅ | ✅ | ⚠️ | ✅ | biggest story — if it slips, split: (a) routing visible in traces only → (b) in-UI agent activity |
| S6 persona | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | prompt-level; no model training |

## 6. Walking skeleton (build order)
1. Chat UI → `/chat` → supervisor → **one stubbed agent** → streamed reply → one card rendered *(end-to-end day 1–2)*
2. Real Discovery + RAG (seed KB, pgvector, local embeddings)
3. Weather agent (live) → 4. Safety agent (KB + live) → 5. persona → 6. generative components → 7. eval harness → 8. traces/polish

## 7. Reuse from weli-backend
FastAPI shell, pgvector migration/schema, LangChain agent scaffold, LangSmith setup, pytest/CI — the PoC extends this repo rather than starting fresh.

## 8. Risks & fallbacks
- **Agentic slip** (mentor's route-B): registry degrades gracefully — even 1 real agent + supervisor still demos the pattern.
- **Live API flakiness on demo day:** cache last-good responses; every live answer carries its timestamp.
- **KB too thin:** scope KB to the 15 questions' topics — depth over breadth.
- **Solo + 1 week:** MUSTs only until skeleton is fully walking; SHOULDs strictly after.

## 9. Not resolved here (parallel track)
Champion user · willingness-to-act signal · wireframes (mentor Issue 3) · confirm next mentor call date.
