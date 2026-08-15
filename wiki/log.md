# AI Launchpad MVP — Session Log

_Append-only. Newest entries at top._

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
