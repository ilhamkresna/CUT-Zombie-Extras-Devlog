# 2026-11-14 19:14 WIB — art-pipeline — TASK-067 intro asset imports (partial: C++ not built)

- **Who / tool:** executor, Claude Haiku 5.5 in Claude Code (Bash, PowerShell, Blender 5.1 CLI, UnrealEditor-Cmd Python)
- **Task:** TASK-067 (intro and caught cinematics), asset part only · **Branch / commit:** main, see commit below
- **Asked for:** import the crew NPC rigs, the intro set pieces and the hero/NPC/zombie clips into Unreal, and add the intro set to the env manifest; then build the cinematic C++ and data asset, run tests and capture screenshots.
- **Result:** Content now holds `SK_Npc_{Cameraman,BoomOperator,MakeupArtist,ProductionAssistant}` with `T_`/`MI_Npc_*` (M_MeshyProp, all slots), 13 clips (`A_Hero_SitIdle`, `A_Hero_SitToStand`, `A_Hero_RunLeap`, `A_Npc_*` x7, `A_Zombie_LungeBite`, `A_Zombie_BiteLoop`), and `Content/Art/Env/Meshy/Intro_set/` (`SM_/T_/MI_` for the four set pieces). `tools/assets/meshy_env_manifest.json` has four `intro_set` entries (slot_count 236). `import_meshy_env.py` takes `JAM_IMPORT_CATEGORY`. Blender prep produced FBX for the four set pieces (heights 2.6 / 2.6, length 2.4 and 5.0).
- **Verified by:** headless UnrealEditor-Cmd Python import, log shows `npcs=4 clips=13 failed=0 missing=0` (report: raw_assets/MESHY-INTRO-B12-20261114/ue_intro_import_report.json). Set-piece import: assets exist in Content; the summary line was not re-read. Blender prep: `[ok]` for all four set pieces. Nothing compiled, nothing played in PIE, no screenshot.
- **What went wrong:** the Interchange importer logged `cannot merge bone tree with the existing skeleton` and `Imported skeleton has some invalid` for the NPC rigs. The bind check (skeleton equals SK_Hero_Preview_Skeleton) still passed, so these were accepted without a visual check. The first intro import run exited silently before the Python ran; the second run succeeded. Build.bat called through bash failed on the spaces in its path, so I relaunched it through PowerShell. The wall came out 5.0 m × 2.6 m, not the 3.5 m tall in the packet, because one uniform fit cannot meet both height and length. The C++ cinematic module (`Source/JamGame/Cinematic/`, tests, GameMode/Config/PlayerController edits) and `tools/ue/make_task067_cinematics.py` were written but never compiled, so they are not in this commit.
- **Corrected by:** self (re-ran the importer after bite clips appeared, with zero failures). Coordinator: owner priority change, so I cancelled my queued build and started no further Unreal process.
- **Human decision needed or taken:** none. Owner priority moved the next Unreal window to the UI lane (Codex).
- **Not done / not verified:**
  1. C++ cinematic module: not compiled. The build was queued behind another worker's build and cancelled by the coordinator.
  2. `make_task067_cinematics.py` (creates DA_Cinematic_Intro/Caught and wires DA_GameConfig): not run; needs the built UJamCinematicDA class.
  3. Tests `JamGame.Cinematic.*` and the 75-test suite: not run.
  4. Screenshots `Devlog/media/20261114_task067_{menu,1,2,3,4}.png`: not captured.
  5. QUEUE row 067: not added (tasks/QUEUE.md has another lane's uncommitted changes).
  6. Set and cast placement, door orientation and camera offsets are guesses until screenshots exist.
  7. Zombies are data cast members, not horde-pool actors (deviation from the packet).
- **Screenshot:** none

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 18:50 | Edit manifest (4 intro_set entries) | Bash python, TASK-067 | tools/assets/meshy_env_manifest.json, slot_count 236 | Parsed as JSON; pending import |
| 18:55 | Blender prep, 4 set pieces | blender -b -P tools/blender/prep_meshy_env.py --only set_* | raw_assets/MESHY-ENV-PREP/intro_set/*.fbx | 4 x [ok] (raw, git-ignored) |
| 18:57 | UE import NPC rigs, 9 clips | UnrealEditor-Cmd -run=pythonscript import_task067_intro.py | Crew/SK_Npc_*, Crew/Anims, HeroPreview/Anims | npcs=4 clips=9 failed=0 |
| 18:59 | UE import bite clips + re-run | same script, updated | ZombiePreview/Anims bite clips, Crew GrabbedBitten/TurnZombie | npcs=4 clips=13 failed=0 |
| 19:01 | UE import intro set (env importer, category filter) | import_meshy_env.py, JAM_IMPORT_CATEGORY=intro_set | Env/Meshy/Intro_set SM/T/MI | Assets present; summary not re-read |
| 19:05 | Queued JamGameEditor build, then cancelled | Build.bat via PowerShell -WaitMutex | No build ran for this entry | Not compiled; cancelled per coordinator |
