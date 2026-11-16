# 2026-11-15 15:46 WIB — S2-G3 — skin characters batch 2 (RobotExtra, HotDogMascot, SpaceAce, GoldenDirector): Meshy t2.5 + rig, bone check, rest-pose stills

- **Who / tool:** Worker S2-G3, Claude Haiku 5.5 (claude-haiku-5-5) in Claude Code; Meshy REST API via `tools/meshy/hero_skin_chars_20261115.py` (Meshy API key from the user environment, value never printed); Blender 5.1.1 CLI for bone check (`tools/skins/check_skin_rig.py`, part a only) and rest-pose stills.
- **Task:** S2 daily economy, skin characters batch 2 (follow-up to S2-G2 pilot aaa922c) · **Branch / commit:** main / (see commit line below)
- **Asked for:** Concepts RobotExtra, HotDogMascot, SpaceAce, GoldenDirector turned into rigged skeletal meshes on the hero skeleton (same Meshy settings and rig height as pilot), Meshy spend hard cap 100 credits, bone check, one front 3/4 rest-pose still each, a 4-up sheet, and a match-to-concept verdict per character.
- **Result:** All four generated and rigged in Meshy (8 tasks, all SUCCEEDED, 80 credits). Rigged GLB + FBX + mesh and 2K base colour in raw_assets/HERO-SKINS/chars/<SkinId>/ (not in git). Bone check: names and hierarchy equal to the hero rig for all four. Stills written to raw_assets/HERO-SKINS/icons/T_SkinIcon_<SkinId>.png and the 4-up sheet to Devlog/media/20261115_S2_chars_batch2.png. No Unreal import, no SkeletalMesh, no asset_manifest rows.
- **Verified by:** Blender CLI bone check (numbers below) and the rest-pose stills read by eye at 512 px against the concept PNGs. Nothing in Unreal, nothing on device.
- **What went wrong:** (1) Script CAP was 60 in the pilot ledger (40 spent), so the first run would have stopped at the third create. Changed CAP to 140 (pilot 40 + batch cap 100), the only script edit. (2) First still render used Workbench with no light: silhouettes only. Second pass had EEVEE but still no light (factory-empty scene), black. Third pass added a sun and fill light and a grey world: colour readable. (3) Still framing took the bounding box of unskinned meshes (reported height 2.8 m) instead of the 1.8 m skinned height; framing fixed with a fixed 2.3 m ortho box centred at 0.92 m. Stills are only for the verdict, not game art.
- **Corrected by:** self (all three items).
- **Human decision needed or taken:** none taken. Human decision needed: whether the four go to the Unreal import step, given the pilot's two skins failed the pose sheet (see S2-G2 entry). Retry: not used (no task marked broken).
- **Not done / not verified:** Pose sheet (part b of check_skin_rig.py) skipped as instructed. Unreal import, in-game rig and animation playback, FBX content, and texture quality at game scale are not verified. Stills are rest pose only.

## Meshy task ids and credits (script ledger: raw_assets/HERO-SKINS/meshy/chars_ledger.json, ignored)
- Balance: 975 before this batch (pilot ended there), 895 after. Batch spent 80 (4 x 15 image + 4 x 5 rig), inside the 100 hard cap. Ledger-wide spent 120 (pilot 40 + batch 80). No retry.
- RobotExtra: image-to-3d 01a1de5b-247d-763c-9382-b031ec878565 (SUCCEEDED, 15 cr); rig 01a1de5c-34ee-72b3-b0ca-bdcc33f3beba (SUCCEEDED, 5 cr, height 1.8 m).
- HotDogMascot: image-to-3d 01a1de5c-8518-7450-a1de-5ca2ebf4a59c (SUCCEEDED, 15 cr); rig 01a1de5d-4367-77d1-8dbd-101719e01793 (SUCCEEDED, 5 cr).
- SpaceAce: image-to-3d 01a1de5d-840e-70c5-a8d3-7cdf6cb49277 (SUCCEEDED, 15 cr); rig 01a1de5e-72d4-704c-9a4f-2a7184305fc7 (SUCCEEDED, 5 cr).
- GoldenDirector: image-to-3d 01a1de5e-b590-7229-a22d-65365f463ed8 (SUCCEEDED, 15 cr); rig 01a1de5f-9c7e-713a-b11c-20d922265ffc (SUCCEEDED, 5 cr).

## Bone check vs hero rig.glb (Blender, world scale, 24 bones each; part a only)
- Names: 24 of 24 equal for all four. Parent hierarchy equal. Missing or extra bones: none. New mesh height 1.80 m for all four.
- RobotExtra: length ratio 0.753 to 1.446 (largest RightShoulder 1.446, 44.6%). Spine01/02 0.753, Head 1.35. Max rest-direction angle 70.2 deg (RightHand). Max head-position difference 0.53 m.
- HotDogMascot: length ratio 0.648 to 1.899 (largest RightShoulder 1.899, 89.9%). LeftLeg 0.648, RightLeg 0.676, Spine 1.72. Max rest-direction angle 90.3 deg (RightHand). Max head-position difference 0.41 m.
- SpaceAce: length ratio 0.883 to 1.417 (largest headfront 41.7%; Head 1.417). RightShoulder 1.309, LeftShoulder 1.294. Max rest-direction angle 68.4 deg (RightHand). Max head-position difference 0.44 m.
- GoldenDirector: length ratio 0.235 to 1.182 (largest Head 0.235, 76.5%: head bone far shorter than hero; check before any use). Max rest-direction angle 77.1 deg (RightHand). Max head-position difference 0.46 m.
- Reading: the pilot's MummyStunt/DinoSuit showed the same kind of rest-direction offset (up to about 60 deg, from the A-pose rest). The 70 to 90 deg offsets here are larger, so shared-skeleton playback is expected to carry more pose error. Not confirmed in Unreal.

## Rest-pose verdicts (front 3/4, EEVEE, 512 px, read by eye vs concept)
- **RobotExtra: matches.** Boxy silver head with visor face and twin antennas, chest panel with dials and red/blue buttons, ribbed tube arms and legs, boxy boots. Shape and colours agree with the concept.
- **HotDogMascot: matches.** Bun body, red sausage with mustard squiggle, face in the bun opening, white gloves, red sneakers. Shape and colours agree.
- **SpaceAce: matches.** Clear dome helmet with dark hair, red suit with silver chest and knee panels, silver boots and gloves. Concept's small back piece is present. Shape and colours agree.
- **GoldenDirector: matches.** Black beret, round glasses, moustache, gold jacket, camera on dark roll-neck, tan trousers, brown boots. Shape and colours agree.
- No missing limbs, no text on any model. Limb-length deviations in the bone check (above) are the thing to check in Unreal, not visible in the stills.

## Files
- Written (not in git): raw_assets/HERO-SKINS/chars/<SkinId>/ (rigged.glb, rigged.fbx, mesh/, rig_check.json, batch2_blender_check.log, batch2_still.log); raw_assets/HERO-SKINS/icons/T_SkinIcon_<SkinId>.png (4).
- Committed: this entry, Devlog/media/20261115_S2_chars_batch2.png, tools/meshy/hero_skin_chars_20261115.py (CAP 60 to 140).
- Render helper lives outside the repo (temp folder), not committed.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| ~15:30-15:46 (approx.) | Script CAP 60 to 140 | tools/meshy/hero_skin_chars_20261115.py | ledger-wide cap for batch 2 | done |
| (same session) | Meshy batch create + rig (4 skins) | meshy-t2.5 smart topology, target 12000, meshy-n1 2K, a-pose, rig 1.8 m, Meshy API key from the user environment; ids in entry | 8 tasks SUCCEEDED, 80 cr, balance 975 to 895 | done (Meshy side); Unreal pending |
| (same session) | Blender bone check part a, 4 skins | tools/skins/check_skin_rig.py (no --poses) | rig_check.json per skin, 24/24 names | verified numbers |
| (same session) | Rest-pose stills (3 passes: Workbench, EEVEE no light, EEVEE with light) | render helper outside repo, Blender 5.1.1 -b | T_SkinIcon_<SkinId>.png, 4-up sheet | verified by eye, final pass |
