# 2026-11-15 00:27 WIB — assets — TASK-073 follow-up: obstacles, corridors, cross-segment mesh memory, roof/far, front yaw (code only, not compiled)

- **Who / tool:** code worker, Claude Haiku 5.5 in Claude Code; file edits and a brace/paren balance check only.
- **Task:** TASK-073 follow-up requested by the coordinator (five items) · **Branch / commit:** main, see the commit that adds this entry.
- **Asked for:** (1) wire ObstacleLowClusterOffsets and NeedsBlockPair into JamObstaclePlacement::BuildRow; (2) corridor caller in the track manager; (3) same-mesh 30 m rule across segments; (4) keep roof and far in rule mode; (5) FrontYawDeg as a UPROPERTY default 0.
- **Result:**
  - (1) `Art/JamObstaclePlacement.cpp`: obs_low lanes are always a 2..4 item cluster (`PlaceLowCluster`, all or nothing, span >= 230 cm); obs_low never gets a long piece; obs_block narrower than 200 cm and clusterable is a 2-up pair (`PlaceBlockPair`); on centre lanes narrow block variants are excluded from clusters. Wide blocks, highs and edge placement are unchanged.
  - (2) `Track/JamTrackManager.cpp`: `MakeHazardCorridors` (SideEntry family only: BusDoorway, SideVan, SideTruck; corridor from the 450 cm road edge out to SideStartY, over ContactDepthCm + 300 cm each side, both sides). Scheduled hazards set the corridor BEFORE the segment's Configure. A hazard armed by `ArmHazardAhead` calls `RedressWithCorridors` on the already dressed segment. The legacy dressing path clears stale corridors.
  - (3) `JamDressingRules::FHistory` (per side, global X, pruned at 6 km behind). The track manager owns `DressingHistory` and passes it into every Configure. `SameMeshClash` checks history and this segment (scenery excluded); facades that would repeat a mesh are skipped, leaving a hole.
  - (4) Roof and far are placed by the rule path, not the legacy lists: roofs on L3 facades (RoofChance 0.5, lifted to the facade top), far landmarks (FarChance 0.35, inner edge in 4000..8000 cm). Both are marked `bScenery` and sit outside the ground footprint rules.
  - (5) `FJamDressingRuleRow::FrontYawDeg` (UPROPERTY, default 0), passed into the piece description. The import worker verifies it.
  - Redress design: `RedressWithCorridors` filters the pieces already placed (drops L0-L4 ground pieces in the corridor) and re-shows them. It does not re-run the random draw, so facade cursors and later segments keep their placement. This is a filter, not a full re-dress.
- **Verified by:** nothing yet. Brace and paren balance on the eight touched C++ files only. No compile, no UBT, no automation test run (gate: wait for the orchestrator's go).
- **What went wrong:** the first `Edit` attempts on the header failed on whitespace; the segment and track manager files are CRLF, and the Python helper first missed the newline pattern. Both fixed by normalising to LF for matching and restoring CRLF on write. A bash heredoc of the full rules source failed to parse; the file was re-written with the Write tool.
- **Corrected by:** self.
- **Human decision needed or taken:** Item (4) is a rule-based roof/far placement, not the previous legacy list placement. If the legacy placement must be kept, say so and it becomes a separate change.
- **Not done / not verified:**
  - Not compiled; the new automation test expectations are untested.
  - `BuildRow` has no runtime caller outside the tests (grep: only `JamObstaclePlacementTest.cpp` calls it), so the obstacle wiring does not change gameplay yet.
  - `BlockPairBelowCm` (200) and `LowClusterMinSpanCm` (230) are constants in the .cpp, not tuning fields.
  - Edge-anchor for narrow block is not implemented (only the 2-up pair).
  - Corridor X span uses ContactDepthCm + 300 cm each side as a proxy; the real hazard span is not checked.
  - Cross-segment facade overhang vs back row is still not checked across the seam.
  - No new obstacle test was added; the existing `JamGame.Obstacle.*` tests are expected to stay green (not run).
  - Hazard family SideEntry is assumed to cover every side-entry hazard; confirmed for BusDoorway, SideVan, SideTruck in the catalog.
- **Screenshot:** none.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 00:20 | Code edit | Claude Haiku 5.5, Edit/Python/Write, TASK-073 follow-up | JamObstaclePlacement.cpp, JamDressingRules.h/.cpp, JamDressingRulesTest.cpp, JamTrackSegment.h/.cpp, JamTrackManager.h/.cpp | Written; not compiled |
| 00:27 | Brace/paren balance check | Python 3 | 0 imbalance on the eight touched files | Balance only; compile pending |
