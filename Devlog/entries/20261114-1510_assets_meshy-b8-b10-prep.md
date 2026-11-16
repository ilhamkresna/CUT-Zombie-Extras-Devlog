# 2026-11-14 15:10 WIB — assets — Meshy B8 obstacles and B10 env: manifest, prep, contact review

- **Who / tool:** asset-prep worker, Claude Haiku 5.5 in Claude Code; Blender 5.1 CLI (`blender -b -P`) for prep and contact sheets; python for manifest edits
- **Task:** Meshy B8 (obstacles, 30 slots) and B10 (env, 37 slots) prep · **Branch / commit:** main, see commit message
- **Asked for:** add 67 slots to `tools/assets/meshy_env_manifest.json` by orchestrator category rules; run `prep_meshy_env.py` for them; contact sheets; list rejects, slide-safe flags, arch openings; log credits B8 450, B9 160 (rigged zombies, not prepped here), B10 555.
- **Result:**
  - Manifest: 214 slots (147 + 67). Batch rows carry `review` where a verdict applies.
  - Prepped FBX in `raw_assets/MESHY-ENV-PREP/<category>/`: 66 (obstacle_high 10, obstacle_low 10, obstacle_block 10, landmark 4, far_scenery 3, facade 14, road_arch 4, clutter 11). 1 failed (bg_hills_rolling, broken geometry).
  - Contact sheets: `Devlog/media/20261114_meshy_b8_obstacles_contact.png`, `Devlog/media/20261114_meshy_b10a_contact.png` (first 19 B10 slots), `Devlog/media/20261114_meshy_b10b_contact.png` (last 18).
  - Script change: `tools/blender/prep_meshy_env.py` gained `width_y`, `orient_span_y`, `clamp_z`, `max_factor` keys and per-slot metrics (`clear_min_z_centre60_m`, `arch_opening`, `nonuniform_z_clamped`).
- **Verified by:** Blender run (exit 0 for the 66 prepped; 1 failure reported by the script) and contact-sheet visual review by the agent. Not verified in UE: no Unreal lease, no import, no PIE.
- **Rejects (contact sheet + metrics):**
  - `bg_hills_rolling`: broken, thin vertical needle (raw bbox 0.03 x 0.02 x 1.0 m). Prep failed.
  - `bg_soundstage_flat`: wrong object, reads as an elephant head.
  - `arch_scifi_ring`: a hoop, not an arch. Opening 0.63 m wide, 0 m clearance.
  - `arch_town_lightstring`: a solid box with no arch. The metric opening (13.4 x 6.3 m) is not valid for a solid box.
  - `arch_monster_trees`: thin branch arc. Opening 8.3 m x 0.14 m.
  - Flagged, not rejected: `arch_west_ranch` (gate posts plus low sign, no measurable 13 x 6 opening, centre clearance 0.05 m); `bg_soundstage_arched` (reads as a plain box).
- **Slide-safe flags (obstacle_high, underside = lowest vertex Z in central 60% of width, must be >= 1.4 m):**
  - Pass: `obs_high_pipe_frame` 1.64 m.
  - Not slide-safe (below 1.4 m): banner_bar 0.09, boom_arm 0.02, light_rig 0.005, scaffold_bridge 0.0, hanging_sign 0.13, west_beam 0.43, monster_branch 0.004, scifi_laser_gate 0.003, clothesline 0.38.
  - Metric is vertex-based and approximate. Low values mean ground-level geometry sits in the centre third.
- **Arch openings (clear inner width x height, need >= 13 x 6 m):**
  - No arch passes on a valid object. `arch_town_lightstring` 13.4 x 6.3 m is the only metric that passes, but that mesh is a solid box (rejected).
  - `arch_west_ranch`: not measurable. `arch_monster_trees` 8.3 x 0.14 m. `arch_scifi_ring` 0.6 x 0.0 m.
- **Other flags:**
  - obstacle_low: 7 of 10 were taller than 0.9 m after width fit and were squashed in Z to 0.8 m (`nonuniform_z_clamped`): sandbags, cable_ramp, film_cans, hay_bale, monster_tombstones, scifi_pipes, scifi_crater.
  - obstacle_block: 7 of 10 outside 2.4-3.2 m height after width fit: crate_tower 8.1 m, scifi_pod 7.5 m, lighting_cart 4.6 m, ticket_booth 4.0 m, portable_toilet 3.8 m, monster_gate 3.24 m, west_wagon_back 1.4 m. In range: container, camera_truck_back, prop_wall.
  - obstacle_high heights came out 1.25-2.66 m, not 3.0 m. Width (2.7 m) was given priority over the 3.0 m height target because a min-factor fit on both shrank the banners to ~0.8 m wide.
  - far_scenery at 120 m long: heights 73-115 m. Confirm that is intended.
- **Decisions and deviations (orchestrator to confirm):**
  - **Lane width is 300 cm, not ~440 cm.** `Source/JamGame/Data/JamRunnerTuning.h` `LaneWidth = 300.f`, and the horde and duel values match. Used 3.0 m: width targets 2.7 m (high and block), 2.55 m (low), 16 m (arch). If 440 cm is the real lane width, every obstacle width target changes.
  - Obstacles and arches were rotated about Z so the longest horizontal axis lies along Y (`orient_span_y`). Meshy GLBs come in arbitrary orientation; without this a 2.7 m "width" was really a thickness. Not in the spec; applied to all obstacle and road_arch rows.
  - Far scenery needed `max_factor` 250. A 1 m GLB scaled to 120 m is factor 120, above the old cap of 50.
  - Themes: literal prefix rule. `arch_west_ranch` is Shared although it reads as WildWest. `obs_high_monster_branch`, `obs_low_monster_coffin`, `obs_block_monster_gate` and the scifi obstacles are Shared (the rule only lists `obs_*_west_`).
  - "Lateral" = Blender Y as the prep script measures it. The Blender-to-UE axis mapping of Y after FBX export is not checked in UE.
- **Human decision needed or taken:** Lane width (300 vs 440), whether the 9 non-slide-safe high obstacles are redesigned or accepted, and whether the 7 out-of-range blocks are re-gated.
- **Not done / not verified:**
  - No UE import (no lease). FBX axis and pivot not checked in UE.
  - B9 zombies not prepped here (rigged, separate path).
  - Contact sheets use the raw GLBs, not the prepped FBX. Sizes come from the report table, not the sheet.
  - Clearance and arch measurements are vertex-based approximations.
- **Screenshot:** `Devlog/media/20261114_meshy_b8_obstacles_contact.png`, `Devlog/media/20261114_meshy_b10a_contact.png`, `Devlog/media/20261114_meshy_b10b_contact.png`

## Operation ledger (append while working)
| Step | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| Manifest: 67 slots appended (B8 30, B10 37), CRLF preserved | python, category rules from orchestrator | slot_count 147 to 214 | done, git diff shows additions only |
| `prep_meshy_env.py`: added `width_y`, `clamp_z`, metrics | Blender 5.1 CLI, edit | script runs | done |
| Prep run 1 (uniform min-factor fit) | `blender -b -P tools/blender/prep_meshy_env.py --only ...` | obstacle heights won, widths too narrow (banners ~0.8 m) | superseded |
| Prep run 2 with `orient_span_y`, `max_factor` 250, `--force` for 38 slots | same | 37 OK, bg_hills_rolling FAILED (broken GLB) | done, 66 FBX total |
| Manifest review fields (7 slots) | python | REJECT x5, FLAG x2 | done |
| Contact sheets x3 | `blender -b -P tools/blender/contact_sheet.py` on raw GLBs | 3 PNGs in Devlog/media | reviewed by agent, not by a human |

## Addendum 16:00 WIB — slide-gap carve for obstacle_high (coordinator follow-up)

- **Script:** `tools/blender/carve_slide_gap.py` (run from raw GLB, not the earlier FBX). Per slot: orient long axis to Y, uniform scale to 3.0 m height, squash Y to 2.8 m only if wider, pivot, delete faces whose centre is in |Y| <= 1.15 m and Z <= 1.5 m, measure lowest vertex Z in |Y| <= 1.15 m (must be >= 1.4 m), export FBX over `raw_assets/MESHY-ENV-PREP/obstacle_high/`.
- **Result: 0 of 10 pass the literal 1.4 m test.** Residual lowest Z in the gap: banner_bar 0.006, boom_arm 0.90, light_rig 0.003, pipe_frame 0.008, scaffold_bridge 0.13, hanging_sign 0.86, west_beam 0.00, monster_branch 0.002, scifi_laser_gate 0.001, clothesline 0.00 (all m). Not traced per vertex. Likely causes: faces whose centre sits above Z 1.5 but still reach down into the gap (cloth, sign panel, frame members), and post feet inside |Y| <= 1.15.
- **Visual check** (`Devlog/media/20261114_high_obstacles_carved.png`, front view along +X, grey reference bar at Z 1.4): most stay recognisable (clothesline, laser gate, west beam, pipe frame, light rig, boom arm, banner bar). Fragmentary or reduced: scaffold_bridge (reads as loose strut pieces), monster_branch (log gone, rock bases left), hanging_sign (panel floats on chains, posts removed). These are reject candidates under the coordinator rule.
- **Not done:** no stricter variant (e.g. also deleting faces with any vertex in the gap below Z 1.4) was run. That would change the design, so it is left for the coordinator's call. Not verified in UE.
- **Credits/logging:** this addendum; ledger for this step: carve script run once (exit 0, 10 processed, 0 pass), check image rendered and viewed.
- **Commit:** see commit message (script, image, entry only).

## Addendum 2 — strict carve approved and run

- **Mode:** `carve_slide_gap.py --strict`: delete every face with ANY vertex in |Y| <= 1.15 m and Z < 1.45 m. Each slot rebuilt from its raw GLB (uncut source), not from the earlier carved FBX.
- **Pass (7):** banner_bar 1.59 m, boom_arm 1.45, light_rig 2.07, pipe_frame 2.46, west_beam 1.45, scifi_laser_gate 2.60, clothesline 1.45 (all >= 1.4 m). Each is still recognisable on the check image.
- **Rejected (3, unchanged):** scaffold_bridge, monster_branch, hanging_sign. Not processed in this run.
- **Note:** strict mode guarantees >= 1.45 m by construction, so the measurement only confirms it. Recognisability is the real check, judged by the agent on one image, not by a human.
- **Outputs:** `Devlog/media/20261114_high_obstacles_carved.png` (overwritten with the 7-slot strict render); `raw_assets/MESHY-ENV-PREP/obstacle_high/*.fbx` for the 7 pass slots (ignored path); `raw_assets/MESHY-ENV-PREP/carve_report.json` now holds the strict run.
- **Not verified:** UE import, in-game clearance and feel, Android. Lateral and vertical axis mapping unchecked in UE.
