# 2026-11-14 13:15 WIB — assets/presentation — TASK-022b duel enemy visible + facing (scope cut)

- **Who / tool:** worker, Claude Sonnet 5.5 in Claude Code; UnrealEditor (PIE, Python tick driver), UnrealEditor-Cmd, UBT
- **Task:** TASK-022b · **Branch / commit:** main, see git log "TASK-022b"
- **Asked for:** visual polish of the skeletal characters; mid-task the owner cut scope to ONLY step 3: duel enemy visible and facing the player in a forced duel so duels can be play-tested. Graphics polish deferred.
- **Result:** DuelEnemy mesh yaw in Config/DefaultGame.ini changed 90 to -90 (same convention as the hero: Meshy forward is local +Y, the enemy actor's +X points at the runner). Location Z=0 and scale 1 kept: mesh bounds (read from the asset) hero 180 cm, zombie 190 cm, both with feet at the origin, so the 190 vs 180 ratio is already right. New non-shipping console command `Jam.Start` (Title to Running without a key press) in JamRunGameMode.cpp so scripted play-tests can start a run. Script tools/ue/task022b_duel_shots.py drives PIE (540x960): Jam.Start, then Jam.Duel 1 s later, then HighResShot loop.
- **Deferred (not started or reverted):** step 1 materials (hero slot binding, zombie texture + MI), step 2 run-camera framing in DA_CameraModes, step 4 skeletal horde. Partial work from step 1 (zombie texture/MI, extra zombie horde/crowd clips, SK_* material edits) was reverted before commit: nothing of it is in the repo.
- **Verified by:** UBT JamGameEditor build OK (322 s); automation `JamGame` 47/47 pass; PIE screenshots (540x960). Before the fix (yaw 90) the enemy showed its back/side to the hero; after (yaw -90) it faces the hero (face visible, same direction as the hero's gaze) and stands on the ground. Images: Devlog/media/20261114_task022b_duel_jumpout.png (ambush landing, JumpOut clip, enemy mid-air), 20261114_task022b_duel_fightidle.png (enemy in its fight stance facing the hero; pink bar = Low-attack telegraph cue), 20261114_task022b_duel_open.png (second run, enemy upright in the Open/Dizzy stance, face to camera side).
- **What went wrong:** (1) -ExecutePythonScript quits the editor when the script returns; used `-ExecCmds="py <script>"` with a Slate post-tick callback instead. (2) Jam.Retry only works from Game Over and Jam.Duel only in Running, so the first captures never had a duel (Title screen); added Jam.Start. (3) Starting the duel 4 s into the run let the horde catch the runner first; the duel is now forced 1 s after start. (4) HighResShot drops frames when issued faster than about 0.7 s (13 of 40 shots saved at 0.28 s).
- **Corrected by:** same agent.
- **Human decision needed or taken:** owner cut scope to step 3 (taken, via orchestrator).
- **Not done / not verified:** an actual AttackSweep/AttackOverhead strike frame was NOT captured as a distinct image (the strike lasts 0.3 s; the Low telegraph uses a Crouch slot that has no clip, so the enemy holds its previous pose during telegraph); the enemy FightIdle stance looks nearly static in stills; enemy untextured grey; hero cut off at the bottom in the duel camera; Android device; no dodge/hit-react/KO clip checked.
- **Screenshot:** Devlog/media/20261114_task022b_duel_*.png

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 12:28 | Imported 7 zombie horde/crowd clips, zombie texture + MI, bound materials (step 1/4 start) | UnrealEditor-Cmd python, TASK-022b | imported OK, slots read 1/1 | NOT screenshot-verified; reverted at 12:40 after scope cut |
| 12:40 | Revert of the above (git checkout of changed uassets, removal of own new assets) | git | clean tree except DefaultEngine.ini (not mine) | status checked |
| 12:42-13:08 | PIE duel capture runs (4 runs) | task022b_duel_shots.py | run 3 first showed enemy; yaw 90 faces away | screenshots viewed |
| 12:58 | Added Jam.Start console command | C++ edit | build OK | used by capture script |
| 13:05 | Yaw 90 to -90 for DuelEnemy | Config/DefaultGame.ini | enemy faces hero | PIE screenshot viewed |
| 13:13 | Automation JamGame | UnrealEditor-Cmd | 47 passed | all green |
