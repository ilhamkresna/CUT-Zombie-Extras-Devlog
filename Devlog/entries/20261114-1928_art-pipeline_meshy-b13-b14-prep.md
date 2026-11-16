# 2026-11-14 19:28 WIB — art-pipeline — Meshy B13/B14 env prep (18 slots), fuselage centre hole, sky/decal crops

- **Who / tool:** asset prep worker, Claude Haiku 5.5 in Claude Code; Blender 5.1 CLI scripts (`blender -b -P`) and Python Pillow/numpy. No Unreal process started. No Meshy API calls, no credits spent.
- **Task:** Meshy batch 13 (sky/road) and batch 14 (setpiece), no TASK number given · **Branch / commit:** main, based on 7367d72
- **Asked for:** add the 18 new 3D slots to the env manifest with real sizes (uniform scale), prep them, cut a centre-lane hole in sp_airliner_fuselage, measure tunnel gate openings, crop the sky and road sprite sheets, and make a contact sheet with rejects listed.
- **Result:**
  - `tools/assets/meshy_env_manifest.json`: 18 entries added (236 to 254 slots), categories ramp / platform_vehicle / tunnel_gate / setpiece. `size_rule` documents the new `orient_span_x`.
  - `tools/blender/prep_meshy_env.py`: new `orient_span_x` option (rotate so the longest horizontal extent is along X). Existing behaviour unchanged.
  - Prepped FBX under `raw_assets/MESHY-ENV-PREP/{ramp,setpiece,tunnel_gate,platform_vehicle}/` (ignored by git).
  - `raw_assets/MESHY-ENV-PREP/setpiece/sp_airliner_fuselage_CentreHole.fbx` (cut by `tools/blender/cut_airliner_centre_hole.py`).
  - `tools/assets/crop_skyroad_sheets.py` output: `sky/clouds_01..06.png`, `sky/moon.png` (alpha from black), `sky/img_star_field.png` (copy), `decals/decal_01..28.png`.
  - Check images: `Devlog/media/20261114_meshy_b13_b14_contact.png` (real-size contact, 18 slots), `Devlog/media/20261114_fuselage_hole.png` (view along +X).
  - Helper scripts: `tools/blender/measure_gate_opening.py`, `render_prep_axis_check.py`, `contact_real_size.py`.
- **Prepped sizes (metres after uniform scale; target in brackets):**
  - plank_ramp_tall 5.7 x 5.7 x 3.2 [8 long, 3.2 high]. Length short.
  - plank_ramp_short 3.3 x 2.3 x 2.6 [6 long, 2.6 high]. Length short.
  - container_with_ramp 4.3 x 2.4 x 1.95 [6.1 x 2.4 x 2.6]. Length and height short.
  - tunnel_gate_1lane 4.5 x 4.0 x 4.1. Lateral clear 3.1 m (ok, >= 2.8). Ceiling 2.3-2.9 m (fails 4 m). Length 4.5 (fails 6-10 m).
  - tunnel_gate_2lane 6.6 x 6.6 x 1.19. REJECT: flat hoop, not a gate; a 4 m opening would need about 22 m width under uniform scale.
  - bus_long_parked 8.4 x 3.1 x 3.2. Length short (12). Roof ok.
  - sp_box_truck 4.5 x 2.6 x 3.0. Length short (8). Roof ok.
  - sp_actor_trailer 6.5 x 2.3 x 2.9. Length short (9). Roof ok.
  - sp_equipment_trailer 8.0 x 3.95 x 3.1. Deck 1.4 not checked.
  - sp_coach_bus 6.9 x 3.1 x 3.3. Length short (12). Roof ok.
  - sp_float_zombie_head 9.0 x 3.9 x 5.7; sp_float_monster_hand 9.0 x 4.2 x 7.1; sp_float_saucer 9.0 x 7.8 x 8.7. Tall or wide for a parade float on trailer; review.
  - sp_hay_ramp 3.6 x 3.6 x 2.6. Rise ok.
  - sp_rocket_trailer 14.0 x 7.8 x 6.5 (fins make the width).
  - sp_airliner_fuselage 12.4 x 13.0 x 4.9 (long axis along Y, principal plan axis already within 0.3 deg).
  - sp_metal_ramp 5.5 x 5.5 x 3.0. Rise ok.
  - sp_scaffold_bridge 5.1 x 5.1 x 3.0. Span 5.1 (target 6.5).
- **Verified by:** Blender headless runs (prep report, ray casts, real-size contact sheet, rendered check image). Verified: 18/18 prepped without error; hole ray test. Not verified in UE, not on device.
  - Fuselage hole: rays along +X at Y = 0, Z 0.3-2.6 hit nothing (open passage). Lane centres Y = -3 and +3 are both hit at mid height. The view along +X (`20261114_fuselage_hole.png`) shows a centred, open hole.
  - Gate measure: see sizes above. The 1-lane gate was measured after the fit (4.0 m outer width).
- **What went wrong:**
  - Uniform scale plus the Meshy aspect ratios means the "length" targets are not met on about half the slots. Using the min factor, every slot stays inside its spec; the length ends up short. Director decision needed (accept, or re-generate with a longer prompt aspect).
  - The Meshy airliner still has wings, engines, prop and tail fin. The spec said "wingless". The hole cut leaves wing and engine scraps around its edge (visible in the check render). Not removed, because that was not in the brief.
  - The 2-lane gate came out as a flat ring (1.19 m high). The batch 13 gate prompt did not produce a gate.
  - First run of contact sheet cropped the frame; re-rendered with a wider camera.
  - First hole-cut attempt used a wrong edge-centre API call; fixed and rerun.
- **Corrected by:** self (re-runs; `--only` prep; report counts include earlier runs from the shared prep_report.json, which is expected).
- **Human decision needed or taken:** Decide for each flagged slot: accept the short length, regenerate, or change the spec. Decide whether the airliner wings should be removed before import. Reject tunnel_gate_2lane (recommended), and rework tunnel_gate_1lane (ceiling too low for 4 m).
- **Not done / not verified:**
  - Tunnel gates not regenerated; the 2-lane is recommended as a reject.
  - The fuselage hole edges are ragged (about 0.1 m subdivision); no boolean solver used because the shell is open.
  - Deck height of sp_equipment_trailer not measured.
  - Sky/decal crops: visually checked as a mosaic (28 decals, 6 clouds). Moon alpha from black, not re-checked by eye at scale.
  - Not imported into UE; asset_manifest.csv not touched (UE import is the UI lane's job later).
  - Sourced third-party assets: none. All are Meshy AI outputs.
- **Screenshot:** `Devlog/media/20261114_meshy_b13_b14_contact.png`, `Devlog/media/20261114_fuselage_hole.png`

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| - | Manifest: 18 entries appended, slot_count 254 | Python text edit, CRLF kept | tools/assets/meshy_env_manifest.json | Verified (JSON parse) |
| - | Prep: orient_span_x added; prep run `--only` 18 slots | Blender 5.1 CLI, tools/blender/prep_meshy_env.py | 18 FBX in raw_assets/MESHY-ENV-PREP (ignored) | Verified (bbox report); sizes flagged above |
| - | Raw bbox inspection | Blender CLI, scratch script | scratchpad raw_dims.json | Verified |
| - | Fuselage PCA align + centre hole cut, export | Blender CLI, tools/blender/cut_airliner_centre_hole.py | sp_airliner_fuselage_CentreHole.fbx; 1485 faces removed | Verified by ray test and render |
| - | Gate opening measure | Blender CLI, tools/blender/measure_gate_opening.py | Printed profile; 1lane ceiling 2.3-2.9 m, 2lane flat | Verified (measured) |
| - | Sky/road sprite crops | python -I tools/assets/crop_skyroad_sheets.py (Pillow, numpy, scipy) | sky/clouds_01..06, moon, star field; decals/decal_01..28 | Verified by mosaic view |
| - | Real-size contact sheet of 18 slots | Blender CLI, tools/blender/contact_real_size.py | Devlog/media/20261114_meshy_b13_b14_contact.png | Verified by view |
| - | Meshy generation | none | none, no credits spent | n/a |

## Addendum (orchestrator decisions, same session)
- **Accepted:** length shortfalls on uniform scale (plank ramps, container, bus, box truck, coach bus, scaffold bridge, actor trailer). Recorded in manifest `note` fields.
- **tunnel_gate_2lane:** REJECT confirmed. Code builds the 2-lane gate as two 1-lane gates.
- **tunnel_gate_1lane re-scaled:** fit `width_y` 5.3 m (was 4.0). Factor 5.99 on raw. Inner ceiling now 3.05 m at the worst X (3.9-4.7 m at centre). Lateral clear 4.1 m at 3.5-4.2 m height. Length 6.0 m (meets 6-10 m). The orchestrator's "width ~3.6 m" does not reach ceiling 3.0 m under uniform scale, so 3.6 m was not used.
- **Fuselage made wingless:** body axis at X 0, Z 1.9, radius 1.8 (from vertices near X 0, Y -4.5..-0.5). Faces with centre outside radius + 0.3 m are deleted (radial, not |X|, so the tail fin and prop go too). Long triangles subdivided first. Then the centre hole (Y 0, Z 1.5, r 1.4) is cut. Ray test at Y 0 still 0 hits. Output overwrites `raw_assets/MESHY-ENV-PREP/setpiece/sp_airliner_fuselage_CentreHole.fbx` (raw, ignored by git).
  - Residual: 72 vertices (of 15.8k) lie 2.1-2.4 m from the axis, i.e. wing-root remnants inside the 0.3 m margin. Remaining X ±2.4, Z 0.06-4.08.
  - A small hook/ring remains under the belly at the nose-side (front right in the side view). Not removed.
- **Renders:** `Devlog/media/20261114_fuselage_hole.png` (view along +X) and `Devlog/media/20261114_fuselage_hole_top.png` (from above). Both looked at.

Ledger addendum:
| - | Gate re-scale width 5.3 m + measure | tools/blender/prep_meshy_env.py --only tunnel_gate_1lane --force; tools/blender/measure_gate_opening.py | ceiling 3.05 m, length 6.0 m, lateral 4.1 m | Verified (measured) |
| - | Fuselage wingless + centre hole re-cut | tools/blender/cut_airliner_centre_hole.py | CentreHole FBX overwritten; 3999 wing faces removed; 1824 hole faces | Verified (ray test + 2 renders) |
