# Dev Log (Glass Box)

This folder is the public record of how "CUT! Zombie Extras" was built with AI agents during the Epic AI MegaJam 2026. It is written for judges: honest, specific, in English. It is published on the itch.io game page before the deadline (the GitHub repo stays private, so the log must make sense on its own).

## Structure
| Path | What | Who writes it | When |
|---|---|---|---|
| `entries/` | One small file per finished piece of work (a task, an audit, an asset batch, a human test). Raw material | **Every agent and the human**, see the rule below | In the same commit as the work |
| `DAY1.md` ... `DAY4.md` | The readable daily story, built from the entries, reports and audits | Orchestrator or the dev-log lane (prompt `LOG-01`) | End of each day |
| `screenshots/` | `dayN_<what>.png` / short clips (Git LFS) | Human, or an agent with editor access | When something is visible |
| `ENTRY_TEMPLATE.md`, `DAY_TEMPLATE.md` | Templates | — | — |

## The rule (also in `CLAUDE.md` and `AGENTS.md`)
Every agent session that changes the repo adds **one entry file** before it reports back:

- Name: `Devlog/entries/YYYYMMDD-HHMM_<lane>_<short-slug>.md` (local time, WIB), for example `20261112-1905_exec_task-021-horde-reactions.md`. One file per session, never edit someone else's entry. A new file per session means parallel agents never conflict.
- Content: copy `ENTRY_TEMPLATE.md`. Ten lines is enough. English.
- It must be **true and specific**: which model/tool, what was asked, what came out, what failed, what a human or another agent had to correct, and what is still **not verified**. "Compiles" is not "tested"; say which one it is.
- Never include API keys, tokens, e-mail addresses, private links or provider perk codes.
- An audit does not pass a task whose branch has no entry. The orchestrator may add a missing entry itself and says so in the audit.

## What makes a good Glass Box log

### Owner's per-operation requirement

The Team Owner requires a record for **every editing and generation operation**, not merely a summary at the end. Start one uniquely owned entry per session, append a small operation ledger as work happens, and update submitted/pending jobs with their actual outcomes. Include prompt/model/tool, safe task IDs, input/output paths, human corrections, rework and the actual validation stage. Raw frame-render batches can be one operation with a manifest of frames; separate provider generation jobs still get separate rows/IDs. Never edit another agent's entry or copy secret values/signed download links. Coordinator backfills say that they are retrospective. A log edit does not recursively require a second log entry.

Preserve explicit OpenAI attribution where actually used: Codex/coordinator, worker model identifiers and built-in image generation. Do not infer an unreported underlying image model, claim a prize qualification was checked, or attribute Meshy/Rodin/Blender outputs to OpenAI's image model. The published narrative links actual concept/source/editor/device evidence with its correct label. Keep existing publication/submission under the human owner's control.
- Failures are content. A wrong guess, a rework, a rate limit, a build that only failed on Android: write them down with the fix.
- Name the human decisions (scope cuts, design choices, what was rejected from an AI proposal and why).
- One real prompt per day, with what had to change in it.
- Numbers when available: build times, sizes, frame times, how many reworks.
- No marketing voice, no claims that were not checked.
