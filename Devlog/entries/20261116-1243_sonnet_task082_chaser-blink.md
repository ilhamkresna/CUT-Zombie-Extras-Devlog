# 2026-11-16 12:43 local — gameplay — chaser surge and roadside zombie blink

- **Who / tool:** executor, Claude Sonnet worker under a Claude orchestrator, Claude Code (source reading and editing only)
- **Task:** TASK-082 owner bug fix · **Branch / commit:** main, uncommitted
- **Asked for:** make the chasing horde keep a steady speed relative to the hero and stay present, and stop roadside zombies from blinking.
- **Result:** (1) `Run/JamHordeActor.cpp`: the target gap between horde and hero (`CurrentGap`) now moves with `FInterpConstantTo` at the new tunable `GapEaseRateCmPerSec` (default 220 cm/s, `Run/JamHordeActor.h`) in the Running and Duel branches. It used `FInterpTo`, whose speed is proportional to the remaining distance, so a Safe/Close switch made the horde surge then crawl, or drop back fast out of frame. (2) `Run/JamZombieVisuals.cpp` `MakeComponent`: `bComponentUseFixedSkelBounds = true` and `SetBoundsScale(2.5f)` on every pooled zombie component (horde, stragglers, roadside crowd, audience). With `OnlyTickPoseWhenRendered` the animated bounds go stale once culled, so zombies flickered at the frustum edge.
- **Verified by:** nothing. Source read only; NOT compiled, NOT run, no PIE. Engine header checked: `bComponentUseFixedSkelBounds` (public bitfield, SkinnedMeshComponent.h) and `UPrimitiveComponent::SetBoundsScale(float)` exist in UE 5.8.
- **What went wrong:** nothing observed; the cause of the blink is inferred from code, not reproduced.
- **Corrected by:** n/a
- **Human decision needed or taken:** none. The horde still leaves the frame when the chase is Safe (SafeDistance 1500 cm is off camera by design); changing that is a tuning decision.
- **Not done / not verified:** compile, PIE feel of the 220 cm/s rate, the blink fix. `JamRoadsidePen.cpp` (pen members use SetHiddenInGame, not edited, owned by another worker) not audited as a blink source.
- **Screenshot:** none

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 12:43 | Edit JamHordeActor.h/.cpp, JamZombieVisuals.cpp | Claude Sonnet, TASK-082 | constant-rate gap, fixed skel bounds | source only, not compiled |

No keys, account identifiers, signed URLs or private perk codes. Log self-edits are covered by this entry; do not recurse.
