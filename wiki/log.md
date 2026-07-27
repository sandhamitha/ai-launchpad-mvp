# AI Launchpad MVP — Session Log

_Append-only. Newest entries at top._

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
