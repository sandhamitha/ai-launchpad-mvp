# AI Launchpad MVP — Shared Research Memory

This is a **shared, isolated knowledge vault** for the AI Launchpad MVP research project. It is a self-contained Obsidian vault backed by its own private GitHub repo, kept deliberately separate from any other work.

It exists so my mentor and I have **one source of truth** for how the work is progressing: context, decisions, meeting notes, experiments, and weekly status.

## Who uses this
- **Sandhamitha (me)** — maintains active context and logs, runs experiments, records decisions.
- **Mentor** — reads progress, leaves feedback/notes, adds guidance. Full read/write via GitHub collaborator access.

## How it's organized

| Folder | What goes here |
|--------|----------------|
| `wiki/hot.md` | **Start here.** Current active context — what I'm working on right now, blockers, next steps. Rewritten as things change (~300 words). |
| `wiki/log.md` | Append-only session history, newest at top. What happened, when. |
| `wiki/index.md` | Index of all wiki/topic pages. |
| `meetings/` | One note per mentor sync — agenda, notes, action items. |
| `decisions/` | Key decisions + *why* (so we never re-litigate). One file per decision. |
| `experiments/` | Research runs — hypothesis, setup, result, takeaway. |
| `weekly-logs/` | Weekly progress summaries for the mentor. |

## How to use it (mentor — quick start)

1. Install [Obsidian](https://obsidian.md) (free).
2. Clone this repo: `git clone <repo-url>` (or use the Obsidian Git plugin's "Clone" command).
3. In Obsidian → **Open folder as vault** → point it at the cloned folder.
4. (Optional) Install the **Obsidian Git** community plugin to auto-pull/commit/push.
5. Read `wiki/hot.md` first, then browse `weekly-logs/` and `meetings/`.
6. To leave feedback: add a note anywhere (e.g. in the relevant `meetings/` file or a `decisions/` note), then commit & push (or let Obsidian Git sync).

## Syncing rules (learned the hard way — please follow)

- **On mobile (iPad/phone), keep Obsidian Git PULL-ONLY.** Do **not** enable auto-commit/auto-push on a phone or tablet — mobile can commit a partial copy of the vault and delete everyone's files. Commit/push only from a computer with the full vault.
- A **deletion-guard** GitHub Action watches this repo and fails loudly if any push deletes more than 5 files — so accidental wipes get caught in ~1 minute. If you get a red ❌ email titled "Deletion Guard," ping me before force-fixing.
- If Obsidian shows a **merge conflict**, don't panic — everything is in git history. Pull, resolve, or ask me.

## Recovery
Nothing is ever truly lost — every version is in git history. To recover a deleted file:
`git checkout <commit>^ -- path/to/file`
