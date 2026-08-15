# Mentor Board Guidance (GitHub Project #2)

_Source: mentor's AI LaunchPad Project board, read 2026-08-15. His note: "use your own creativity and drive it — this is just to give a different perspective." So: framework, not gospel. (The reference links appended to each item are auto-generated noise — ignore them; the body text is the substance.)_

The board has 3 issues, all **Todo** — a pre-build discipline: **finalize idea → break down features → wireframe.**

## Issue 1 — Finalizing the idea (validation gates BEFORE building)
- **Falsifiable hypothesis:** `[specific user] + [specific problem] + [evidence/workaround they use] + [cost of it]`. Vague = not ready.
- **Problem-model fit:** run real prompts on real *messy* SL data — can the model actually do it, or only on the clean demo case?
- **Agent-native fit:** name ONE bounded responsibility handed end-to-end to an agent. Test: *"if you removed the AI, is it just a form?"* If yes → not agent-native.
- **AI-native design equation:** `Visible × Understandable × Controllable × Recoverable = Used` (any missing factor = fails).
- **Demography:** specific user (age, device, income, **automation-trust level**), one-sentence value prop, the exact search query a user would type.
- **Regional fit:** lean on SL-specific infra/regulation/data as the moat — *"what still wins if a global competitor copies this tomorrow?"*
- **Distribution:** the first-20-users channel + a champion user (not "go viral").
- **Longevity:** unit economics at 10x (**per-inference cost** matters), weekly (not one-time) retention, build-vs-buy (buy commodity infra, build the differentiator).

## Issue 2 — Feature breakdown (idea → prioritized MVP)
1. **MECE feature list** from validated problem + research
2. **User Story Map:** activities → tasks → epics → stories; **draw the MVP line** (above = v1, below = later)
3. Write each as a **user story:** `As a [user], I want [goal], so that [value]`
4. **INVEST** check (Independent/Negotiable/Valuable/Estimable/Small/Testable); slice **vertically**
5. **Acceptance criteria** (Given/When/Then), ≥3 per story incl. an error path
6. **Prioritize:** MoSCoW or Value/Effort (best for no-data, deadline-bound); RICE/Kano later
7. **Walking skeleton:** thin end-to-end path first, then thicken (de-risks integration)
8. **Prototype** at lowest fidelity: sketch → interactive mockup → AI code prototype (v0/Bolt/Lovable/Replit)

## Issue 3 — Wireframing
Empty placeholder — he wants wireframes (the design step, before/with prototyping).

---

## How we apply this (given 1-week PoC deadline)
- **Timebox each gate** — do the lightweight version, don't skip.
- **Reuse Visit SL work:** audience, competitor landscape, regional edge, and the 15 recurring tourist questions are ALREADY researched — fill his idea-checklist from that instead of starting cold. Big time-saver.
- **His feature-breakdown method = how we finalize the PoC scope** (the thing we paused to decide). User-story-map the agentic companion → draw the MVP line → that IS the PoC.
- **Gaps to actually fill:** falsifiable hypothesis · problem-model-fit test on messy SL data · the ONE agent responsibility · first-20 distribution · unit economics at scale.
- **Sunday:** mentor call — bring the filled checklist + story map + wireframes.
