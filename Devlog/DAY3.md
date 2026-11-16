# Day 3 — 14 November 2026 — integration day: merges, a lit L_Run, tests climbing to 47 (WIB)

> Built from `Devlog/entries/` of the day plus task reports and audits. Separate "compiled" from "tested"; list what nobody has verified. Still a record in progress, not a release announcement.

## Goal for today
Land the branches still open from Day 2 (052, 053, 016), fix the black L_Run view, then merge the logic tasks 014, 012, 031, 032, 013, 033 and 022 onto `main`, with a compile and automation pass after each step. Continue the asset lane (inventory import, Meshy regen) under the same honesty rules.

## Starting point
The HANDOFF said everything was merged except logic and integration, and that branches 016, 052 and 053 were still unmerged. Several executor sessions had hit usage limits, and one worker had left an unresolved TASK-032 merge on `main`.

## What shipped
- **Merges (coordinator, 05:53 to 05:56):** 052 themes and 053 elevated routes merged. The 053 merge had one text conflict in `JamHazardCatalogTest.cpp`, resolved by keeping both sides. The 016 UI merge was done by hand; an auto-merge had duplicated two controller members, which were removed. 26 worktrees were removed after WIP backup commits on their own branches; `asset/postmerge-prep` was left alone. All branches kept. Storage: the ~102 GB figure is a logical size, not a verified physical reclaim.
- **L_Run black view fixed (07:35):** sun mobility changed from Stationary to Movable, intensity 10 to 8, SkyLight set to Movable, fog added. A game launch at 540x960 showed lit buildings and a grey track: [`Devlog/media/20261114_lrun_lit.png`](media/20261114_lrun_lit.png). This fixes the black view, not the scene. A later owner screenshot still showed oversized ramps, building footprints and missing characters as rework items.
- **TASK-014 difficulty curve and enemy dodge (08:05):** `FJamDifficultyCurve` drives difficulty, speed scale (hard cap 1.7) and duel interval. Tests: 29 JamGame passing.
- **TASK-012 retake (09:15):** `FJamRetakeLedger` and `AJamRunGameMode::Retake()`: a caught run continues in place. Tests: 33 of 33 (4 new `JamGame.Retake.*`).
- **TASK-031 duel ambush and TASK-032 roadside pens:** written and reviewed by reading only, then merged and integrated at 11:03. The 031 merge had three text conflicts (`JamTypes.h`, `JamTrackManager.cpp`, `JamTrackSegment.h`), all pure additions, both sides kept. Tests: 39 of 39.
- **TASK-013 juice (11:30):** `Juice/` subsystem, time-effect mixer, light camera shake and haptics. Tests: 43 of 43.
- **TASK-033 data-first (19:10):** `UJamGameConfig`, `UJamRunnerTuning`, `UJamCameraModes`, `UJamCueStyle`, `UJamHordeTuning`; runner, horde, obstacle and duel tuning read from data assets with fallbacks. Asset script run twice: created, then all kept. Tests: 47 of 47.
- **TASK-022 skeletal wiring (12:25):** `FJamSingleNodePlayer` plays the base loop and one-shots on the hero and enemy skeletal meshes, with a fallback when a mesh is null. Tests: 47 of 47, including `JamGame.Presentation.SingleNodeFallback`.
- **Assets:** TASK-049 source refresh (five CSV rows updated; CRLF line endings restored after a line-ending slip). Batch 04 imported 12 of 12 candidates into Unreal as `importedCandidateUnverified`; 558 of 558 registry records loaded. Batch 05 aborted in preflight on a bad destination name for HORSE_01, so nothing from it was imported. Four ANIM_05 records came back `errorpartial` with icosphere placeholder meshes. The coordinator noted they were replaced, but no entry recording that replacement was found in `entries/`.
- **Meshy regen (11:50):** hero and zombie rigged; zombie library clips saved; a bone-name check across five FBX files gave 24 bones with identical names. The owner's tally is 114 credits: rigs plus 23 clips, including 5 text-to-motion clips with logged prompts. The entry itself records 37 credits spent at the time it was written, before the owner's later run.

Test count progression (JamGame automation): 27 (after 052, 053, 016) → 29 (014) → 33 (012) → 39 (031 + 032) → 43 (013) → 47 (022, 033).

## What broke and how we fixed it
- Executor sessions hit usage limits mid-task (TASK-016 and others). Resumed from WIP commits.
- The TASK-032 merge left six unresolved files on `main` (`MERGE_HEAD 485b603`). Audited and resolved before the 031 integration.
- A hand re-application of 031 dropped the final closing brace of `JamTrackSegment.cpp`, causing a cascade of Canvas errors in the next build. Fixed in the same session; green on the third build.
- TASK-053 failed its first headless run: `JamGame.Hazard.Traffic` pins the default `ApproachStartCm` at 6000, while the packet asked for 2400. Default reverted; re-run green (26 tests).
- Duplicate `TestEqual` overloads with a tolerance broke the 052 test build; replaced with `IsNearlyEqual`.
- A `TSubclassOf` in a test needed the full class type; added the include.
- L_Run lighting: a positional `FRotator` construction made the directional light point upward (pitch +45, roll -45). Fixed with named rotator arguments in the source helper and a saved-level correction.
- A Python helper failed because the commandlet consumed backslashes in the script path; fixed with forward slashes. An overbroad process kill was also noted.
- Meshy: the first regen run read an old 2-credit key from the process environment. The script now reads only the user registry key and aborts below a balance threshold. Hero stumble action 674 was rejected by the API ("no longer available"), 0 credits spent; it was replaced with 562.
- Interchange GLB import ignored the forced skeleton and created per-clip skeletons; the script deleted those products (its own assets only).
- `JamPlayerController.h` had duplicated members after an auto-merge; one of each was removed.

## Human and orchestrator corrections
- Root (orchestrator): re-prioritised to viewport-first on 14 Nov after a UI-authoring step ran before the saved world preview was accepted. Root acknowledged the priority error, issued TASK-046 viewport-first, and paused UI trials and PIE until a saved lit composition is proven.
- Root: revalidated `main` at 09:28 and moved all new work to `main` without worktrees, after an earlier engine worker hit a usage limit.
- Owner: took over storage cleanup and stopped computer use with Escape. Manual selection of old crane frames to keep is still pending the owner and root.

## How AI was used
| Tool / model | Used for | What needed human correction |
|---|---|---|
| Claude Haiku executors (Claude Code) | Logic tasks 014, 012, 031, 032, 013, 033, 022, and 052, 053, 016 in worktrees | Coordinator merges, conflict resolution, resumes after limits |
| Claude orchestrator | Audits (016, 053), merges, build and test runs, L_Run lighting script | Root priority error (UI before lit scene) |
| Codex GPT-6 (Terra) | Asset helpers, TASK-016 UI layout helper, mechanical motion audit | Engine execution moved to Luna's lane |
| Codex GPT-6 (Luna) | Native Unreal import batches 01 to 05; registry snapshots | Batch 05 preflight abort; ledger rows appended by the runner |
| Meshy REST API (stdlib Python script) | Rigs and animations for hero and zombie | Owner runs paid steps; key source fixed |
| Blender 5.1 CLI | Free bone-name check | none |

## Prompt of the day
No verbatim prompt is recorded in the entries for today. The case that matters: the TASK-053 (elevated routes) packet asked for a default `ApproachStartCm` of 2400, but the existing traffic test pins 6000. The first headless run failed. The fix kept the existing default and flagged the conflict to the orchestrator. Lesson for the next packet: state the conflicting default in the packet itself, so the executor does not have to choose between the packet and a passing test.

## Screenshots / clips
- `Devlog/media/20261114_lrun_lit.png`: L_Run after the lighting fix, game launch 540x960. Placeholder characters and an overlapping fallback menu are still visible.
- `Devlog/media/20261114_task022_hero_1.png`, `Devlog/media/20261114_task022_hero_2.png`: TASK-022 hero skeletal wiring.

## Not verified (14 Nov)
- No PIE, and no Android or Windows device run, for any logic task above. "47 of 47" is an automation count, not a play test.
- TASKs 014, 031, 032 and 022: compile and tests only; not played in the editor.
- L_Run: the lit scene is a screenshot. Buildings, props and characters are still placeholders or rework items, as the owner's screenshot shows.
- Asset imports: `importedCandidateUnverified` means imported and not yet checked. Reopen and persistence checks are not done.
- Meshy: hero stumble 562, library extras and text-to-motion apply steps are pending the owner's run. Whether the clips loop is not verified.
- Mechanical sequence import (TASK-046) was prepared offline only; live playback is not verified.

## Cut from scope (and why)
- UI authoring paused on 14 Nov until the lit world is proven: viewport first.
- HORSE_01 import batch 05 not retried; the frozen manifest needs an orchestrator decision on the destination rule.
- ANIM_05 retry blocked until the four placeholder-mesh sources are re-exported.

## Entries (14 Nov)
Integration: [coordinator merge cleanup](entries/20261114-0553_coord_merge-cleanup.md), [UI merge](entries/20261114-0556_coord_ui-merge.md), [L_Run build and lighting](entries/20261114-0735_coord_build-lrun-lighting.md), [viewport priority](entries/20261114-0445_coord_viewport-priority.md), [inventory first](entries/20261114-0933_coord_inventory-first.md), [integrate 031 + 032](entries/20261114-1103_logic_integrate-031-032.md).
Logic: [052 themes](entries/20261114-0042_exec_task-052-themes.md), [audit 016](entries/20261114-0047_audit_task-016.md), [053 elevated routes](entries/20261114-0100_exec_task-053-elevated-routes.md), [audit 053](entries/20261114-0127_audit_task-053.md), [014 difficulty and dodge](entries/20261114-0805_logic_task014-difficulty-dodge.md), [032 roadside pens](entries/20261114-0848_logic_task032-roadside-pens.md), [012 retake](entries/20261114-0915_logic_task012-retake.md), [031 duel ambush](entries/20261114-1015_logic_task031-duel-ambush.md), [013 juice](entries/20261114-1130_logic_task013-juice.md), [033 data-first](entries/20261114-1910_logic_task033-data-first.md), [022 skeletal wiring](entries/20261114-1225_assets_task022-skeletal-wiring.md).
Assets: [CSV refresh 049](entries/20261114-0002_metadata_task049-source-refresh.md), [batches 04 and 05](entries/20261114-1131_assets_046-batches-04-05.md), [Meshy regen](entries/20261114-1150_assets_meshy-regen.md), [Blender cleanup](entries/20261114-0425_luna_blender-cleanup.md), [storage](entries/20261114-0431_coord_storage-and-part3.md).
Frontend and environment: [UI layout helper](entries/20261114-0015_terra_ui-layout.md), [black PIE audit](entries/20261114-frontend_black-pie-audit.md), [L_Run authoring guard](entries/20261114-environment_lrun-authoring-guard.md).

## Plan for tomorrow
- Run the full JamGame automation set on the merged `main`, then a PIE pass on L_Run.
- Replace the four ANIM_05 placeholder sources; decide the HORSE_01 destination rule.
- Owner runs the pending Meshy steps (hero 562, extras, text-to-motion apply).
- Android build and device test of the merged logic before any "playable" claim.
