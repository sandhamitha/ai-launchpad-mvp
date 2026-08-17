# Prototype Development & UX Alignment — Mentor Sync

**Date:** 2026-08-17, 18:32
**Participants:** Speaker 1 (likely Sandhamitha — builds prototype, monitors communities), Speaker 2 (likely Shazni — provides templates/tech guidance, architecture review). *Labels kept as in transcript.*
**Source:** `../transcripts/2026-08-17-prototype-ux-alignment-{transcript,summary}.md`
**Note:** the Fireflies summary says "April 19/29" — transcription artifact; context makes clear these are **August** dates.

---

## TL;DR
Post-gap mentor sync. Vision validated (hidden gems + multi-agent + human validation + provenance UI all praised). Course-corrections: **mobile-first UI** (not desktop), a **landing page / canvas app split**, **FastAPI + WebSockets streaming** prototyped early, and a compressed sprint: **design + clickable prototype by Aug 19, PoC review Friday (Aug 21), presentation Aug 29.** Native mobile (Swift/React Native) to be *explored*. Daily short calls + parallel workstreams (UI prototype ∥ streaming backend).

## Decisions (see `../decisions/`)
1. **Mobile-first UI** — sliders, minimal visuals, quick on-the-go interactions. ⚠️ *supersedes the desktop-first wireframe orientation.* → `2026-08-17-prototype-ux-alignment-1.md`
2. **Landing page ↔ main app (canvas) split** — anonymous landing captures queries w/ generic suggestions; personalized interactive canvas after engagement; maps/globe deferred. → `-2.md`
3. **Streaming backend = FastAPI + WebSockets, prototyped early** — start with simple chat UI ↔ Python backend. → `-3.md`
4. **Timeline re-cut** — design + clickable prototype **Aug 19** · prototype PoC review **Fri Aug 21** · sprint → presentation **Aug 29**; parallel workstreams + daily 15–20 min calls. → `-4.md`

## Key discussion points
- **Validated:** hidden-gems differentiation · conversational UI w/ generative elements + provenance ("clarify content provenance and context") · adaptive persona learning over repeat visits · human-in-the-loop guide validation (35-yr guide collaboration mentioned) · supervisor + sub-agent delegation incl. web-search fallback for out-of-scope/misspelled queries · streaming/incremental presentation.
- **KB freshness loop:** daily scan of SL travel Reddit/FB threads feeds new questions into KB/agent memory.
- **Canvas concept (new):** a central interactive trip-planning space (plan → view → update with live status/nudges: weather alerts, transport). Maps deferred for device compatibility.
- **Prototype method:** clickable skeleton (index HTML / Replit link), Claude Design for consistency — flow over polish.
- **Pitch guidance:** simplicity + clarity; naming matters for the mental image; avoid jargon-heavy diagrams.
- **Native mobile:** React Native / Swift exploration for location + real-time; Swift leaning (Mac env); blockwise from login page.

## Action items
**Speaker 1 (me)**
- [ ] Clickable **index HTML prototype** (interactive flow skeleton) — **by Aug 18 evening** ← URGENT
- [ ] Prototype **PoC for review by Friday (Aug 21)**
- [ ] Populate GitHub **milestones + start dates** *(already largely done — verify)*
- [ ] **Daily** Reddit/FB monitoring → KB/agent memory
- [ ] Review **Swift** options for streaming/mobile
- [ ] Sprint to presentation submission **Aug 29**

**Speaker 2 (mentor)**
- [ ] Basic clickable index-HTML template for inspiration — by Aug 18 evening
- [ ] Technical guide: real-time streaming (Swift + backend) — EOD
- [ ] Architecture review (UI/UX, agent workflow, cost-effectiveness)
- [ ] Schedule 15–20 min follow-up call

## ⚠️ Open flags
1. **Mobile-first vs locked desktop wireframes** — wireframes need a mobile-first pass (v4); the AgentTrace/provenance system carries over, orientation changes.
2. **Native mobile (Swift) scope risk** — exploration only; a Swift app by Aug 22 is not realistic alongside everything else; keep it post-PoC unless mentor insists.
3. **"Canvas" surface is new** — extends beyond chat; needs a wireframe frame + scope decision (thin version for PoC?).
4. **WebSockets vs SSE** — spec assumed SSE-style streaming; mentor said WebSockets. Decide at architecture session.
5. Speaker labeling ambiguity (guide-collaboration attributed to Speaker 2) — verify who said what if it matters later.
