# 2026-11-15 00:19 WIB — assets — TASK-073 strict dressing rules (code only, not compiled)

- **Who / tool:** code worker, Claude Haiku 5.5 in Claude Code; file edits and Python 3 dry-run only.
- **Task:** TASK-073 (`tasks/TASK-073.dressing-rules.md`) · **Branch / commit:** main, see the commit that adds this entry.
- **Asked for:** Replace the random dressing with explicit rule rows (absolute real size, layer bands, densities, footprint overlap, hazard corridor reservation, cluster presets, obstacle cluster helpers), a data generator for `DA_DressingRules`, and `JamGame.Dressing.Rules*` tests. Code only; no Unreal or UBT run until the orchestrator says go.
- **Result:**
  - `Source/JamGame/Art/JamDressingRules.h/.cpp`: `UJamDressingRules` (rows, cluster presets, `FJamDressingBands` with the packet table defaults), enums, and the pure `JamDressingRules::PlaceSegment` (facades via `JamDressingPlan::PackRow`, L4, L2 one item or one cluster per side, L2_edge fence at the strip edge with the 200 cm facade clearance, poles on a 1600 cm rhythm, clutter, L0 ground cover, 30 cm padded overlap test, corridor skip, |Y| >= 450 check). Also `ObstacleLowClusterOffsets` (2 to 4 items, span >= 230 cm) and `NeedsBlockPair`.
  - `Source/JamGame/Track/JamTrackSegment.h/.cpp`: rule pool (24 components per side, created at warm-up); when `UJamArtSet::DressingRules` is set the segment dresses only from its rows and ignores `DressingSets`. `SetDressingCorridors` reserves corridors for the next Configure.
  - `Source/JamGame/Art/JamArtSet.h`: `DressingRules` pointer.
  - `tools/ue/make_dressing_rules.py`: builds DA_DressingRules from `real_size_table.json` (skips `excluded`, maps `use` to layer, SM_<PascalSlot> lookup under /Game/Art/Env/Meshy, idempotent create/load). `--dry-run` run: 251 rows, table valid, nothing written.
  - `Source/JamGame/Tests/JamDressingRulesTest.cpp`: `JamGame.Dressing.Rules.NoOverlapsSeeded`, `ScaleMatchesTarget`, `CorridorAndFenceClearance`, `NothingInsideRoadClearance`, `ObstacleLowClusterSpan`, `PaddingAndScaleMath`. Synthetic fixture, 200 seeded segments on one continuous track.
- **Verified by:** nothing yet. Python dry-run only (`python tools/ue/make_dressing_rules.py --dry-run`, exit 0). C++ is not compiled and the tests are not run (gate: no UBT until the orchestrator's go).
- **What went wrong:** nothing observed; the C++ was re-read once by hand, which is not a compile.
- **Corrected by:** self (a helper rename and a TObjectPtr ternary made explicit before commit; script module-passing fixed after the dry-run check).
- **Human decision needed or taken:** Authored choices, to review: fence sits on the strip edge facing the sidewalk (inner edge 800 cm, outer must stay <= 900 cm); footprint class is a size heuristic (<=100 S, <=300 M, <=800 L, else XL); the film cluster preset offsets are a first guess; L2 cluster chance 45 % per side.
- **Not done / not verified:**
  - Not compiled, no automation test run, no PIE or device check.
  - DA_DressingRules is not created; the script's editor path (`unreal.DataAssetFactory`, enum value names, struct field names) is not run.
  - Hazard corridors: no caller sets them yet (the track manager does not reserve corridors from armed or scheduled hazards). The garage facade placement is not implemented.
  - Same-mesh 30 m rule is enforced only inside one segment (segments are 20 m); no cross-segment memory.
  - Cross-segment overlap: facades may overhang a segment (contiguous packing) and L4 is checked only against its own segment, so a deep facade could meet a back-row piece across the seam. Not tested.
  - Roof and far layers are not placed by the rule path (legacy path is replaced when the rules asset is set, so landmarks disappear in that mode).
  - Obstacles: `ObstacleLowClusterOffsets` and `NeedsBlockPair` exist and are tested, but are not wired into `JamObstaclePlacement::BuildRow`; obs_high and obs_block placement are unchanged.
  - Front-axis yaw of the imported facades is still unverified in the editor (FrontYawDeg is 0 for all rule rows).
- **Screenshot:** none (no build yet).

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 00:10 | Code edit (new files and segment hook) | Claude Haiku 5.5, Edit/Write, TASK-073 | JamDressingRules.h/.cpp, JamDressingRulesTest.cpp, JamArtSet.h, JamTrackSegment.h/.cpp | Written; not compiled |
| 00:17 | Script | Python 3 `--dry-run` | 251 rows, 0 errors, nothing written | Dry-run verified; editor path pending |
