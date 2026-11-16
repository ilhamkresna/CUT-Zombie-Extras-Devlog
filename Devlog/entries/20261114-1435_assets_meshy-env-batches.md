# 2026-11-14 14:35 WIB — assets — Meshy env batches 1-7: manifest, prep, bus doorway cut, contact sheets

- **Who / tool:** asset pipeline worker, Claude Haiku 5.5 in Claude Code (subagent); Blender 5.1 CLI (`blender -b -P`) for all scripts.
- **Task:** Meshy env batches B1-B7 (wreck and bus slots added to manifest) · **Branch / commit:** main, see commit below.
- **Asked for:** add wreck_* and bus3_* slots to `tools/assets/meshy_env_manifest.json`, re-run `prep_meshy_env.py`, cut bus doorways, render contact sheets, list rejects, log the batches.
- **Result:**
  - Manifest: 147 slots (138 + 7 `vehicle_wreck` + 2 `bus3`). Batch counts: B1 10, B2 36, B3 32, B4 60, B6 7 (wrecks), B7 2 (bus3). B5 (7 rigged zombies) excluded, handled by the rig path.
  - Prep (run over all 147 slots, idempotent): 67 new FBX written, 79 skipped as up to date, 1 failed (`mesa_flat_west`). Output in `raw_assets/MESHY-ENV-PREP/<category>/` (git-ignored). Per-category FBX present: clutter 48, facade 21, vehicle 15, vegetation 11, pole 8, crowd 7, landmark 7, tunnel 6, vehicle_wreck 7, roof 4, sign 4, backdrop 3, bus3 2, pickup 1, deck 1, ramp 1.
  - `mesa_flat_west` skipped/failed: source GLB bbox 0.113 x 0.032 x 0.999 m, fit scale 266.7 is outside the 0.05-50 sanity range, so prep refuses it. Left out of the import. Not repaired.
  - Bus doorways: `tools/blender/cut_bus_doorways.py` writes `<slot>_FrontOpen.fbx` (hole at Y = +3.0 m) and `<slot>_RearOpen.fbx` (hole at Y = -3.0 m) for both bus3 slots into `raw_assets/MESHY-ENV-PREP/bus3/`. Hole: 2.4 m wide (Y), 2.4 m high (Z), floor at Z = 0, through the full depth in X. LaneWidth = 300 cm from `Source/JamGame/Data/JamRunnerTuning.h`, so the hole centres are at +/-3.0 m. The earlier note mentioning 4.4 m / 440 cm does not match the code.
  - Check render: `Devlog/media/20261114_bus3_doorways.png` (city bus, FrontOpen left, RearOpen right, looking down the running axis). Both holes are visibly open.
  - Contact sheets: `Devlog/media/20261114_meshy_env_b2_contact.png`, `_b3_contact.png`, `_b4a_contact.png` (30 slots), `_b4b_contact.png` (30 slots), `_b6_wrecks_contact.png`.
- **Verified by:** compile n/a; Blender CLI runs (prep, cut, render, contact sheets) completed with exit 0 and the output files exist; visual review of every contact sheet and the bus check image. Nothing run in Unreal yet (see the import addendum below if present).
- **What went wrong:**
  1. The first bus cut used a boolean DIFFERENCE with the EXACT solver. It produced junk geometry (a cutter box stuck outside the bus, and the front bbox grew by 4 m). Recalculating normals and using the MANIFOLD solver then did nothing at all: the shells are not closed, so the boolean silently leaves the mesh unchanged.
  2. Fallback used: split faces on the four door planes (bisect), then delete faces whose centre lies inside the door volume. This gives a clean-looking hole, but its edges follow the mesh triangulation, so they are not perfectly straight. This is not a true boolean and is recorded here as a deviation from the spec.
  3. The first `bpy` FBX import call passed an unrecognised `apply_unit_scale` keyword; removed.
  4. The bus meshes are 13 m long but only ~6 m wide and ~6.7 m tall, so they read as a minibus/van with wheels rather than a 2.5 m wide city bus. Proportions come from the Meshy output.
- **Corrected by:** self (script fixes listed above). No other agent touched these files during this session, except an external save of `cut_bus_doorways.py` that turned out to be my own edit.
- **Human decision needed or taken:** Bus proportions (a 6 m wide, 6.7 m tall "bus") and the wheels that sit inside the door volumes (the cut removes wheel faces that fall within the hole). Decide whether to regenerate the bus or accept it for the MVP.
- **Not done / not verified:**
  - Meshy's own door positions do not match the cut holes. The cut defines gameplay, so this is accepted for the MVP. Meshy door panels sit on the lower body between the window rows, offset from the lane centres.
  - Wreck facing (+X forward) is a guess and is not verified against the mesh.
  - `wreck_shuttle_bus` renders like a van (10 m fit length), not a bus. Kept as a wreck slot.
  - `wreck_box_trailer` fit length 9 m gives a Y extent of 8.87 m because the trailer is jackknifed. Looks right in the sheet.
  - B4 credit cost is not recorded anywhere in the repo, so it is left as TBD. B1 150, B2 540, B3 480, B5 140, B6 105, B7 30 (+10 abandoned previews) are as supplied by the coordinator.
  - Not checked against the zombie batch (B5, rigged, not prepped by the static script).
- **Screenshot:** `Devlog/media/20261114_bus3_doorways.png`, `Devlog/media/20261114_meshy_env_b2_contact.png`, `Devlog/media/20261114_meshy_env_b3_contact.png`, `Devlog/media/20261114_meshy_env_b4a_contact.png`, `Devlog/media/20261114_meshy_env_b4b_contact.png`, `Devlog/media/20261114_meshy_env_b6_wrecks_contact.png`.

## Contact sheet review (reject list)
- B2: no wrong objects. `mesa_flat_west` is not on the sheet as a usable mesh (a thin stick, prep failed). Rejected for import.
- B3: no wrong objects found.
- B4a / B4b: no wrong objects found.
- B6 wrecks: all seven read as wrecks. `wreck_shuttle_bus` is a van, flagged above.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 14:0x | Manifest edit | Text edit, `tools/assets/meshy_env_manifest.json`, 9 slots added, CRLF kept | 147 slots | Verified (JSON parse) |
| 14:2x | Prep run | Blender 5.1 CLI, `tools/blender/prep_meshy_env.py` (all slots) | 67 ok, 79 skipped, 1 failed (mesa_flat_west) | Verified (report written) |
| 14:30 | Bus cut v1 | `tools/blender/cut_bus_doorways.py`, EXACT boolean | Junk geometry, rejected | Failed |
| 14:31 | Bus cut v2 | MANIFOLD solver with normals recalculated | No change (open shell), rejected | Failed |
| 14:33 | Bus cut v3 | Plane-split plus face delete fallback | Holes open in render | Verified visually |
| 14:33-14:34 | Contact sheets | `tools/blender/contact_sheet.py` x5 | 5 PNGs in Devlog/media | Verified visually |

## Addendum: UE import (run after the commit above)
- **Command:** `UnrealEditor-Cmd.exe JamGame.uproject -run=pythonscript -script=tools/ue/import_meshy_env.py -unattended -nop4 -nosplash`, unchanged script, no fixes needed. Started 14:37, editor exited 14:38 (exit 0, 0 errors, 155 warnings).
- **Result:** 146 slots imported, 1 skipped (`mesa_flat_west`, not prepped), 0 failed. Per category: clutter 48, facade 21, vehicle 15, vegetation 11, pole 8, crowd 7, landmark 7, vehicle_wreck 7, tunnel 6, roof 4, sign 4, backdrop 3, bus3 2, pickup 1, deck 1, ramp 1.
- **Parent:** `/Game/Art/Characters/HeroPreview/M_HeroPreview_BaseColor` (parent of `MI_HeroPreview_BaseColor`), texture param `BaseColorTexture`.
- **Content:** `Content/Art/Env/Meshy/<Category>/` (SM_, T_..._BC, MI_), 438 files. Committed in 777419d.
- **Report:** `raw_assets/MESHY-ENV-PREP/ue_import_report.json` (git-ignored).
- **Known issue:** the two `bus3` base meshes were imported as the uncut bus (no doorway holes). The gameplay variants `<slot>_FrontOpen` / `_RearOpen` are not imported yet. The script only knows base slots, so those need a follow-up.
- **Not verified:** nothing opened in the editor. Collision is box only. Materials and textures not visually checked in UE.
- **Git note:** the Content commit took several minutes because git-lfs hashed the large packages. An index.lock appeared from another process in the meantime. It cleared on its own; I did not delete it.
