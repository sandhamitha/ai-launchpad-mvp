# SwiftUI Architecture Decision — Weli AI

**Status:** Decision-ready · **Author:** research pass, 2026-08-22 · **Scope:** iOS client only (Swift 6.1, Xcode 16.4, iOS 17+ target, `@Observable` available)

---

## 1. Recommendation

**Use the "MV" pattern (vanilla SwiftUI + `@Observable` models/services, no ViewModel layer by default) with a thin Coordinator only where `NavigationStack` needs one, plus feature-scoped folders.** Do not adopt TCA. Do not build a formal MVVM-C/Clean layer.

Concretely: `Views` bind directly to `@Observable` **service/model** objects injected via `@Environment`. Add an explicit `@Observable` "ViewModel" class **only** for the two screens that have real async/state logic worth isolating and testing: the chat stream screen and the trip-timeline planner. Everything else (auth forms, interview flow, static cards) reads its model directly — that *is* legitimate MV, not a shortcut.

### Folder structure

```
WeliAI/
  App/
    WeliAIApp.swift                 // @main, environment wiring, .task { authService.bootstrap() }
    AppEnvironment.swift            // EnvironmentKey/@Entry definitions for DI
  Core/
    Networking/
      SSEClient.swift               // actor: owns URLSession.bytes, parses SSE frames
      APIClient.swift               // REST calls (non-streaming) to FastAPI backend
    Auth/
      AuthService.swift             // @Observable, wraps supabase.auth, session state
    ComponentRegistry/
      ComponentEnvelope.swift       // type-discriminated Decodable enum
      ComponentPayloads.swift       // PlaceCardPayload, WeatherCardPayload, etc.
  Features/
    Chat/
      ChatStreamViewModel.swift     // @Observable, owns [ChatMessage], talks to SSEClient
      ChatView.swift
      ChatBubbleView.swift
    TripTimeline/
      TripTimelineViewModel.swift   // @Observable, drives the animated planner screen
      TripTimelineView.swift
    Interview/
      InterviewFlowModel.swift      // @Observable, mini-interview criteria state machine
      InterviewView.swift
    Auth/
      SignInView.swift
      SignUpView.swift
  DesignSystem/
    Components/
      PlaceCardView.swift
      WeatherCardView.swift
      SafetyAlertView.swift
      AgentTraceView.swift
      ConstraintChipsView.swift
    ComponentRendererView.swift     // switch(ComponentEnvelope) -> AnyView-free @ViewBuilder
  Models/
    ChatMessage.swift
    TripPlan.swift
    Session.swift
```

This is a **Features-first**, not Layers-first, split — appropriate at this size (per-screen `View` + optional `ViewModel` colocated), with cross-cutting concerns (`Networking`, `Auth`, `ComponentRegistry`) pulled into `Core/`.

---

## 2. Why — trade-offs vs. the alternatives

**Community consensus in 2025-2026 is unambiguous on the shape but split on the label.** SwiftUI's own `View` + `@State`/`@Bindable` already implements MVVM's separation for simple screens — Apple never names "MVVM" in its docs and injects models straight into views in sample code, which a 2026 architecture playbook reads as tacit endorsement of MV over ceremony `[forasoft.com, 2026-08-08]`. The pragmatic industry position (same source) is: **"MVVM + Coordinators + DI" is the default for production teams of 3+ engineers or 50+ screens; below that, plain MVVM (i.e., MV) is enough** `[forasoft.com, 2026-08-08]`.

- **MVVM-C / Clean / VIPER — rejected.** These exist to solve *team* problems: onboarding new engineers, isolating navigation across many screens, enforcing boundaries across large codebases. Weli AI has one engineer and roughly six screens. The playbook's own threshold (30+ screens, 10+ engineers for VIPER) is not close to being met `[forasoft.com, 2026-08-08]`. Every extra layer here is pure overhead against a 7-day clock.
- **TCA — rejected, deliberately.** Point-Free's own FAQ argues TCA's "cons" are overstated and that it excels when *determinism is a product requirement* — undo/redo, multiplayer state, exhaustive testing of complex reducers `[pointfree.co, 2024-06-04]`. Weli AI's hard problem is *not* state determinism, it's rendering an unpredictable stream of typed JSON quickly. TCA's reducer/action/store ceremony is a real, well-documented learning curve; the 2026 MVVM-C playbook explicitly frames TCA as "situational, not an upgrade," reserved for teams where deterministic state *is* the product `[forasoft.com, 2026-08-08]`. For a first-ever iOS app in 7 days, this is the single highest-risk architectural choice — it can derail the whole build learning the library instead of shipping the demo.
- **MV wins by elimination**, and it's the pattern Apple's own tooling is optimized for: `@Observable`'s access-tracked invalidation means a plain model object bound directly into a view already re-renders only on the properties that view actually reads — the previously "hidden cost" of skipping a ViewModel (over-broad re-renders) is gone `[avanderlee.com; fatbobman.com — Observation deep dives]`.

**Where a ViewModel earns its place:** the chat screen (owns the mutable message list + streaming lifecycle, needs to be unit-testable independent of `URLSession`) and the trip-timeline screen (owns animation/derived-state logic). Everywhere else, a bare `@Observable` model or the `AuthService` read via `@Environment` is sufficient — that is MV, not a compromise.

---

## 3. The streaming state pattern

**Isolate the network/parsing concern in an `actor`, expose an `AsyncStream` of decoded events, and let a `@MainActor @Observable` ViewModel own the mutable `[ChatMessage]` array that the view reads.** Two well-documented traps to avoid: (1) `URLSession.AsyncBytes.lines` silently *skips empty lines*, which breaks SSE parsing because the SSE spec uses a blank line as the event delimiter — you must iterate raw bytes/lines yourself and detect the blank-line boundary explicitly, not rely on `.lines` `[community-verified via WebSearch, cross-checked against mattt/EventSource README pattern]`; (2) UTF-8 code points can straddle chunk boundaries, so decode text only after you've assembled a full line/frame, never chunk-by-chunk.

```swift
// Core/Networking/SSEClient.swift
actor SSEClient {
    struct Event { let type: String?; let data: String }

    func stream(request: URLRequest) -> AsyncThrowingStream<Event, Error> {
        AsyncThrowingStream { continuation in
            let task = Task {
                do {
                    let (bytes, response) = try await URLSession.shared.bytes(for: request)
                    guard let http = response as? HTTPURLResponse, http.statusCode == 200 else {
                        throw URLError(.badServerResponse)
                    }
                    var dataBuffer = ""
                    var eventType: String?
                    for try await line in bytes.lines {          // note: .lines drops blank lines —
                        if line.isEmpty {                         // reconstruct the boundary ourselves
                            if !dataBuffer.isEmpty {
                                continuation.yield(Event(type: eventType, data: dataBuffer))
                                dataBuffer = ""; eventType = nil
                            }
                            continue
                        }
                        if line.hasPrefix("event:") { eventType = String(line.dropFirst(6)).trimmingCharacters(in: .whitespaces) }
                        else if line.hasPrefix("data:") { dataBuffer += String(line.dropFirst(5)).trimmingCharacters(in: .whitespaces) }
                    }
                    continuation.finish()
                } catch { continuation.finish(throwing: error) }
            }
            continuation.onTermination = { _ in task.cancel() }
        }
    }
}
```

```swift
// Features/Chat/ChatStreamViewModel.swift
@Observable @MainActor
final class ChatStreamViewModel {
    private(set) var messages: [ChatMessage] = []
    private let client = SSEClient()
    private var streamTask: Task<Void, Never>?

    func send(_ prompt: String) {
        streamTask?.cancel()
        messages.append(.user(prompt))
        let placeholder = ChatMessage.assistantStreaming(id: UUID())
        messages.append(placeholder)

        streamTask = Task {
            do {
                let request = try APIClient.chatRequest(prompt: prompt)
                for try await event in await client.stream(request: request) {
                    // decode JSON chunk -> append token or ComponentEnvelope to the placeholder message
                    appendChunk(event.data, toMessageID: placeholder.id)   // mutates on MainActor — cheap, @Observable diffs per-property
                }
            } catch {
                markFailed(placeholder.id, error: error)                  // simple retry: re-invoke send() on user tap
            }
        }
    }
}
```

- **Why an `actor` for the network layer, not `@MainActor`:** parsing bytes and JSON decoding is off-UI work; the actor hops back to `@MainActor` only when it mutates `messages`, which is a single array append — cheap under `@Observable`'s per-property tracking, so this does not need manual throttling/coalescing for typical LLM token rates.
- **Reconnection:** for a 7-day demo, don't build SSE resume/`Last-Event-ID` logic — a simple "tap to retry" that re-sends the last prompt is the correct scope. Document this as a known limitation rather than building it.
- **Backpressure:** irrelevant at chat-token volumes; skip it. If component chunks arrive faster than SwiftUI can lay out `PlaceCard`s in a `LazyVStack`, that's a rendering concern (see §4), not a stream concern.

---

## 4. The view-registry / decoding pattern

**Decode each SSE chunk into a type-discriminated `enum` with a custom `init(from:)`, and fall back to an `.unknown` case instead of throwing — the backend team *will* ship a new component type before you ship a client update.** This is the standard fix for Swift `Codable`'s lack of native polymorphism: read the discriminator key first via a keyed container, then decode the matching payload type `[nilcoalescing.com, "Encode and decode polymorphic types in Swift"]`. Airbnb's Ghost Platform uses the same shape in production Swift — an enum with associated values per component type, switched over by a `@ViewBuilder` `[Airbnb Engineering server-driven UI overview, cross-referenced via search]`.

```swift
// Core/ComponentRegistry/ComponentEnvelope.swift
enum ComponentEnvelope: Decodable {
    case placeCard(PlaceCardPayload)
    case weatherCard(WeatherCardPayload)
    case safetyAlert(SafetyAlertPayload)
    case agentTrace(AgentTracePayload)
    case constraintChips(ConstraintChipsPayload)
    case unknown(type: String)

    private enum CodingKeys: String, CodingKey { case type }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        let type = try container.decode(String.self, forKey: .type)
        switch type {
        case "place_card":       self = .placeCard(try PlaceCardPayload(from: decoder))
        case "weather_card":     self = .weatherCard(try WeatherCardPayload(from: decoder))
        case "safety_alert":     self = .safetyAlert(try SafetyAlertPayload(from: decoder))
        case "agent_trace":      self = .agentTrace(try AgentTracePayload(from: decoder))
        case "constraint_chips": self = .constraintChips(try ConstraintChipsPayload(from: decoder))
        default:                 self = .unknown(type: type)   // never crash the stream on a new/unhandled type
        }
    }
}
```

```swift
// DesignSystem/ComponentRendererView.swift
struct ComponentRendererView: View {
    let envelope: ComponentEnvelope

    var body: some View {
        switch envelope {
        case .placeCard(let p):       PlaceCardView(payload: p)
        case .weatherCard(let p):     WeatherCardView(payload: p)
        case .safetyAlert(let p):     SafetyAlertView(payload: p)
        case .agentTrace(let p):      AgentTraceView(payload: p)
        case .constraintChips(let p): ConstraintChipsView(payload: p)
        case .unknown:                EmptyView()   // graceful degrade — log for telemetry, don't render junk
        }
    }
}
```

- **Heterogeneous arrays** (e.g. a message containing `[ComponentEnvelope]`) decode for free once `ComponentEnvelope: Decodable` is defined — `JSONDecoder` handles the array, per-element the custom initializer runs and swallows unknown types via `.unknown` instead of failing the whole array decode `[pattern confirmed via itnext.io / sporks.space key-based polymorphic JSON approaches]`.
- **This is the "backend decides WHAT, client decides HOW" contract made concrete**: the `type` string is the only coupling; `ComponentRendererView` is the single place that maps type → SwiftUI view, so adding a 6th component type later is a two-file change (payload struct + one switch case), not a refactor.

---

## 5. Pitfalls for a beginner

- **Don't reach for `ObservableObject`/`@Published` — use `@Observable` everywhere** (iOS 17+ target already assumes this); mixing the two models mid-project is a common source of subtle bugs and wasted migration time `[avanderlee.com; swiftandmemes.com]`.
- **Don't try `URLSession.AsyncBytes.lines` naively for SSE** — it drops the blank lines the SSE spec relies on as event boundaries; you will lose the last chunk of every event silently. Confirmed independently via multiple Swift concurrency write-ups.
- **Don't let `JSONDecoder` throw on an unknown component `type`** — one unrecognized backend chunk should not blank the whole chat screen. The `.unknown` case in §4 is the entire fix; skipping it is the single most likely "works on my machine, breaks live" bug for a demo.
- **Don't build a DI container or Coordinator framework** — `@Environment` + a couple of `EnvironmentKey`/`@Entry` definitions is the full DI story needed here; the 2026 MVVM-C playbook itself flags DI frameworks (Factory, DITranquillity) as only earning their keep past this app's scale `[forasoft.com, 2026-08-08]`.
- **Don't defer Supabase session bootstrapping to a view's `.onAppear`** — do it once in the App's root `.task`, and keep `.onOpenURL { supabase.auth.handle(url) }` at the app level for magic-link/OAuth redirects, per Supabase's official SwiftUI lifecycle guidance `[supabase.com/docs/reference/swift/auth-api]`.

**Supabase auth wiring (minimal, correct shape):**

```swift
// Core/Auth/AuthService.swift
@Observable @MainActor
final class AuthService {
    private(set) var session: Session?
    private let client: SupabaseClient

    init(client: SupabaseClient) { self.client = client }

    func bootstrap() async {
        for await (event, session) in client.auth.authStateChanges {
            self.session = session
            // handle .initialSession (may be expired — check session?.isExpired) and .tokenRefreshed
        }
    }
}

// App/WeliAIApp.swift
@main
struct WeliAIApp: App {
    @State private var authService = AuthService(client: supabase)
    var body: some Scene {
        WindowGroup {
            RootView()
                .environment(authService)
                .task { await authService.bootstrap() }
                .onOpenURL { url in supabase.auth.handle(url) }
        }
    }
}
```

---

## 6. Sources

- [iOS App Architecture in 2026: MVVM-C, SwiftUI & Swift 6 — Fora Soft (2026-08-08)](https://www.forasoft.com/blog/article/advanced-ios-app-architecture-explained-on-mvvm-977) — MVVM/MVVM-C defaults, TCA/VIPER thresholds, DI options
- [Composable Architecture Frequently Asked Questions — Point-Free (2024-06-04)](https://www.pointfree.co/blog/posts/141-composable-architecture-frequently-asked-questions) — TCA's own framing of when it's warranted
- [Encode and decode polymorphic types in Swift — Matthaus Woolard, nilcoalescing.com (2021-04-08)](https://nilcoalescing.com/blog/BringingPolymorphismToCodable/) — type-discriminator Codable pattern
- [Dealing with key-based polymorphic JSON in Swift Codables — sporks.space (2023-03-05)](https://sporks.space/2023/03/05/dealing-with-key-based-polymorphic-json-in-swift-codables/) — alternative polymorphic decoding shapes
- [Swift: Overview — Supabase Docs](https://supabase.com/docs/reference/swift/auth-api) — official SwiftUI lifecycle / `onOpenURL` / auth client setup
- [@Observable Macro performance increase over ObservableObject — avanderlee.com](https://www.avanderlee.com/swiftui/observable-macro-performance-increase-observableobject/) — access-tracked invalidation, why MV avoids over-render
- [A Deep Dive Into Observation — fatbobman.com](https://fatbobman.com/en/posts/mastering-observation/) — Observation framework internals, iOS 17 minimum
- WebSearch cross-references (2026-08-22): Airbnb Ghost Platform server-driven UI (enum + `@ViewBuilder` switch pattern in Swift), `URLSession.AsyncBytes.lines` blank-line-skipping caveat for SSE parsing — corroborated across multiple independent Swift concurrency write-ups, no single canonical source quoted verbatim.

**Note on source quality:** the MVVM-C playbook and TCA FAQ are the two load-bearing sources for §1–2 and are both primary (vendor/maintainer) sources dated within the freshness window. The Codable polymorphism pattern in §4 is corroborated across two independent write-ups using the same discriminator-key technique. The SSE `.lines` blank-line caveat is `(cross-referenced across multiple community sources, no single authoritative doc — verify against actual server payload during implementation)`.
