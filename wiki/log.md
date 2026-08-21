# AI Launchpad MVP — Session Log

_Append-only. Newest entries at top._

---

## 2026-08-21 — Mentor's streaming guide filed; transport resolved (SSE-first)
- Received + filed mentor's technical guide → `wiki/mentor-streaming-guide.md` (verbatim + "what this settles" header).
- **Transport flag RESOLVED:** SSE by default; WebSocket only for interrupts / human-in-the-loop / persistent sessions. Noted on issue #4.
- **Generative UI protocol confirmed:** structured JSON chunks (component type + props) + client view registry — independently matches our F5/OpenUI contract. `astream_events` feeds AgentTrace rows.
- Build order locked per guide: plain-text SSE pipe → JSON chunks + registry → WebSocket only where needed → chrome → reconnection/error states.
- ⚠️ **Ground-truth re-sync pending:** status of #18 prototype (was due Aug 18 eve), any Aug 18–20 build progress, whether today's PoC review (Aug 21) is happening, and blockers (Anthropic key, Docker) — user hasn't confirmed yet.

**Next:** answer the 4 re-sync questions → re-plan today → build (#18 prototype if unshipped, else S1 SSE pipe per guide).

---

## 2026-08-18 — Logged 2026-08-17 mentor sync (Prototype & UX Alignment)
- Filed transcript + summary → `transcripts/2026-08-17-prototype-ux-alignment-*`; meeting record → `meetings/`; **4 decisions** → `decisions/` (mobile-first UI · landing↔canvas split · FastAPI+WebSockets streaming-first · timeline re-cut).
- Vision validated by mentor (hidden gems, provenance UI, persona, guide validation, multi-agent, streaming).
- **New timeline:** clickable prototype **Aug 18 eve** → design final **Aug 19** → PoC review **Fri Aug 21** → presentation **Aug 29**; daily 15–20 min calls; parallel UI ∥ backend workstreams.
- Flags: mobile-first supersedes desktop wireframes (v4 pass needed) · "canvas" is a new surface to scope · WebSockets vs SSE open · Swift = post-PoC exploration only · Fireflies "April" = August.

**Next:** build the clickable index-HTML prototype TODAY (mentor template incoming) · update board dates/tickets · then streaming skeleton (S1).

---

## 2026-08-16 (later) — Wireframes locked (#3 closed) + Claude Design import
- Iterated wireframes v1→v3.1 with user: desktop-first, **inline collapsible AgentTrace** (Cloudflare/LangGraph pattern — auto-open while running, collapses to one-liner), hero empty state w/ suggestion tiles + resume cards, both control mechanics (editable constraint chips + pin/👍/👎), OpenUI component contract (F5). Committed; **#3 closed** with full summary.
- User produced a polished hand-drawn wireframe set in **Claude Design** ("Weli AI travel companion" project); imported via DesignSync → `wireframes/weli-ai-wireframes.dc.html` + `support.js`, pushed, linked on #3 with viewing instructions.
- **Folded wireframe details into spec + tickets** (S2/#5: "keep my spelling" option, web-vs-guide side-by-side, skeleton cards; S4/#7: ◷ re-check control, safety copy rule — never "you'll be fine", no all-clear badge).
- **Name (#13) deferred** — "Weli AI" is a working title only; noted brand-tie consideration on the ticket.
- Studied OpenUI docs (Agent Interface SDK / OpenUI Lang / Cloud) — components composed from OUR library, props-only streaming.

**Next:** #15 landing page + waitlist (due Aug 17–19) · user's architecture session → apply audit's 5 technical fixes → build S1 · blockers: Anthropic key + Docker.

---

## 2026-08-16 — Idea-phase audit → findings + fix tickets; comms reply drafted
- Ran a deep adversarial audit (full: `wiki/risk-audit-2026-08-15.md` — 4 critical / 5 high / ~13 medium). Technical findings **parked** pending the architecture session.
- Idea-layer evaluation vs mentor's metrics → `wiki/idea-phase-findings.md`: strongest risks are story-layer — F1 "guide-reviewed" claim integrity, F2 no product name, F3 Visit-SL-vs-standalone narrative, F4 wedge asserted not demonstrated, F5 thin distribution evidence, F7 "Controllable" gap in V×U×C×R.
- Created fix tickets **#13–#17** (name, guide review, landing page + waitlist, Gemini side-by-side, controllability mechanic) — scheduled on the roadmap; #17 under PoC, rest under Pitch milestone.
- MCP direction locked earlier now committed in spec (§2 transport-agnostic registry + §4 pitch-week stretch, issue #12).
- Drafted the (late) reply to the AI Launch Pad comms email (project blurb + spam-folder apology) — user to attach headshot + send.

**Next:** wireframe review (#3) · name decision (#13) · landing page (#15) · user's architecture session (then apply audit's 5 technical fixes: code home, S1 split, KB 60–100 chunks, early smoke-eval, calendar re-cut) · blockers: Anthropic key + Docker.

---

## 2026-08-15 (later) — Steps 1+2 delivered, board + timeline wired
- Idea finalized (core: brainstorming/discovery, dynamic multi-agent, adaptive persona) — `wiki/idea-finalization.md`; refined via real interview findings (accuracy + timing; brainstorming-stage wedge).
- Spec written per mentor's exact feature-breakdown method — `specs/2026-08-15-poc-scope-design.md`; user-approved.
- Scope decision recorded — `decisions/2026-08-15-poc-scope-dynamic-multiagent.md` (supervisor + 3 core agents + web-search fallback on KB miss; safety = KB + real-time; graceful no-dead-end UX; provenance split).
- GitHub: #1 #2 closed w/ evidence; #3 In Progress; created milestone *PoC — Aug 22* + issues #4–#11; roadmap timeline (start/target dates) set. Token upgraded read:project → project.
- First weekly log written — `weekly-logs/2026-08-15.md`.

**Next:** wireframes (#3, Aug 15–16) → S1 skeleton. Build needs: Anthropic key + Docker running.

---

## 2026-08-15 — Resumed; read mentor's GitHub board
- Resumed after ~2wk pause. Target: PoC in ~1 week; Aug 29 pitch.
- Refreshed `gh` token with `read:project`; read mentor's board (Project #2): 3 issues — **Finalizing the idea**, **Feature breakdown**, **Wireframing**.
- His guidance = a pre-build discipline (validate idea → story-map features → wireframe). "Use your own creativity — just a perspective." Digested into `wiki/mentor-board-guidance.md`.
- Key move: reuse existing **Visit SL** research to fill his idea-checklist fast; use his feature-breakdown method to lock the PoC scope (the thing we paused on).
- **Plan NOT yet finalized** — dev paused by user until scope is locked.

**Next:** finalize PoC scope via story-map + MVP line; fill idea-checklist gaps; wireframe; Sunday mentor call.

---

## 2026-07-29 — Processed 2026-07-27 planning meeting (Fireflies)
- **MVP defined:** agentic travel companion for Sri Lanka (autonomous agents + SL-local RAG + generative UI). Deadline **Aug 29**, 50 participants.
- Filed Fireflies transcript + summary → `transcripts/`; wrote full meeting record → `meetings/2026-07-27-agentic-companion-planning.md`.
- Extracted **4 decisions** → `decisions/` (product, differentiation, UX, tech approach).
- Flagged heavy overlap with **Visit SL / Weli** — decide reuse vs fresh build.
- Note: Fireflies free plan blocks API/MCP → transcripts come via manual download + paste for now.

**Next:** feature mind map + wireframes for Saturday 9AM mentor review; set up GitHub project board.

---

## 2026-07-26 — Auto-sync + dedicated save command
- Installed a **launchd background auto-pull agent** (`com.saddy.ailaunchpad-pull`) that runs `git pull --ff-only --autostash` every 10 min + at login, so the mentor's edits flow in even when Obsidian is closed. Verified working (runs=2, exit 0).
- Created a **project-scoped `/save-mvp` slash command** — updates this vault's `wiki/log.md` + `hot.md`, then commits/pushes to THIS repo only. Never touches `~/Development/_memory` (second-brain). Confirmed the global `/save-session` would have mis-targeted second-brain, which is why this is separate.
- Confirmed the vault is a standalone Obsidian vault: open as a 2nd vault (both can be open at once); configure Obsidian Git inside it (desktop commit+push, mobile pull-only).

**Next:** invite mentor as GitHub collaborator (need their username); define MVP scope + first mentor sync.

---

## 2026-07-25 — Vault created
- Set up standalone shared research vault + private GitHub repo (`ai-launchpad-mvp`), isolated from all other work.
- Structure: `wiki/` (hot/log/index) + `meetings/`, `decisions/`, `experiments/`, `weekly-logs/`.
- Added deletion-guard workflow + sync rules (mobile pull-only) to prevent Obsidian sync accidents.
- Next: define MVP scope with mentor.
