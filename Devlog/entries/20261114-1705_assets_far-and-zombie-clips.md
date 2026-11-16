# 2026-11-14 17:05 WIB — assets — far landmarks, L5 density, zombie crowd clips

- **Who / tool:** worker agent, Claude Haiku 5.5 in Claude Code; UnrealEditor-Cmd commandlets (pythonscript) and UnrealEditor.exe -game for screenshots; PowerShell PrintWindow capture.
- **Task:** batch 11 far assets, L5 landmark density, zombie crowd clips (orchestrator packet) · **Branch / commit:** main, commit hash in git log (see commit with this entry)
- **Asked for:** import batch 11 far assets (excluding far_pine_ridge) into the L5 landmark pools, make L5 denser, import 7 zombie crowd clips and set DA_AnimSet_Zombie slots, capture three game screenshots and describe them honestly.
- **Result:**
  - Imported 17 far assets into /Game/Art/Env/Meshy/Far_landmark and /Far_scenery (SM_, T_, MI_ each), 0 failed. far_pine_ridge excluded in import and populate.
  - DA_ArtSet_046 landmark pools after populate: Town 22, WildWest 25, Monster 26, SciFi 26 entries.
  - L5 tunables set on the dressing layout: LandmarkChance 0.75 (was 0.35), LandmarkMinY 4000, LandmarkMaxY 9000. Readback in tune report matches. Tune run moved 40 tall clutter pieces to L2.
  - 7 zombie clips imported as AnimSequences on SK_Zombie_Preview_Skeleton: /Game/Art/Characters/ZombiePreview/Anims/A_Zombie_{HordeRunA, HordeRunB, CrowdCheer, CrowdClap, CrowdOoh, CrowdJumpCheer, CrowdMob}. Existing assets were not overwritten (none existed). DA_AnimSet_Zombie now has 20 clip entries.
  - Screenshots: Devlog/media/20261114_task064_v2_1.png, _v2_2.png, _v2_3.png (540x960 windowed, Jam.God 1 + Jam.Start, 60 s shader wait, captures 5 s apart).
- **Verified by:** import commandlet exit 0 with reports (ue_import_report.json, populate and tune reports, ue_crowd_clip_import_report.json: 7 imported, 0 failed); screenshots only. Not compiled (no C++ change). Not played in PIE, not on device.
- **What went wrong:**
  - First screenshot attempt failed: `Add-Type -ReferenceAssembly` is not valid in Windows PowerShell 5.1, so no PNG was written. Fixed to `-ReferencedAssemblies` and recaptured; the second run wrote all three PNGs.
  - HordeRunB clip length is 0.733 s, versus about 3.97 s for the other six clips. Possibly truncated or a short take in the source. Not investigated.
  - The populate script had no mapping for the `far_landmark` category, so the far assets would have been skipped silently. Added the mapping.
- **Corrected by:** self (script edits listed above, all in tools/ue/).
- **Human decision needed or taken:** none. far_pine_ridge exclusion was set by the orchestrator packet.
- **Not done / not verified:**
  - Did not run Jam.Duel, so no duel screenshot.
  - Did not verify that the zombie clips animate in-game; the stills show a near-identical pose in frames 2 and 3.
  - Did not verify that the L5 skyline is continuous in-game; only three frames of the street at ground level were captured.
  - Did not verify that costumes are mixed.
  - Did not check the HordeRunB clip beyond its length.
  - Did not test on Android.
- **Screenshot:** Devlog/media/20261114_task064_v2_1.png, Devlog/media/20261114_task064_v2_2.png, Devlog/media/20261114_task064_v2_3.png

**Screenshot description (honest, from the three stills):**
- Not black: the game renders, HUD shows "DEMO SCENE 1 · TAKE 1", "x1", "SWIPE UP!" / "TAP!".
- Frame 1 is motion-blurred (running). Frames 2 and 3 are nearly identical.
- Zombies: dark silhouettes are visible in the lane, one mid-lane and one in the foreground. The figures read as dark, flat silhouettes rather than the textured variants. No zombies were clearly visible on the sidewalks in these frames.
- Costumes: cannot be judged from these frames; all visible zombies are dark.
- Animation: cannot be judged from stills.
- Horizon: buildings and a dark band at the top of the frame. Far landmarks are not clearly identifiable as a continuous skyline in these frames.
- Cylinders: no obvious cylinders in the frames; a round object in the lane is a pickup/reel or prop, not a zombie.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 17:00 | Edit tools/ue/import_meshy_env.py (EXCLUDE far_pine_ridge) | Claude Code Edit | 5 lines | compiled n/a; run ok |
| 17:01 | UnrealEditor-Cmd import_meshy_env.py | tools/ue, nullrhi | 17 imported, 215 skipped, 0 failed | exit 0 |
| 17:01 | Edit populate_task062_dressing.py (far_landmark map, reject pine ridge); tune_task062_composition.py (landmark tunables) | Claude Code Edit | - | run ok |
| 17:01 | UnrealEditor-Cmd populate_task062_dressing.py | nullrhi | landmarks Town 22, WildWest 25, Monster 26, SciFi 26 | exit 0 |
| 17:01 | UnrealEditor-Cmd tune_task062_composition.py | nullrhi | L5 chance 0.75, Y 4000-9000; 40 clutter to L2 | exit 0 |
| 17:02 | New tools/ue/import_zombie_crowd_clips_20261114.py | nullrhi | 7 AnimSequences, DA 20 entries | exit 0, 0 failed |
| 17:04-17:05 | UnrealEditor.exe -game portrait, PrintWindow x3 (first attempt failed: Add-Type param) | Jam.God 1,Jam.Start | Devlog/media/20261114_task064_v2_{1,2,3}.png | screenshots only; game stopped 17:05:44 |
