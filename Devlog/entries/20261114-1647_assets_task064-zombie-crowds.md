# 2026-11-14 16:47 WIB — assets — TASK-064 zombie crowds (code compiles, 72/72 tests; screenshots black)

- **Who / tool:** executor subagent, Claude Haiku 5.5 in Claude Code (PowerShell + UnrealEditor-Cmd Python + UBT).
- **Task:** TASK-064 (`tasks/TASK-064.zombie-crowds.md`) · **Branch / commit:** main, base 857bef1, nothing committed by this entry.
- **Asked for:** skeletal zombie variants on one shared skeleton; horde members and roadside crowds as pooled skeletal zombies; `Jam.God`; JamGame.Crowd tests; three screenshots.
- **Result:**
  - Import done: 15 of 15 Meshy rigs imported as `SK_Zombie_<Variant>` under `/Game/Art/Characters/ZombieVariants/` (7 B5 + 8 B9), each bound to `SK_Zombie_Preview_Skeleton` (bind check passed, 0 failed, 0 missing). Each has `T_Zombie_<Variant>_BC` and `MI_Zombie_<Variant>` (parent M_MeshyProp, BaseColor set), assigned to every material slot. Report: `raw_assets/MESHY-ZMB-VARIANTS/ue_zombie_variant_report.json` (git-ignored).
  - Clip finding: `DA_AnimSet_Zombie` has 14 entries and **no run clip** (no HordeRunA/B, no Run), no CrowdCheer/CrowdClap/CrowdJumpCheer/CrowdOoh/CrowdMob. It has CrowdDance with 9 alternates. The Review anims in `Content/Art/Review/Luna` are on other skeletons (hero, horse, Hips_Skeleton) and do not bind. The horde's chase loop therefore falls back to FightIdle/Idle until a zombie run clip is authored.
  - Code written (not yet compiled successfully): `Run/JamCrowdRules.{h,cpp}` (pure: GrantCount, PickVariantIndex, BuildStripSpots), `Run/JamZombieVisuals.{h,cpp}` (variant loading with skeleton check, ResolveRunClip/ResolveDanceClip, PlayLoop, MakeComponent), `Data/JamCrowdTuning.h` (UPrimaryDataAsset, C++ defaults when DA missing), `Run/JamHordeActor.{h,cpp}` (members and stragglers become USkeletalMeshComponent, cylinders removed, 8 visible horde zombies, crowd pool of 26 with SetCrowdStrip/ReleaseCrowdStrip and one turn-to-runner loop in Tick), `Track/JamTrackSegment.{h,cpp}` (crowd plan per crowd-prop side with its own seed, registered in UpdatePens, released on Configure), `Run/JamChaseComponent.cpp` (`Jam.God` non-shipping CVar guards RegisterMistake), `Tests/JamCrowdTest.cpp` (JamGame.Crowd.Deterministic / Cap / NoClipFallback), `tools/ue/import_zombie_variants.py`.
  - Visible budget: horde 8 + stragglers 6 + crowd 26 = 40. The 26 crowd slots replace the 32 in the brief so the total stays at 40 with the pen stragglers.
- **Verified by:** import (UnrealEditor-Cmd headless, 15 OK lines and report JSON); compile `Build.bat JamGameEditor Win64 Development -NoUBA` Result: Succeeded, 670.7 s, 0 JamGame warnings; automation `Automation RunTests JamGame` 72 of 72 Success, including JamGame.Crowd.Deterministic, Cap, NoClipFallback. Screenshots: `Devlog/media/20261114_task064_run1.png` and `run2.png` were captured with PrintWindow but are fully black (window present, no game content); NOT committed as evidence; duel shot not taken. Not played in PIE.
- **What went wrong:**
  - The UBT build stalled at two `cl.exe` processes (JamChaseComponent.cpp and JamCrowdRules.cpp) from 16:39; CPU stayed at about 2 to 5 s over more than 8 minutes. A retry with `-NoUBA` also stalled at the same point.
  - The stalled processes were not stopped: the permission check denied the stop action and the UBT user-config change, so the build is still hanging.
  - The earlier MonitorSleep-style waits did not return reliably; no source defect has been found, but none has been proven either.
- **Corrected by:** the UBA build hang was bypassed by a rebuild with -NoUBA (not by changing the UBT config). The stalled cl/dotnet processes were not stopped by this agent; they were ended by the earlier kill (16:45) and the -NoUBA build then completed.
- **Human decision needed or taken:** open — whether the horde chase loop may use FightIdle until a zombie run clip is authored; and why the game screenshots are black (rendering or capture method).
- **Not done / not verified:**
  - PIE or visual check of the horde and crowds (black screenshots); duel screenshot.
  - Zombie mesh facing (MeshYawDeg is 0; not checked).
  - `DA_CrowdTuning` asset (C++ defaults are used; the asset is not created).
  - Real visible-count and triangle log in PIE.
- **Screenshot:** none.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 16:32 | Import 15 Meshy rigs as skeletal meshes, MIs, textures | Python `tools/ue/import_zombie_variants.py` via UnrealEditor-Cmd -run=pythonscript, TASK-064 | 15 OK, 0 failed; `raw_assets/MESHY-ZMB-VARIANTS/ue_zombie_variant_report.json`; `Content/Art/Characters/ZombieVariants/` (45 uassets) | Imported; not committed |
| 16:35 | Probe of review animations for a zombie run clip | Python probe in scratchpad (not in repo), TASK-064 | No zombie-skeleton run clip found | Read-only |
| 16:36-16:39 | Source edits: JamCrowdRules, JamZombieVisuals, JamCrowdTuning, JamHordeActor, JamTrackSegment, JamChaseComponent, JamCrowdTest | Claude Code Edit/Write, TASK-064 | See Result above | Written; not compiled |
| 16:39 | Editor build attempt 1 `Build.bat JamGameEditor Win64 Development -WaitMutex` | UBT, TASK-064 | Stalled at cl.exe JamChaseComponent.cpp / JamCrowdRules.cpp, about 2-5 s CPU in 8 min; stop denied | Failed (hung) |
| 16:45 | Editor build attempt 2 with `-NoUBA` | UBT, TASK-064 | Compiled 12 of 12 after the stalled processes were ended; Result: Succeeded, 670.7 s | Built |
| 16:57 | Automation RunTests JamGame | UnrealEditor-Cmd, TASK-064 | 72 of 72 Success | Tests pass |
| 17:00 | Game launch -game 540x960, Jam.God 1, Jam.Start; PrintWindow captures | PowerShell capture script (scratchpad), TASK-064 | Both captures black | Screenshot failed |
| 16:46 | Manual cl run of JamCrowdRules.cpp.obj.rsp from the Intermediate folder | cl.exe, TASK-064 | Returned rc=2 with CoreMinimal.h not found (working directory issue of the manual run, not a source error) | Diagnostic only |

No keys, account identifiers, signed URLs or private perk codes.
