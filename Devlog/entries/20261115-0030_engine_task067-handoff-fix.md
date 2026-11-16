# 2026-11-15 00:30 WIB — engine — TASK-067 hand-off bug fix (intro to run camera)

- **Who / tool:** bug-fix worker, Claude Haiku 5.5 in Claude Code (Unreal 5.8 C++ build via Build.bat, UnrealEditor-Cmd automation, UnrealEditor -game)
- **Task:** TASK-067 (intro and hand-off) with TASK-069 screenshots · **Branch / commit:** main, commit see git log (committed after this entry)
- **Asked for:** fix the camera locked on the intro set during a run; Jam.SkipIntro test switch; intro set on a floor; headless hand-off test; screenshots; owner feedback (menu shows hero + 2 crew only, run camera shows whole hero).
- **Root cause (confirmed from Saved/Logs):** `Jam.Start` in ExecCmds ran before `SetupCinematics` (the director is built on the first tick with a pawn). `StartRun` found no director, entered Running, and the later setup called `BeginMenuIdle`, which locked the view on the cine camera, showed the set and hid the runner. Letterbox bars were the cine camera's 16:9 constraint.
- **Result:**
  - `StartRun` now waits (`PlanStart` Defer) until the director exists; then plays the intro or starts instantly.
  - Intro setup only in Title. `Jam.SkipIntro 1|0` (non-shipping) skips the intro set.
  - Hand-off forces the Run camera pose (`SetCameraMode(Run, 0)`), clears the letterbox flag (cine camera no longer 16:9-constrained), and shows the horde again.
  - Floor plane spawned behind the start, plus two side wall flats reusing the DA's wall mesh.
  - Title idle hides all cast except the hero and two crew (MakeupArtist, ProductionAssistant); horde hidden in Title.
  - Log line `Hand-off check` (view target, letterbox) after each hand-off. Confirmed in a run: `view target JamRunnerCharacter_0 (runner yes), letterbox off`.
- **Verified by:** compile (JamGameEditor Win64 Development: succeeded); automation (`Automation RunTests JamGame`: 87 of 87 pass, incl. new `JamGame.Cinematic.StartWaitsForDirector`); game run with -game screenshots (title idle, run view).
- **What went wrong:** the first hand-off fix was not enough to fix the menu framing; SendKeys sometimes did not reach the game window (fixed by focusing via AppActivate); PrintWindow was used for capture; one capture overwrote a good frame (restored by re-capture). Build times ~15–18 min each.
- **Corrected by:** self (root cause from the log, not from the screenshot alone).
- **Human decision needed or taken:** none.
- **Not done / not verified:**
  - Crew NPCs (MakeupArtist, ProductionAssistant) are kept but are not visible in the title frame. Missing-bone warnings on SK_Npc_* / SK_Hero_Preview_Skeleton in the log may be the cause. Not fixed.
  - Run framing: the latest run capture shows only the hero's head and shoulders; an earlier capture of the same build showed the whole hero with feet near 90 % height. Not reproduced consistently. Owner's target (feet near 80 %) not confirmed.
  - Intro beat order is authored in DA_Cinematic_Intro (tools/ue/make_task067_cinematics.py); not changed. Intro frames were not captured for this build (the key press did not register in the capture session, so the intro2/intro4 frames were deleted, not committed).
  - Hazard screenshots: CraneLow captured (pink telegraph band across three lanes with a JUMP prompt; crane arm is greybox). DropUFO, LaserSweep, PlaneCrash, BusDoorway not recaptured for this build; the existing 20261114_task069_{ufo,laser,planecrash}.png are older and unchanged.
  - No world-level automation test for the view target; the view-target check is the log line above.
  - Not verified on Android or in PIE.
- **Screenshot:** Devlog/media/20261114_fix067_menu.png (title idle), 20261114_fix067_run.png (run view), 20261114_task069_crane.png (CraneLow telegraph).

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 2026-11-14 ~21:00 | Code: PlanStart, director letterbox, floor/walls, Jam.SkipIntro, deferred start | Claude Haiku 5.5, Edit tool; TASK-067 | Source/JamGame/Cinematic/*, Run/JamRunGameMode.*, Tests/JamCinematicTest.cpp | compile ok |
| 2026-11-14 ~21:10 | Editor build JamGameEditor (x3 more) | Build.bat; TASK-067 | Binaries updated | compile ok |
| 2026-11-14 ~21:20 | Automation RunTests JamGame | UnrealEditor-Cmd -nullrhi | 87/87 pass | automation ok |
| 2026-11-15 ~00:20 | Game -game captures (menu, run, crane) | UnrealEditor -game, PrintWindow capture | Devlog/media/20261114_fix067_menu.png, _run.png, task069_crane.png | captured; visually described |
