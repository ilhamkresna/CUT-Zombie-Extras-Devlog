# 2026-11-15 15:30 WIB — S2-G2 — pilot skin characters MummyStunt + DinoSuit (Meshy t2.5 + rig, rig key-pose sheets)

- **Who / tool:** Worker S2-G2, Claude Haiku 5.5 (claude-haiku-5-5) in Claude Code; Meshy REST API via `tools/meshy/hero_skin_chars_20261115.py` (Meshy API key from the user environment, value never printed); Blender 5.1.1 CLI for bone check and renders via `tools/skins/check_skin_rig.py`.
- **Task:** S2 daily economy pilot, skin characters (tasks/S2-DAILY-ECONOMY.md "Owner 15:50" rig paragraph) · **Branch / commit:** main / (see commit line below)
- **Asked for:** Concepts MummyStunt and DinoSuit turned into rigged skeletal meshes on the hero skeleton that play the hero clips without rig bugs; Meshy spend cap 60 credits; key-pose contact sheets; Unreal import only for a clean sheet.
- **Result:** Both characters generated and rigged in Meshy (40 credits, 2 tasks each). Bone check done. Both key-pose sheets written. Verdict below: neither sheet is clean, so NO Unreal import, no SkeletalMesh assets, no asset_manifest rows, and no `import_skin_chars_20261115.py` were written.
- **Verified by:** Blender CLI bone check (names, hierarchy, lengths, rest pose) and Blender CLI key-pose renders, read by eye at 256 px per pose. Not verified in Unreal and not on device.
- **What went wrong:** (1) First Meshy create was rejected with HTTP 400 `ai_model meshy-t2.5 requires model_type: smart-topology` (not charged). Fixed to `model_type: smart-topology` and resubmitted once. (2) First Blender pose render wrote PNGs to a stray folder `C:/raw_assets/HERO-SKINS/chars/HERO/poses` because render paths were relative; script fixed to absolute paths and the 12 files moved into the repo. (3) Pose application did nothing at first: Blender 5 needs `action_slot` set when an action is assigned from Python; fixed in the script. (4) The shell safety check blocked one `rm -r "C:/raw_assets"` cleanup; the stray folder is still there (it holds only the 12 HERO test renders of this session). Needs a manual delete.
- **Corrected by:** self (all four items)
- **Human decision needed or taken:** Orchestrator decision needed: MummyStunt and DinoSuit both fail the sheet check as described below. Options: accept MummyStunt as a candidate after a closer look, re-prompt the concept to a T-pose, or drop. Translation retargeting (see bone check) is the likely fix for DinoSuit.
- **Not done / not verified:** Unreal import (not done: sheets not clean). Retry on either skin (not used; no task marked broken). In-game look, animation timing and frame-rate are not verified. Pose sheets are one extreme frame per clip, from the raw hero clip rotations applied to the new armature (same method the shared skeleton would use in UE, with no retarget).
- **Screenshot:** Devlog/media/20261115_S2_rig_HERO.png, Devlog/media/20261115_S2_rig_MummyStunt.png, Devlog/media/20261115_S2_rig_DinoSuit.png

## Meshy task ids and credits (script ledger: raw_assets/HERO-SKINS/meshy/chars_ledger.json, ignored)
- Balance before 1015, after 975. Spent 40 (planned 40, cap 60). No retry.
- MummyStunt: image-to-3d `01a1de4c-3422-76ef-bf0f-ed35ea0d0e5e` (SUCCEEDED, 15 cr: meshy-t2.5 smart topology, target 12000, meshy-n1 2K, pose_mode a-pose sent); rig `01a1de4d-1d84-715d-b94a-9295ec9f8a71` (SUCCEEDED, 5 cr, height 1.8 m).
- DinoSuit: image-to-3d `01a1de4d-5f4b-779a-9c07-23fa276e5b15` (SUCCEEDED, 15 cr); rig `01a1de4e-4a76-7544-8749-a2afda87c49b` (SUCCEEDED, 5 cr).
- Downloads (not in git): raw_assets/HERO-SKINS/chars/<SkinId>/ (rigged.glb, rigged.fbx, mesh/ raw model and 2K base colour).
- Hero height for the rig: 1.8 m (char1 mesh of raw_assets/HERO-01/benchmark/rig.glb, measured in Blender, 0 to 1.8 m; the rig file also contains a 2 m Cube and an Icosphere, not used).

## Bone check vs hero rig.glb (Blender, world scale, both 24 bones)
- Names: equal (24 of 24). Parent hierarchy: equal. Missing or extra bones: none.
- MummyStunt: length ratio new/hero 0.957 to 1.285 (largest deviation headfront, a tiny bone, 28.5%; Head 1.285). Limb ratios over the 8% threshold: LeftArm 1.22, RightArm 1.115, LeftShoulder 1.121, RightShoulder 1.112, LeftFoot 1.086. Max rest-direction angle 57.5 deg (RightHand); max head-position difference 0.35 m. Mesh height 1.80 m.
- DinoSuit: length ratio 0.713 to 2.881. Spine 2.881 (188%), Spine01 and Spine02 1.364, LeftShoulder 1.33, RightShoulder 1.358, LeftLeg 0.713, RightLeg 0.743, LeftUpLeg 0.832, RightUpLeg 0.833, neck 0.763. Max rest-direction angle 60.2 deg (LeftHand); max head-position difference 0.45 m. Mesh height 1.80 m.
- Interpretation: the hero rig is T-pose. pose_mode a-pose was sent to Meshy, but the rest pose it returned was not checked separately; the measured rest-direction offsets (up to about 60 deg) are the number to go by. Shared-skeleton playback of raw hero rotations carries that offset into the pose.

## Key-pose sheet verdicts (4 columns, 256 px per pose, Workbench, front 3/4, ground plane)
Poses rendered from the hero clip GLBs: Idle, Run (clip_run_fast_10_inplace), Jump (clip_Regular_Jump), Backflip (for FrontFlip, no front flip GLB), Slide (clip_sliding_stumble), Roll (clip_Roll_Dodge), PunchJab, PunchHook, PunchUppercut, HitReact, Knockdown, Victory (clip_Victory_Cheer). Missing clips: RunLeap, Land, FightIdle, a separate Stumble, FrontFlip. Frame = max deviation from the clip's mean bone pose, chosen on the clip's own armature.
- **MummyStunt: NOT clean.** Idle, Jump, Backflip, Roll, Knockdown, Victory read as clean at this size. Arm poses look wrong in PunchJab (arm up toward the head), HitReact (forearm off-axis) and Run (arm swing stiff); consistent with the 57 deg A-pose rest offset on the arms. Bandage ends not verified at this resolution. Slide reads as a lying body, same as the hero. Not imported.
- **DinoSuit: NOT clean.** Body is elongated (spine 2.9x) and legs short (0.71 to 0.83x). Slide and PunchUppercut fold the body onto the floor, Roll is cut by the frame edge, Knockdown floats with the tail over the body. Tail and big head: the tail is not a bone chain in this rig, so it cannot follow the clip, and in Idle it lies on the body. Not imported.
- **Hero reference:** clean at this size in all 12 poses (Devlog/media/20261115_S2_rig_HERO.png), so the test itself is valid.
- Sheet readings are by eye at 256 px; a closer pass may change MummyStunt's verdict.

## Commit
- Files: tools/meshy/hero_skin_chars_20261115.py, tools/skins/check_skin_rig.py, Devlog/media/20261115_S2_rig_HERO.png, _MummyStunt.png, _DinoSuit.png, this entry. No `Content/` assets, no tools/ue script, no asset_manifest rows (no import).
- Config/DefaultEngine.ini and other workers' modified files were not staged.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 15:21 | Meshy create image-to-3d MummyStunt | meshy-t2.5 (no model_type), meshy-n1, a-pose, Meshy API key from the user environment | HTTP 400 (model_type smart-topology required), 0 credits | rejected, not charged |
| 15:24 | Meshy create image-to-3d MummyStunt | model_type smart-topology, meshy-t2.5, meshy-n1 2K, target 12000, a-pose; task 01a1de4c-3422-76ef-bf0f-ed35ea0d0e5e | SUCCEEDED, 15 cr; files to raw_assets/HERO-SKINS/chars/MummyStunt/mesh/ | done |
| 15:25 | Meshy rig MummyStunt | rigging, height 1.8, task 01a1de4d-1d84-715d-b94a-9295ec9f8a71 | SUCCEEDED, 5 cr; rigged.glb, rigged.fbx | done |
| 15:26 | Meshy image-to-3d DinoSuit | task 01a1de4d-5f4b-779a-9c07-23fa276e5b15 | SUCCEEDED, 15 cr | done |
| 15:27 | Meshy rig DinoSuit | task 01a1de4e-4a76-7544-8749-a2afda87c49b | SUCCEEDED, 5 cr; total spent 40 | done |
| 15:23-15:45 | Blender CLI: hero pose renders and bone checks (tools/skins/check_skin_rig.py) | blender 5.1.1 -b | renders for HERO, MummyStunt, DinoSuit; JSON in raw_assets/HERO-SKINS/chars/<SkinId>/rig_check.json | verified by sheet read at 256 px |
| 15:45 | Blender stray output fix | relative render path wrote to C:/raw_assets/... | 12 files moved into repo; stray folder remains (delete blocked by shell safety check) | pending human delete of C:/raw_assets |
