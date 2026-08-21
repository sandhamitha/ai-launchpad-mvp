# Weli AI — Agentic Design Pattern (Final)

_Ticket #25 deliverable · 2026-08-22 · synthesized from a 3-track research pass (pattern survey · LangGraph implementation · production case studies), ~70 cited sources. For mentor review._

## The chosen pattern

**Flat Supervisor–Worker with Capped Dynamic Fan-Out**, plus a **verification pass** on consequence-bearing outputs and a **sequential assembly** step. Composition (each block is an established, documented pattern):

```
User ──► INTERVIEW agent (capped mini-interview → structured TripCriteria)
              │            [interrupt() gate: user confirms criteria chips]
              ▼
        SUPERVISOR (flat, single level)
              │  decides AT RUNTIME which workers to spawn (Send API, parallel)
    ┌─────────┼──────────┬────────────┐
    ▼         ▼          ▼            ▼ (conditional — KB miss only)
 DISCOVERY  WEATHER    SAFETY      WEB-SEARCH
 (RAG, KB)  (live API) (KB+live)   (spawn-on-miss)
    │         │          │            │     each returns a STRICT, CONDENSED
    └─────────┴────┬─────┴────────────┘     Pydantic schema — never raw dumps
                   ▼
        VERIFIER (lightweight critic — Safety & Weather outputs only)
                   ▼
        PLANNER/ASSEMBLER (sequential): justified day-by-day TIMELINE
        with provenance labels + explicit-assumption banners
```

## Why this pattern (evidence-backed)

1. **It has a proven production analog:** Anthropic's own Research feature uses exactly this shape — a lead agent dynamically spawning specialized subagents — measuring **+90.2% over single-agent** on breadth tasks ([Anthropic engineering](https://www.anthropic.com/engineering/built-multi-agent-research-system)). The industry is moving the same way: Expedia acquired Layla (Jul 2026) explicitly to build "multi-agentic" travel ([Skift](https://skift.com/2026/07/31/expedia-acquired-ai-trip-planner-layla-exclusive/)).
2. **Flat is right-sized:** hierarchical orchestration only pays off past ~5–8 workers; we have 4 ([Beam.ai](https://beam.ai/agentic-insights/multi-agent-orchestration-patterns-production)).
3. **The economics demand conditional spawning:** multi-agent runs cost ~15× a single chat (Anthropic). Our KB-first / Web-Search-only-on-miss design pays the expensive path only when the cheap path fails — a demonstrable guardrail, not just an optimization.
4. **Explainability is scored:** supervisor routing is a discrete, traceable decision (LangSmith node traces + our live AgentTrace UI). Judging rubrics increasingly weight reasoning-trace transparency as heavily as output quality ([Galileo](https://galileo.ai/blog/agent-evaluation-framework-metrics-rubrics-benchmarks)).
5. **The failure data endorses the additions:** the MAST taxonomy (NeurIPS 2025, 1,600+ annotated failures) finds most multi-agent failures are orchestration design — with "task verification gaps" a top category ([arXiv 2503.13657](https://arxiv.org/abs/2503.13657)). Hence the verifier pass on Safety/Weather before anything enters the timeline.

## Framework choice: LangGraph (considered: Google ADK)

Google's ADK offers comparable orchestration primitives (Sequential/Parallel/Loop agents) and was considered. LangGraph wins for this build on ecosystem gravity, not capability: the mentor-aligned stack (LangChain streaming guide, LangSmith zero-config observability), a mature supervisor + `Send` fan-out story with documented pitfalls we've already mapped, and per-node model mixing that covers the "use Gemini where it's cheap" case without a second runtime. Mixing frameworks would double state/streaming/error-handling surfaces for zero judged benefit — the pattern, not the framework, is the differentiator. ADK/A2A remains a roadmap item alongside MCP for exposing Weli's agents to third-party assistants.

## Rejected alternatives (and why)

| Pattern | Why not |
|---|---|
| **Swarm / peer-to-peer handoff** | Weak explainability (handoff chains reconstructed post-hoc), no default per-agent permission story, no central cost chokepoint — the opposite of what judges score. |
| **Plan-and-Execute (full replanning loop)** | Upfront plans don't survive live-data contact (weather/safety changes); replanner adds a week we don't have. Its useful half (structured planning) lives in our Assembler. |
| **Single ReAct agent** | No parallelism, no per-agent tool scoping, monolithic trace — and "one agent with many tools" is the pattern all criticized competitors (Layla-class) effectively use. ReAct still runs *inside* each worker. |
| **Hierarchical (supervisor-of-supervisors)** | Premature below 5–8 workers. |

## Product-specific design rules (from competitor failure analysis)

- **Never silently assume.** The most-criticized UX failure across Layla/Mindtrip-class products is silent guessing on ambiguous input. Our rule: the interview cap limits *question count*, never *ambiguity tolerance* — an unresolved hard constraint either extends the interview one turn or renders an explicit **assumption banner** in the trace. ([aitravel.tools](https://aitravel.tools/layla-vs-mindtrip-vs-wonderplan/))
- **Provenance is enforced server-side** (`guide-reviewed` / `web-sourced` / `live·timestamp`) — the model cannot mint trust labels; and web-sourced content is summarized into structured fields, never passed through raw (prompt-injection boundary).
- **Strict condensed output schema per worker** — prevents synthesis-time context/cost blowup (Anthropic's own key fix).

## LangGraph implementation blueprint

- **Topology:** a **hand-rolled tool-calling supervisor node on `StateGraph`** (current LangGraph docs steer away from the `langgraph-supervisor` prebuilt toward this — verify against docs at build time; it's also one fewer dependency and a cleaner "we built the router" judge story); the fan-out leg uses the **`Send` API** inside a node for runtime-decided parallel dispatch (heterogeneous targets supported). Workers are `create_agent` subgraphs.
- **State:** single shared schema; `trip_criteria` as its own Pydantic field (interview writes, others read); parallel worker results merge via per-key reducers or per-worker keys merged in one node. No hiding state in chat history.
- **Streaming (feeds the AgentTrace UI):** `stream_mode=["messages","custom"]` **with `subgraphs=True`** (else worker tokens are silently swallowed — documented pitfall); `custom` channel via `get_stream_writer()` emits `agent_started/agent_done/spawned-on-miss` lifecycle events; UI keys events by node id (no cross-branch ordering guarantee). Backend: async clients only (`astream` — sync calls block FastAPI's loop); SSE with `X-Accel-Buffering: no`. **Deliberate guardrail:** we stream only whitelisted channels — never `stream_mode="values"`, which forwards full graph state including private scratch fields to the client (state-leak risk).
- **Guardrails in-graph:** per-worker `bind_tools` allowlists · `response_format=<PydanticModel>` structured-output validation with auto-retry (note: node `RetryPolicy` does NOT catch Pydantic ValidationError — open bug #6027, use the response_format path) · spawn/step counter in state short-circuiting before the recursion limit (default 25) · `interrupt()` gate after the interview for criteria confirmation · per-session token budget (documented $47K runaway-loop incident is the cautionary tale).
- **Models:** different ChatModel per node — cheap/fast for interview + supervisor routing, stronger model for the planner/assembler; `nostream` tag on internal routing calls so they don't leak into the UI stream. **Observability:** LangSmith via env vars only (zero code).

## Top risks & mitigations

1. **Verification gap** (MAST cat. 3) → critic pass on Safety/Weather before timeline entry; deliberate failure-injection tests in the eval harness.
2. **Silent defaults** → assumption banners; eval includes ambiguous-input cases.
3. **Cost/latency blowup at synthesis** → condensed schemas, spawn caps, per-session budget, `output_mode="last_message"`.

## Demo-day story (one breath)
"A capped interview builds your trip criteria; a supervisor spawns only the specialists your question needs — watch them run live in the trace; a web-search agent appears only when our curated KB misses; safety and weather outputs pass a verifier before they can touch your timeline; and every card tells you where it came from."
