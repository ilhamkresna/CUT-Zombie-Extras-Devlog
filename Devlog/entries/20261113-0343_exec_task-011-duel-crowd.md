# 2026-11-13 03:43 WIB — exec — TASK-011 duel crowd: loops, reactions, wings, mob (greybox)

- **Who / tool:** executor, Claude Haiku 5.5 in Claude Code (worktree subagent)
- **Task:** TASK-011 · **Branch / commit:** `task/011-duel-crowd` (from local `main`), commit hash in the git log of that branch
- **Asked for:** Duel crowd on the horde: per-member loops (Cheer / JumpCheer / Dance / Clap), reactions to duel hit, struck and KO, mob on a lost duel or run, side-rear wings, duel camera pull-back to the DESIGN_SPEC 5.1 values, `Jam.Crowd` switch.
- **Result:** `AJamHordeActor` gains the duel crowd: wings replace the TASK-007 half circle (two columns per side, |Y| 520-760, X -450..+150 from the runner, close tightening to -300), loops and one-shot reactions as greybox offsets on top of the formation slot, a 0.5 s wing return after the outro, and a mob pile with shake for MobTime. `AJamRunGameMode` binds the three duel delegates and waits MobTime before Game Over on the two lose paths (MobTime 0 = as before). Duel camera defaults set (arm 760, pitch -15, socket Z 280, FOV 72). `Jam.Crowd 0|1` disables all crowd visuals and the mob wait.
- **Verified by:** compile only. Editor `Build.bat JamGameEditor Win64 Development`: `Result: Succeeded`, 0 JamGame warnings (final code). Android `Build.bat JamGame Android Development` (clang 18): `Result: Succeeded` (final code). No PIE, no automation test.
- **What went wrong:** The first Android build failed on `-Werror,-Wunreachable-code-loop-increment` in `FindTrackManager`, an iterator loop that always breaks on the first pass. It was inherited from TASK-021 and only the Android compile showed it. Fixed by a single `TActorIterator` test. The first Android run was also the first time this worktree compiled for Android (long build). PowerShell `*>` logs were used and read directly.
- **Corrected by:** self (clang fix, re-run of both builds on the final code).
- **Human decision needed or taken:** (1) The duel camera blend is still 0.4 s; the blend tunable lives on `AJamDuelManager` (Duel/*, out of scope), so 0.5 s needs the owner. (2) Wing visibility at arm 760 / FOV 72 is doubtful for the rear wings (about 60 deg off axis); needs a PIE decision on the arm or FOV. (3) The run-catch path now also waits MobTime (interpreted from "caught in duel or in the run"). (4) `Jam.Crowd 0` keeps the duel formation at the run slots instead of the TASK-007 half circle.
- **Not done / not verified:** PIE of every crowd behaviour (loops, hit cheer, struck ooh, KO jump, wing return without pop, mob pile and shake, close-up, THAT'S A WRAP timing, `Jam.Crowd 0` vs 1 identical duel). Transition from run slots to wings may cross the corridor for a fraction of a second (steady state clean). Dance yaw is invisible on the cylinder greybox. `PlayCrowdReaction` takes a local enum, not `EJamAnim` (not in code yet, TASK-020). No TASK-022 mapping of the 4 pose groups yet.
- **Screenshot:** none

## Operation ledger
| # | Operation | Result |
|---|---|---|
| 1 | `git switch -c task/011-duel-crowd main` | created branch from local main |
| 2 | Read CLAUDE.md, AGENTS.md, TASK-011, AUDIT-021, AUDIT-009, ARCHITECTURE sections, DESIGN_SPEC 3.5/3.7/3.8/5.1, horde / duel / GameMode / runner / chase / types sources | read only |
| 3 | Converted the owned files (horde .h/.cpp, GameMode .h/.cpp) to LF for editing | ok |
| 4 | Appended `EJamCrowdLoop` to `Core/JamTypes.h` | ok, CRLF kept |
| 5 | Rewrote `Run/JamHordeActor.h` and `.cpp` (crowd, wings, mob, console, debug label) | ok |
| 6 | Edited `Run/JamRunGameMode.h/.cpp` (binding, EndRunAfterMob, timer gate) | ok |
| 7 | Edited four duel camera defaults in `Run/JamRunnerCharacter.h` | ok |
| 8 | Restored CRLF on the four LF files | ok, verified counts |
| 9 | `Build.bat JamGameEditor Win64` (run 1) | Result: Succeeded, 0 warnings |
| 10 | `Build.bat JamGame Android` (run 1) | Failed: clang loop-increment error in FindTrackManager |
| 11 | Replaced the loop with `TActorIterator` test, re-normalised CRLF | ok |
| 12 | `Build.bat JamGame Android` (run 2) | Result: Succeeded |
| 13 | `Build.bat JamGameEditor Win64` (run 2, final code) | Result: Succeeded, 0 warnings |
| 14 | `tasks/QUEUE.md` row 011 Todo -> Review | ok |
| 15 | Wrote `tasks/TASK-011.report.md` and this entry | ok |

## Rework 1 ledger (coordinator audit, 13 Nov)
| # | Operation | Result |
|---|---|---|
| 16 | Edited `Run/JamHordeActor.h` wing defaults: WingNearX 0, WingFarX 450, CloseWingNearX -60, WingInnerY 500, WingOuterY 640; rewrote the wing comment | ok |
| 17 | Edited `Duel/JamDuelManager.h` line `DuelCameraBlendTime` default 0.4 -> 0.5 (only change in Duel/*) | ok |
| 18 | `DuelWingOffset` gets an explicit `bool bTight` parameter; `UpdateFormation` passes `bCrowdTight` | ok |
| 19 | Added a non-shipping check in `SetCrowdMode(true)`: worst slot off-axis angle vs `DuelFieldOfView * 0.5 - 2`, one Warning | ok, not run in PIE |
| 20 | Restored CRLF on `JamHordeActor.h/.cpp` after the multi-line edits | ok, counts verified |
| 21 | Computed worst wing slot off-axis angle with awk for 8-12 members | 10 members: 35.93 deg (X 75, |Y| 605), over the 34 deg limit; 8 members: 34.74 deg |
| 22 | `Build.bat JamGameEditor Win64 Development -WaitMutex` | Result: Succeeded, 924.77 s, 0 JamGame warnings (waited on other builds) |
| 23 | `Build.bat JamGame Android Development -WaitMutex` | Result: Succeeded, 132.31 s |
| 24 | Wrote the Rework 1 section in `tasks/TASK-011.report.md`; added this ledger; committed on the same branch | commit recorded in git log |

Verification status after rework 1: compile only. The new off-axis warning is expected to fire at duel start with the default crowd (35.9 deg); PIE needed to confirm the log line and the on-screen look. Not verified: PIE of any crowd behaviour.
