# 2026-11-14 18:51 WIB — assets — TASK-066 real-scale Meshy verification (bounds, MI, screenshots)

- **Who / tool:** verification worker, Claude Haiku 5.5 in Claude Code; UnrealEditor-Cmd `-run=pythonscript` (headless, `-nullrhi`) and UnrealEditor.exe `-game` for screenshots
- **Task:** TASK-066 real-scale re-prep verification · **Branch / commit:** main, commit recorded in the git log after this entry (after 5c9b12d / ea3ccfa)
- **Asked for:** read-only bounds check of 20 sampled `/Game/Art/Env/Meshy` meshes against `tools/assets/meshy_env_manifest.json` (±10%), MI parent check, WorldGrid check on the sampled meshes, fix only obvious slot data issues, three game screenshots at 540x960 with an honest description.
- **Result:**
  - MI check: 229 of 229 `MI_*` under `/Game/Art/Env/Meshy` resolve to a valid parent. `M_MeshyProp` and `M_MeshyPropGlow` both exist.
  - Bounds (20 sampled, height = Z, length = longest horizontal): 19 of 20 within ±10% of target. Outlier: `obs_low_hay_bale` height 0.80 m vs target 0.90 m (-11%), not changed.
  - Sampled meshes: 20 of 20 had slot 0 on `/Engine/EngineMaterials/WorldGridMaterial`. Re-assigned slot 0 to the matching `MI_<Name>` (parent valid) and saved the 20 `SM_*.uasset` files.
  - v2 (coordinator-approved): all 233 `SM_*` under `/Game/Art/Env/Meshy` had every WorldGrid slot re-assigned to the matching MI (bus FrontOpen/RearOpen use the base MI). Read-back: 0 WorldGrid slots left across 233 meshes. `tools/ue/import_meshy_env.py` patched so the already-imported path re-applies the MI to all slots; the old `assign_material` only wrote slot 0 and, via a copied struct list, did not persist at all. Not re-run as a full batch.
  - Screenshots: `Devlog/media/20261114_scale066_1.png`, `_2.png`, `_3.png` (540x960, not black on first capture).
- **Verified by:** headless UE Python (bounds, MI, slot materials) for the numbers above; visual read of three PrintWindow frames for the description below. Not played; no PIE; no Android device.
- **What went wrong:** nothing failed in the scripts. The WorldGrid slot 0 cause is not established; the reimport that deleted and restored M_MeshyProp / M_MeshyPropGlow is the likely source, but this was not proven.
- **Corrected by:** self (slot 0 re-assigned on the 20 sampled meshes only, per the task's "fix only obvious data issues").
- **Human decision needed or taken:** approved by the coordinator: apply the slot fix to all SM_* under /Game/Art/Env/Meshy and patch the importer so every reimport assigns the MI to all slots (done, see v2 below).
- **Not done / not verified:**
  - Bounds for the other 213 meshes and for the 4 category targets (`width_y`, `clamp_z`) were not checked.
  - `obs_low_hay_bale` height outside tolerance; not investigated.
  - v1 screenshots had no car on screen; v2 forces `Jam.HazardNext OncomingCar` at launch (see below).
  - Bounds for the 213 non-sampled meshes not re-measured.
- **Screenshot:** v1 `Devlog/media/20261114_scale066_1.png` to `_3.png`; v2 (after the slot fix, with forced oncoming car) `Devlog/media/20261114_scale066_v2_1.png` to `_v2_3.png`

### What the screenshots show (honest, from the three frames)
- Game runs; HUD shows "SCENE 1 - TAKE 1", "x1", "TAP!". The top-left HUD label overlaps the logo; not part of this check.
- No car or van is visible in the lane or on the parking strip in these frames, so car size vs the hero (about 1.8 m) cannot be confirmed from these shots.
- Mid-distance props: two crate-like blocks (obstacle_block style) sit near the lane edges; they do not clearly block a free lane. Round black discs and gold cog rings are small and lane-sized.
- Sidewalk lamp posts and facade blocks look plausibly large; no checker pattern is visible.
- Surfaces on the props look flat dark grey/blue, consistent with a WorldGrid-style material on the unfixed meshes. Not confirmed for the 20 fixed meshes in-game.

### v2 (after coordinator correction): colours and car
- Launch: `-game -windowed -ResX=540 -ResY=960 -ExecCmds="Jam.God 1,Jam.Start,Jam.HazardNext OncomingCar"`, 60 s warm-up, PrintWindow flag 2, three frames 4 s apart (`scratchpad/scale066_capture_v2.ps1`). The command exists in `Source/JamGame/Track/JamTrackManager.cpp`.
- Colours are back: facades show red, purple, yellow and green; props are coloured (not flat grey or checker).
- Car: an oncoming car is visible mid-lane in frames 1-2 as a small white/black object, roughly the size of the mid-distance zombie or smaller; not a precise measurement. A tall black vehicle on the left sidewalk looks taller than the hero (roughly 2 hero heights on screen); it is at the sidewalk, not in a lane.
- Frame 3 is motion-blurred. A pink horizontal band spans the full track near the hero in frames 1-2 (not identified; may be a lane-start or gate prop). Top-left HUD label still overlaps the logo.
- Hero size (about 1.8 m) cannot be cleanly read because the hero is cut off at the bottom edge.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 18:47 | Read-only bounds + MI check (20 sampled meshes, 229 MIs) | `scale066_check.py` via UnrealEditor-Cmd `-run=pythonscript -nullrhi`, TASK-066 | MI 229/229 valid; 19/20 in tolerance; hay bale 0.80 vs 0.90 m; 20/20 slot0 WorldGrid; report in scratchpad `scale066_report.json` | Read-only, done |
| 18:48 | Slot-0 re-assign to matching MI on the 20 sampled meshes, saved | `scale066_fix_mi.py` via UnrealEditor-Cmd, TASK-066 | 20 `SM_*.uasset` saved (Clutter, Crowd, Facade, Obstacle_*, Pole, Sign, Vegetation, Vehicle, Vehicle_wreck); folder check: 213/233 still WorldGrid | Saved, not yet compiled or played |
| 18:51 | Importer patch: `assign_material` now loops every slot with `set_material`; already-imported path re-applies MI | Claude Code Edit, `tools/ue/import_meshy_env.py`, TASK-066 | Not run as batch | Written; not run |
| 18:51 | Slot fix on all SM_* under Meshy, read-back | `scale066_fix_all.py` via UnrealEditor-Cmd, TASK-066 | 233 SM, 233 slots, 0 WorldGrid left; first run with set_editor_property wrote nothing (213 still WorldGrid), fixed with set_material | Saved, read back 0 |
| 18:53-18:54 | Game launch v2 with `Jam.HazardNext OncomingCar` | `scale066_capture_v2.ps1`, TASK-066 | `Devlog/media/20261114_scale066_v2_{1,2,3}.png` | Captured, described |
| 18:49-18:50 | Game launch -game 540x960, `Jam.God 1,Jam.Start`, 60 s warm-up, PrintWindow flag 2 x3 | `scale066_capture.ps1` (scratchpad), TASK-066 | `Devlog/media/20261114_scale066_{1,2,3}.png`, not black | Captured, visually described |

No keys, account identifiers, signed URLs or private perk codes.
