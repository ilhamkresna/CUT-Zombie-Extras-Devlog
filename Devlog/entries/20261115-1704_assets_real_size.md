# 2026-11-15 17:04 WIB — assets — absolute real-size table applied in Blender (no Unreal)

- **Who / tool:** sizing worker, Claude Haiku 5.5 in Claude Code; Blender 5.1.1 headless (`blender -b --factory-startup -P`), numpy 2.3.4 bundled.
- **Task:** Orchestrator packet for absolute sizes (`tools/assets/real_size_table.json`, binding), Blender only · **Branch / commit:** main, see the commit that adds this entry.
- **Asked for:** Apply the table to every non-excluded Meshy env slot (uniform scale, bottom-centre pivot, obstacles width-fitted to 250/270/285 cm with band checks), export FBX + base colour PNG, gate re-measure at +-5 %, write `real_size_report.json`, render scale sheets per group with a 180 cm figure and 300 cm lane bar.
- **Result:** `tools/blender/apply_real_size.py` (modes `build` and `sheets`). Build: 256 slots processed (table excluded 25 not processed), **228 pass / 28 fail**, gate re-measure passes all 228. Exports in `raw_assets/MESHY-ENV-PREP-REAL/<use>/` (git-ignored, not committed). Per-use pass/fail: far 28/0, L4 13/0, L2 13/0, L2_cluster 16/0, L2_edge 7/0, L2_vehicle 15/0, L3 35/0, L0 22/0, L1 30/0, pickup 1/0, roof 6/0, intro 4/0, garage 3/0, hazard 10/0, setpiece 18/0, obs_low 3/14, obs_block 4/7, obs_high 0/7. Sheets: `Devlog/media/20261115_scale_*.png` (29 files; no obstacles_high sheet because 0 obs_high assets passed).
- **Verified by:** automation (Blender re-import of every exported FBX, +-5 % gate on controlling dimension / obstacle width and band) and visual review of all 29 scale sheets against the 180 cm figure. Not verified in Unreal (not imported, per instruction).
- **What went wrong:** (1) First build run hit "Image does not have any image data" for bus3 x2 and sp_airliner CentreHole; fixed with a source-folder PNG fallback and re-ran. (2) First sheet render showed only the last asset per sheet because the import helper deleted other scene objects and returned all scene meshes; fixed (per-import object tracking) and re-rendered. Both fixes are in the committed script; the report above comes from the second build run, which ran after fix (1).
- **Corrected by:** self (script fixes above, both re-runs logged here).
- **Human decision needed or taken:** Obstacle pools are thin. obs_low has only 3 usable variants (hurdle_sandbags, monster_tombstones, west_trough), obs_high has none. Owner or orchestrator must decide whether to relax the obs_high clearance / obs_low hmax rules or re-generate those meshes. Not decided here.
- **Not done / not verified:**
  - Not imported into Unreal; no in-engine check.
  - Wrong-looking on sheets (relative to 180 cm figure): `obs_block_monster_gate` (W 32 cm, a thin panel reading as a sheet rather than a blocking gate, passes band); `obs_block_prop_wall` (W 73 cm, thin wall, passes band). Other assets looked plausible for their real size.
  - Band failures (table excluded, not in pool): obs_low sandbags/tire stack/bone pile/pumpkins/hay bale/coffin/crater/pipes/oil drums/pallets/canisters/cones/barrels/troughs at heights outside 60..110 cm after width fit; obs_block crate tower, lighting cart, portable toilet, scifi pod, ticket booth (too tall) and coffin cart, west wagon back (too short); obs_high banner bar, boom arm, light rig, pipe frame, laser gate, west beam (clearance < 165 cm), sign arrow board (clearance 0).
  - `bus3_*` uses the existing `_FrontOpen` cut variant (the table gives only length, scale 0.92). The `_RearOpen` variant was not evaluated.
  - obs_high source rule: ENV-PREP carved fbx used where present (`sign_arrow_board` has no carved variant, used GLB).
  - The table `h` values for obstacle slots are informational only (obstacle_fit rules used instead, per the orchestrator).
  - The report and exports live under `raw_assets/` (git-ignored); the report is force-added to git as requested by the orchestrator.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| step 1 | Wrote `tools/blender/apply_real_size.py` | Claude Haiku 5.5, Write tool | build + sheets modes | written |
| step 2 | Build run 1 (Blender 5.1.1 headless) | `apply_real_size.py --mode build` | 225 pass / 31 fail; 3 image-data exceptions | superseded |
| step 3 | Script fix: image fallback | Edit tool | `export_slot` falls back to source PNG | fixed |
| step 4 | Build run 2 + sheet render 1 | `--mode build`, `--mode sheets` | 228 pass / 28 fail; sheets had object-bookkeeping bug | sheets wrong |
| step 5 | Script fix: per-import object tracking; sheet re-render | Edit tool, `--mode sheets` | 29 PNGs in `Devlog/media/20261115_scale_*.png` | sheets reviewed visually, see Not done |

## Addendum: obstacle rerun under obstacle_fit v2 (same session)
- Orchestrator rewrote `obstacle_fit` (commit b1343fc): obs_low/obs_block keep real size from slot `h` (Low band 55..100, Block 200..340); obs_high height-fit to `h`, then clearance >= 165 cm inside |Y|<=115, width(Y) 250..450. Width-fit rule removed.
- Rerun: `apply_real_size.py --mode build --only obstacles` (dressing records kept). Obstacle result: obs_low 17/17 pass, obs_block 10/11 pass (monster_coffin_cart h 140 fails band), obs_high 0/7 pass (lowest geometry in the centre slab is 0-1.4 cm above floor: posts/legs at lane centre; banner_bar, boom_arm, light_rig, pipe_frame, laser_gate, west_beam, sign_arrow_board). Total report 248 pass / 8 fail, all 248 pass re-measured by the gate.
- Sheets: `20261115_scale_obstacles_low_a.png`, `_low_b.png` (each with a cluster-preview row: ground_sandbags + ground_tire_stack + monster_bone_pile stacked along Y, span ~243 cm, meets the >=230 cm rule but is short of the 300 cm bar), `20261115_scale_obstacles_block.png`. No obstacles_high sheet: zero obs_high assets pass.
- Looked at on the sheets: obs_block_monster_gate (D 30 cm) and obs_block_prop_wall (D 80 cm) read as thin panels against the 180 cm figure, still passing band; obs_low sizes look plausible vs the figure.
- Not verified in Unreal (not imported).

## Addendum 2: obs_high redone with strict carve on the height-fitted mesh
- Cause of the earlier 0-1.4 cm clearance: the ENV-PREP obstacle_high FBX carries centre-mode carve only, so faces with a vertex at floor level in |Y|<=115 survived. I did not run `carve_slide_gap.py --strict` (it would rebuild from raw GLB at 3.8 m and overwrite the prep folder); instead the build height-fits each carved FBX to slot h, then applies the strict rule (remove every face with any vertex in |Y|<=1.15 and Z<1.70 m), re-measures, and gates.
- Results: pass banner_bar (clear 201 cm, W 89 D 280), boom_arm (clear 171, D 265), light_rig (clear 263, D 280), scifi_laser_gate (clear 311, D 265), west_beam (clear 170, D 265). Fail: pipe_frame, rejected on the sheet (floating fragments, unrecognisable). sign_arrow_board excluded (no carve). clothesline excluded (table).
- Sheet: `Devlog/media/20261115_scale_obstacles_high.png` reviewed; boom_arm and light_rig read as their props, west_beam is fragmented but recognisable.
- The VISUAL_REJECT set in the script records the pipe_frame rejection.
- Report totals now 254 pass / 2 fail (2 fails = sign_arrow_board excluded, obs_block monster_coffin_cart band).
