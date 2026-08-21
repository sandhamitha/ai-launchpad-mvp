# Mentor Guide — Swift App + LangChain Backend with Streaming Generative UI

_Received from mentor 2026-08-21 (follow-up to the 2026-08-17 sync's promised technical guide). Filed verbatim below. Resolution note at top._

## ✅ What this settles for us
- **Transport decision RESOLVED (was open flag on decision `2026-08-17-…-3`):** default **SSE** for one-way token streaming; **WebSocket only** where we need interrupts mid-generation, human-in-the-loop pauses, or persistent per-chat sessions. Rule: start SSE, upgrade specific screens only.
- **Generative UI protocol confirmed:** stream **structured JSON chunks** (`{"type":"card", …}` = component type + props); backend decides WHAT, client decides HOW via a **view registry**. This matches our OpenUI/F5 component-contract approach exactly — same pattern, transport-level.
- **Build order matches our walking skeleton:** (1) SSE endpoint streaming plain text → (2) client renders streamed text → (3) swap to JSON component chunks + registry → (4) WebSocket only where needed → (5) chat chrome → (6) reconnection/cancellation/error states.
- **LangGraph note:** use `astream_events` for token-level AND tool-call-level events (that's how the AgentTrace gets its live rows).
- Swift specifics (URLSession.bytes / URLSessionWebSocketTask / @Observable) apply to the post-PoC native exploration; the protocol + backend guidance applies NOW to the web PoC.

## Pitfalls he flagged (build checklist)
- Never block the async generator with sync LLM calls — `astream` / `astream_events` only.
- Use proper SSE framing (payloads can contain newlines) — no naive `data:` splitting.
- Parse JSON chunks incrementally — don't wait for full payloads ("live" feel depends on it).
- Don't default to WebSocket everywhere — session management (reconnects, heartbeats, state sync) is real cost.

---

## Full guide (verbatim)

### 1. Decide on Transport Protocol
- Default to **SSE (Server-Sent Events)** for one-way token streaming — simpler, stateless, cache-friendly, reconnectable.
- Use **WebSocket** only when you need bidirectional communication:
  - User needs to interrupt generation mid-stream.
  - Agent pauses for human input during a multi-step LangGraph flow.
  - You want a persistent session per chat instead of one request per turn.
- Rule of thumb: start with SSE, upgrade specific screens to WebSocket only where interactivity during generation is required.

### 2. Backend Setup (FastAPI + LangChain)
- Install: `pip install fastapi langchain uvicorn`.
- For SSE:
  - Create an async generator that calls `llm.astream()` or `chain.astream_log()`.
  - Yield each token as `data: <token>\n\n` inside a `StreamingResponse` with media type `text/event-stream`.
- For WebSocket:
  - Define a `@app.websocket("/chat")` route.
  - Use a custom callback handler (e.g. token callback) to forward each generated token to the socket as it's produced.
  - Keep the connection open per chat session so the client can send interrupts/follow-ups.
- Optional shortcut: use **Lanarky**, a FastAPI wrapper built specifically for LangChain streaming — provides ready-made SSE and WebSocket adapters so you skip writing the callback plumbing yourself.
- If using LangGraph agents with tool calls, stream via `astream_events` so you get token-level AND tool-call-level events, not just final text.

### 3. Swift Client Setup (No Third-Party Library Needed)
- For SSE:
  - Use `URLSession.bytes(for:)` — returns an `AsyncSequence` of bytes.
  - Iterate with `for try await line in bytes.lines`, parse lines starting with `data:`, and decode each chunk.
- For WebSocket:
  - Use `URLSessionWebSocketTask` (built into Foundation, no Starscream/SocketRocket required).
  - Call `send`/`receive` with async/await inside a loop to process incoming frames.
- Push every incoming chunk into an `@Observable` (or `@Published`) view model so SwiftUI re-renders incrementally as data arrives.
- This mirrors how Apple's own `FoundationModels.streamResponse` API streams partially-generated results into SwiftUI-bound state.

### 4. Designing the Generative UI Protocol
- Don't stream raw text only — stream **structured JSON chunks** describing UI intent, e.g.:
  ```json
  {"type": "card", "title": "...", "body": "..."}
  ```
- Backend decides *what* to render (component type + props); Swift client decides *how* to render it.
- Build a small **view registry** in Swift: a switch/dictionary mapping component type strings to SwiftUI views.
- Decode incrementally as chunks arrive rather than waiting for the full JSON payload — keeps the UI feeling live.

### 5. Generative UI Rendering Options (Pick Based on Effort vs Control)
- **Custom JSON/XML protocol + SwiftUI view registry** — full control, mirrors Vercel AI SDK's RSC pattern; medium effort.
- **LLMStream (Swift package)** — renders streamed Markdown + LaTeX in a WebView; low effort, good if output is rich text/formulas rather than custom widgets.
- **GetStream `stream-chat-swift-ai`** — prebuilt SwiftUI AI chat components (supports OpenAI, Gemini, Anthropic, or custom backends); fastest way to ship polished chat UI, less flexible for fully custom widgets.
- **Apple FoundationModels + `@Generable`** — on-device guided generation into typed Swift structs, streamed field-by-field; zero network latency but limited to on-device model capability and Apple Intelligence-supported hardware.

### 6. Suggested Build Order for a Mentee
1. Stand up FastAPI backend with a single SSE endpoint streaming plain text from a basic LangChain chain.
2. Build the Swift client using `URLSession.bytes(for:)` to render streamed text in a simple SwiftUI `Text` view — confirms the pipe works end to end.
3. Swap plain text for structured JSON chunks (component type + props) and build the SwiftUI view registry to render them.
4. Add WebSocket support only for the screen(s) that need interrupts or human-in-the-loop agent steps.
5. Layer in a prebuilt chat UI kit (e.g. GetStream) for chrome (bubbles, avatars, typing indicators) once the core streaming and rendering logic is proven.
6. Optimize: add reconnection logic for SSE, add cancellation for WebSocket, and add loading/error states for both.

### 7. Common Pitfalls to Flag for a Mentee
- Don't block the async generator in FastAPI with synchronous LLM calls — always use the async streaming methods (`astream`, `astream_events`).
- Don't parse SSE `data:` lines with naive string splitting if payloads can contain newlines — use proper SSE framing.
- Don't render generative UI components by waiting for the full JSON object unless payloads are small — partial/incremental parsing is what makes it feel "live."
- Don't default to WebSocket everywhere "just in case" — it adds session-management complexity (reconnects, heartbeat, state sync) that SSE avoids.
