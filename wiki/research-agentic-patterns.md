# LangGraph Multi-Agent Orchestration — Research & Recommendation for Weli AI

_Research date: 2026-08-22. Sources verified against current LangChain/LangGraph docs (`docs.langchain.com`, `reference.langchain.com`) — see Sources. This doc seeds the agentic-pattern deliverable owed to the mentor and is consistent with `agent-guardrails.md` and `hot.md`._

## 1. Recommendation

**Pattern: hand-rolled Supervisor (subagents-as-tools) built directly on `StateGraph`, using the `Send` API for dynamic parallel fan-out, with a Handoffs-style state machine for the mini-interview stage.** Do **not** use the prebuilt `langgraph-supervisor` package.

**Graph shape, one sentence:** `START → intake/interview stage-gate (interrupt-driven, capped turns) → supervisor node (LLM decides which of {discovery, weather, safety} to spawn) → Send-based parallel fan-out to selected specialists → join node checks KB-hit flags → conditional second Send to web-search only on KB miss → aggregator → planner node (schema-validated TripPlan) → END`, all streamed via `stream_events(version="v3")` over SSE.

```
START
  └─▶ interview_node  (interrupt() per turn, capped max_turns; writes TripCriteria)
        └─▶ supervisor_node  (LLM routing call → returns list[Send])
              ├─▶ Send("discovery_agent", ...)   ─┐
              ├─▶ Send("weather_agent", ...)      ├─▶ join_node (reducer merge)
              └─▶ Send("safety_agent", ...)      ─┘        │
                                                 kb_miss? ──┼── no ─▶ planner_node ─▶ END
                                                             └─ yes ─▶ Send("web_search_agent") ─▶ planner_node ─▶ END
```

**Top-3 reasons:** (1) it's what LangChain's own current docs recommend as of this fetch, (2) `Send` gives true 0..n dynamic parallel spawn with a native merge/reducer story, (3) it is the cheapest-to-build, easiest-to-explain-to-judges option that still satisfies every hard requirement in 5 days.

## 2. Why — vs alternatives

**Supervisor (direct, tool-calling) beats the prebuilt `langgraph-supervisor` library.** The official reference page for `langgraph-supervisor` now states: *"We now recommend using the supervisor pattern directly via tools rather than this library for most use cases. The tool-calling approach gives you more control over context engineering and is the recommended pattern in the LangChain multi-agent guide"* [reference.langchain.com/python/langgraph-supervisor, retrieved 2026-08-22]. The library isn't gone (still being kept 1.0-compatible) but it's now positioned as a compatibility shim, not the frontier pattern — using it in a "sophistication"-judged competition risks looking dated to judges who know the current docs.

**Supervisor beats Swarm for this system.** Swarm (peer-to-peer handoff, no central router, last-active-agent memory) trades routing accuracy for lower latency and is best when agents rarely misroute and latency is the proven bottleneck [focused.io supervisor-vs-swarm, retrieved 2026-08-22]. Weli AI's core judged deliverable is a **live agent-trace UI** — a single supervisor node gives one clean routing decision per turn that is trivial to explain and trivial to visualize ("the supervisor looked at your query and decided to spawn Discovery + Safety, skip Weather"). A swarm's decentralized hand-offs are harder to narrate to judges and harder to bound for cost.

**Subagents pattern (all results flow back through the coordinator) beats bare Handoffs for the specialist fan-out.** LangChain's own pattern-selection table shows Subagents costs +1 model call vs Handoffs/Skills/Router (4 vs 3 for a one-shot request), but *"this overhead provides centralized control"* [docs.langchain.com/oss/python/langchain/multi-agent, retrieved 2026-08-22]. That extra call buys exactly what a judged demo needs: one place (the supervisor) where dispatch is decided, one place where guardrails are enforced, one place where the "why 5 agents not 1" story lives.

**Handoffs pattern is still the right tool — but scoped only to the interview stage.** Handoffs are explicitly recommended when you need to *"enforce sequential constraints (unlock capabilities only after preconditions are met)"* and *"collect information in a specific sequence"* [docs.langchain.com/oss/python/langchain/multi-agent/handoffs, retrieved 2026-08-22] — this is precisely the capped mini-interview → planner gate. Using Handoffs there and Supervisor+Send for the specialist layer is not mixing patterns arbitrarily; it's applying each pattern to the sub-problem it's documented for.

**5-day / solo-dev fit:** no new package to learn (`langgraph-supervisor`'s own docs point you back to primitives you already need for `Send` and `interrupt`), one mental model (`StateGraph` + `Command` + `Send`) covers routing, handoff, and parallel fan-out.

## 3. Dynamic spawn + parallel merge — concrete API

The supervisor node is a normal LLM call (structured output, not free text) that returns which of `{discovery, weather, safety}` to run; a routing function turns that into a list of `Send` objects. Nodes returned by the same routing function execute **in the same superstep, i.e. in parallel** [docs.langchain.com/oss/python/langgraph/graph-api, retrieved 2026-08-22].

```python
from langgraph.types import Send
from langgraph.graph import add_messages
from typing import Annotated, TypedDict

class TripState(TypedDict):
    criteria: dict
    messages: Annotated[list, add_messages]
    agent_results: Annotated[list, operator.add]  # reducer merges parallel writes
    kb_miss: bool

def supervisor_route(state: TripState) -> list[Send]:
    decision = supervisor_llm.invoke(state)  # structured output: list[AgentName]
    return [Send(agent, {"criteria": state["criteria"]}) for agent in decision.agents]

graph.add_conditional_edges("supervisor_node", supervisor_route)
```

Each specialist writes into `agent_results` (an `Annotated[list, operator.add]` channel) — LangGraph safely merges concurrent writes via the reducer rather than raising a conflict [docs.langchain.com/oss/python/langgraph/graph-api, retrieved 2026-08-22]. The **web-search-on-KB-miss** requirement needs a *second* round: `join_node` reads `kb_miss` set by discovery/safety and routes conditionally — either straight to `planner_node`, or via one more `Send("web_search_agent", ...)`. This is a two-superstep fan-out, not a single one; budget for it, but don't over-engineer past it — a third phase is not needed for this scope.

Cost/depth caps for the whole graph: pass `recursion_limit` explicitly on every `invoke`/`stream` call. **LangGraph ≥1.0.6 changed the default recursion limit to 1000 steps** [docs.langchain.com/oss/python/langgraph/graph-api, retrieved 2026-08-22] — far too permissive for a judged demo; cap explicitly (e.g. 25–40) and use the `RemainingSteps` managed value inside routing functions for graceful degradation instead of a hard `GraphRecursionError`. Also set `max_concurrency` on `invoke`/`stream` to bound how many Send branches run at once.

## 4. Streaming events → SSE mapping (concrete)

**Use the current event-streaming API, not the older `astream_events(version="v2")` tutorials still circulating online.** The documented interface today is `graph.stream_events(input, config, version="v3")` (sync) / `await graph.astream_events(input, config, version="v3")` (async), which returns a run-stream object exposing **typed projections** rather than a flat event dict list [docs.langchain.com/oss/python/langgraph/event-streaming, retrieved 2026-08-22]:

| Projection | Use for SSE |
|---|---|
| `stream.messages` | token-level deltas → `event: token` |
| `stream.subgraphs` | per-agent start/finish, `graph_name` + `path`, with a `cause` linking back to the dispatching supervisor tool call → `event: agent_start` / `agent_end` (this is your live agent-trace UI, directly) |
| `stream.interrupts` / `stream.interrupted` | mini-interview pause → `event: awaiting_input` |
| `stream.output` | final validated `TripPlan` → `event: done` |

```python
from fastapi.responses import StreamingResponse
import asyncio, json

async def sse_gen(graph, input, config):
    stream = await graph.astream_events(input, config=config, version="v3")
    q = asyncio.Queue()

    async def pump_tokens():
        async for msg in stream.messages:
            await q.put(("token", {"node": msg.node, "text": msg.text}))
    async def pump_agents():
        async for sg in stream.subgraphs:
            await q.put(("agent", {"name": sg.graph_name, "path": sg.path}))

    asyncio.create_task(pump_tokens()); asyncio.create_task(pump_agents())
    while True:
        event, data = await q.get()
        yield f"event: {event}\ndata: {json.dumps(data)}\n\n"

# router: return StreamingResponse(sse_gen(...), media_type="text/event-stream")
```

Filter noise deliberately: only forward the four whitelisted event types above to the Swift client; drop raw `stream.values` (full state snapshots) from the client feed — see Pitfall 6 below on why that specifically matters here.

## 5. Guardrails enforcement map

This maps directly onto `agent-guardrails.md`'s checklist — location in the graph for each control:

| Guardrail | Enforced where |
|---|---|
| Per-agent tool allowlist | **Structural, at agent construction** — each specialist is built with only its own tools (`create_react_agent(tools=[weather_api])` etc.); no shared toolset, so allowlisting isn't a runtime check, it's a build-time impossibility for cross-tool calls |
| Schema-validated outputs | Supervisor's routing decision **and** the planner's final `TripPlan` both use `response_format=ToolStrategy[Schema]` / `ProviderStrategy` on `create_agent` — LangChain validates and returns the result in `structured_response`, auto-selecting native structured output when the model/provider supports it [docs.langchain.com/oss/python/langchain/structured-output, retrieved 2026-08-22] |
| Retrieved-content-as-data (injection defense) | **`join_node` / pre-merge step**, not the specialist agent itself: web-search and RAG chunks are summarized into structured fields (name/fact/source) before they re-enter `agent_results`; system prompts state retrieved content can never override instructions. This is general LLM-security practice (hook/tool-boundary enforcement, not prompt-only) — apply it where untrusted content crosses from tool output back into state `(unverified — general security guidance, not LangChain-specific API)` |
| Spawn-depth / cost caps | `recursion_limit` + `max_concurrency` on every `invoke`, `RemainingSteps` in routing functions, explicit `max_turns` counter in `TripState` checked by the interview routing function |
| KB-poisoning / no-write-access | Structural — no agent node has a KB-write tool; community content flows through the suggestion queue outside the graph entirely |

## 6. State / checkpointing for the interview flow

Use a **checkpointer keyed by `thread_id`** (the Swift session id): `InMemorySaver` for dev, `PostgresSaver` (`langgraph.checkpoint.postgres`) for anything that must survive a restart. Each interview turn calls `interrupt()` inside `interview_node`; LangGraph persists state and pauses [docs.langchain.com/oss/python/langgraph/interrupts, retrieved 2026-08-22]. The client resumes with `Command(resume=user_answer)` on the same `thread_id`; the interrupt payload and pause status surface cleanly via `stream.interrupts` / `stream.interrupted` on the v3 event stream, so no separate polling endpoint is needed.

```python
config = {"configurable": {"thread_id": session_id}}
stream = await graph.astream_events({"messages": [...]}, config=config, version="v3")
if stream.interrupted:
    # send stream.interrupts payload to client, await their answer, then:
    stream = await graph.astream_events(Command(resume=answer), config=config, version="v3")
```

**Critical footgun documented by LangChain itself:** `Command(update=...)` alone as input resumes from the *last checkpoint*, not `__start__` — if the graph already finished, it "appears stuck." To continue a normal (non-interrupted) conversation on an existing thread, pass a plain input dict; reserve `Command(resume=...)` strictly for actually-interrupted turns [docs.langchain.com/oss/python/langgraph/graph-api, retrieved 2026-08-22].

## 7. Pitfalls

- **Don't reach for `langgraph-supervisor`** assuming it's the current best-practice package — the reference docs now redirect you to the direct tool-calling pattern; using the deprecated package is a visible "used the old tutorial" tell to judges who check docs.
- **Don't mix old `astream_events(version="v2")` blog tutorials with v3** — the projection-based API (`stream.messages`, `stream.subgraphs`) is a different shape than the v2 flat-dict-of-events pattern most 2025 tutorials show; copying v2 code against a v3 install will silently misbehave.
- **Recursion-limit default is now 1000**, not a small safe number — set it explicitly per call or a runaway loop in the demo becomes very expensive very fast.
- **Reducer discipline on parallel writes** — every state key written by more than one `Send` branch needs an explicit reducer (`Annotated[list, operator.add]` or custom merge); an un-reduced shared key across parallel specialists will conflict or silently overwrite.
- **`stream_mode="values"` does not redact private state channels** — if the trace UI naively streams full state snapshots, internal scratch fields (raw retrieved docs, unfiltered safety notes) leak to the Swift client even though input/output schemas hide them from `invoke()`'s return value. This is a real guardrail leak specific to this system's live-trace requirement — only forward the whitelisted `messages`/`subgraphs`/`interrupts`/`output` projections, never raw `values`, over SSE.
- **Two-phase Send (specialists → conditional web-search) adds a full extra superstep** of latency; it's necessary for the KB-miss requirement but resist adding a third phase — scope discipline for 5 days.

## 8. Sources

- [LangGraph Supervisor — reference](https://reference.langchain.com/python/langgraph-supervisor) — official note recommending direct tool-calling supervisor over the library
- [Multi-agent — Docs by LangChain](https://docs.langchain.com/oss/python/langchain/multi-agent) — pattern catalog (Subagents/Handoffs/Skills/Router) + model-call cost table
- [Handoffs — Docs by LangChain](https://docs.langchain.com/oss/python/langchain/multi-agent/handoffs)
- [Build a personal assistant with subagents (supervisor tutorial)](https://docs.langchain.com/oss/python/langchain/supervisor)
- [Send — langgraph type reference](https://reference.langchain.com/python/langgraph/types/Send)
- [Graph API overview — Docs by LangChain](https://docs.langchain.com/oss/python/langgraph/graph-api) — Send, Command, recursion limit, reducers, private-channel streaming caveat
- [Event streaming — Docs by LangChain](https://docs.langchain.com/oss/python/langgraph/event-streaming) — `stream_events(version="v3")`, typed projections
- [Interrupts — Docs by LangChain](https://docs.langchain.com/oss/python/langgraph/interrupts) — `interrupt()`, `Command(resume=...)`, thread_id
- [Structured output — Docs by LangChain](https://docs.langchain.com/oss/python/langchain/structured-output) — `response_format`, `ToolStrategy`/`ProviderStrategy`
- [Multi-Agent Orchestration in LangGraph: Supervisor vs Swarm — Focused](https://focused.io/lab/multi-agent-orchestration-in-langgraph-supervisor-vs-swarm-tradeoffs-and-architecture)
- Guardrail/injection general practice (single-source, unverified against LangChain-specific docs): [LLM guardrails best practices — Datadog](https://www.datadoghq.com/blog/llm-guardrails-best-practices/), [AI security: prompt injection — Red Hat](https://www.redhat.com/en/blog/ai-security-defending-against-prompt-injection-and-unsafe-actions)
