# AI Launchpad MVP — Active Context

_Rewritten each session. ~300 words max. Start here._

## What this project is
Selected for the **AI Launchpad** program — build an MVP and present it to a large audience. This vault is the shared research memory for that build, co-maintained with my mentor (separate private repo, mentor has read/write).

## Current status (2026-07-26)
- **Infrastructure done.** Standalone Obsidian vault + private repo `github.com/sandhamitha/ai-launchpad-mvp`, isolated from all other work.
- **Auto-sync live:** launchd agent auto-pulls every 10 min + at login (pull-only, safe). Mentor's edits arrive automatically.
- **Dedicated `/save-mvp` command:** saves memory to this vault's `wiki/` and pushes to THIS repo only — never to second-brain.
- **Deletion guard active** on the repo (fails any push removing >5 files).
- **MVP scope: still undefined.**

## Next steps
- [ ] Invite mentor as GitHub collaborator (**need their GitHub username/email**)
- [ ] Define MVP problem statement + target user
- [ ] Lock MVP scope (in / out for the launch demo)
- [ ] Set build timeline against the Launchpad presentation date
- [ ] First mentor sync → capture in `meetings/`

## Key setup facts / gotchas
- Commit/push from **desktop only**; keep mobile Obsidian **pull-only** (mobile auto-commit once wiped the other repo).
- To save a session: run **`/save-mvp`** (not `/save-session` — that targets second-brain).
- Sync files/paths: launchd plist `~/Library/LaunchAgents/com.saddy.ailaunchpad-pull.plist`; log at `~/Library/Application Support/ailaunchpad-sync/pull.log`.

## Open questions for mentor
- (add here)
