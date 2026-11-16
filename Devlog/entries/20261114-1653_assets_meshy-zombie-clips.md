# 2026-11-14 16:53 WIB — assets — Meshy zombie crowd clips (zombie_clips_20261114)

- **Who / tool:** Meshy worker, Claude Haiku 5.5 in Claude Code; Python stdlib script calling the Meshy REST API; Blender 5.1 CLI for the bone check
- **Task:** Meshy zombie clips 2026-11-14 · **Branch / commit:** main / see commit
- **Asked for:** Adapt tools/meshy/regen_20261114.py into zombie_clips_20261114.py (--key-var option, own ledger and downloads folder, balance gate 95, cap 90), then rig the zombie benchmark source, apply one library action and six text-to-motion clips, download per slot, and bone-check against fight_idle.
- **Result:** Script tools/meshy/zombie_clips_20261114.py written. Rig task succeeded. Library action horde_run_b (action 659 "Run Fast 3") applied. Six text-to-motion clips (prime, 4 s) generated and applied to the rig. Applied FBX per slot in raw_assets/MESHY-ZMBCLIPS-20261114/zombie/ (horde_run_a, horde_run_b, crowd_cheer, crowd_clap, crowd_ooh, crowd_jumpcheer, crowd_mob). Raw motion FBX in raw_motion/, rigged character FBX in rig/. Ledger tasks.json in the same folder (ignored by git).
- **Verified by:** Blender CLI bone check (automation) — all 7 applied FBX files have 24 bones with the same names as raw_assets/MESHY-REGEN-20261114/zombie/fight_idle.fbx (no missing, no extra). Not compiled or run in Unreal (not in scope).
- **What went wrong:** Credit accounting mismatch. Script ledger counted 86 credits (rig 5, library animation 3, six text-to-motion at 10 = 60, six applies at 3 = 18). Meshy balance went 980 before to 844 after, a drop of 136 credits, which is 50 more than the script ledger. The cause is not yet explained; the script does not parse per-task costs. Cap of 90 was enforced on the script ledger only, not on the observed balance drop, so the real spend exceeded the 90 cap by 46 credits. Nothing else failed.
- **Corrected by:** not corrected yet; flagged to the orchestrator.
- **Human decision needed or taken:** Owner needs to review the 136-credit observed drop against the approved 90-credit cap. Decision needed on whether to accept the overage, and whether to check Meshy usage (meshy_list_usage, free) to reconcile per-task charges.
- **Not done / not verified:** Per-task Meshy charges not reconciled. Clips not previewed visually or played in Unreal. Motion quality and loop seams not checked. Not imported into the Unreal project. asset_manifest.csv not updated.
- **Screenshot:** none

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 16:53 (approx., run log) | Edit (new script) | tools/meshy/zombie_clips_20261114.py, adapted from regen_20261114.py | Script written; key read from registry value named by --key-var | Run done |
| run | Balance check (free) | Meshy balance endpoint via script, Meshy API key from the user environment | balance before 980 | Done |
| run | Rig (5 cr) | rigging on raw_assets/ZMB-01/benchmark/rig.glb, height 1.9 m | rig task SUCCEEDED; rigged FBX in rig/ | Done |
| run | Library animation (3 cr) | action_id 659 "Run Fast 3" → slot horde_run_b | SUCCEEDED; zombie/horde_run_b.fbx | Done |
| run | Text-to-motion, prime, 4 s (10 cr each, 6 clips) | prompts verbatim in script MOTION_SLOTS | all 6 SUCCEEDED; raw in raw_motion/ | Done |
| run | Apply motion to rig (3 cr each, 6 clips) | rig_task_id + motion_task_id | all 6 SUCCEEDED; zombie/<slot>.fbx | Done |
| run | Balance check (free) | Meshy balance endpoint | balance after 844 (drop 136; script ledger 86) | Discrepancy flagged |
| after run | Blender CLI bone check (automation) | check_bones.py run from scratchpad, reference fight_idle.fbx | 7 of 7 files: 24 bones, names match | Verified (bone names only) |

## Prompts (verbatim, owner brief)
- horde_run_a (text-to-motion): "A cartoon zombie extra running clumsily forward in place, arms stretched straight out in front, stiff legs, head wobbling side to side, goofy, seamless loop."
- crowd_cheer: "A cartoon zombie extra cheering in place, both arms raised and pumping in the air, bouncing on the toes, goofy and happy, seamless loop."
- crowd_clap: "A cartoon zombie extra clapping hands excitedly in place while bobbing the head, goofy, seamless loop."
- crowd_ooh: "A cartoon zombie extra gasping in surprise in place, hands on cheeks and leaning back, then settling, goofy, seamless loop."
- crowd_jumpcheer: "A cartoon zombie extra jumping up and down excitedly in place with fists raised, goofy, seamless loop."
- crowd_mob: "A cartoon zombie extra reaching forward and grabbing with both arms in place, hungry and goofy, seamless loop."

No keys, account identifiers, signed URLs or private perk codes. Log self-edits are covered by this entry; do not recurse.

## Addendum 2026-11-14 - Meshy B11 far scenery prep (asset-prep worker)

- **Who / tool:** asset-prep worker, Claude Haiku 5.5 in Claude Code; Blender 5.1 CLI (`prep_meshy_env.py`, `contact_sheet.py`); python for the manifest edit. No Meshy API calls made.
- **Asked for:** add the 18 B11 far slots (`raw_assets/MESHY-FAR-B11-20261114`, brief `tools/meshy/far_batch11_20261114.json`) to `tools/assets/meshy_env_manifest.json`, prep them, one contact sheet, list rejects, log credits.
- **Result:**
  - Manifest: 214 to 232 slots. 13 `far_landmark` (height fits: 40 m for hotel tower, radio mast, castle, launch tower, dome city, west water tower; 35 m ferris wheel; 30 m soundstage rows A/B, office, apartment, windmill; 20 m gas tank). 5 `far_scenery` (length 120 m). Themes: `far_west_*` WildWest, `far_monster_*` MonsterMovie, `far_scifi_*` RetroSciFi, others Shared.
  - Brief gave no height for `far_monster_windmill` or `far_scifi_dome_city`; 30 m and 40 m were chosen within the 25-45 m band.
  - Prep: `--only` run on the 18 slots, 18 ok, 0 failed, 0 pending. FBX in `raw_assets/MESHY-ENV-PREP/far_landmark/` and `far_scenery/` (ignored by git).
  - Script change: `prep_meshy_env.py` gained `min_axis_ratio` (reject when the smallest bbox axis is below the given fraction of the longest). Set to 0.01 on the far_scenery rows. `max_factor` 250 on all 18 rows. No row was rejected by the guard.
  - Contact sheet: `Devlog/media/20261114_meshy_b11_far_contact.png` (18 cells, raw GLBs).
- **Verified by:** Blender prep run (exit 0, per-slot bbox and scale factor in the run output) and agent review of the contact sheet. Not verified in UE.
- **Rejects and flags (contact sheet):**
  - REJECT `far_pine_ridge`: reads as two separate pine trees, not a ridge. Marked `review` in the manifest.
  - FLAG `far_west_mesa_wide`: reads as a rounded rock blob, not a flat-top layered mesa. Marked `review`.
  - Other 16 read as intended. `far_monster_windmill` sits on a dark square plinth, which may need a check in UE.
- **Credits (from the brief, not re-checked):** B11 270; zombie clips 86; Meshy key6 balance now 744.
- **Not done / not verified:** no UE import, no FBX axis or pivot check in UE, no placement in the run scene. `asset_manifest.csv` not updated. Prep report `failed: 2` in the counts comes from earlier batches, not this one.
