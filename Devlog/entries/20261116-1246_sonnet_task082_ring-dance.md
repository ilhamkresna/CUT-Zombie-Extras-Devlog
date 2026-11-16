# 2026-11-16 12:46 WIB — sonnet — duel audience ring packed tighter, dance randomizer audit

- **Who / tool:** worker, Claude Sonnet in Claude Code (under the Claude orchestrator)
- **Task:** TASK-082 (owner tweak) · **Branch / commit:** main, uncommitted
- **Asked for:** pack the zombie ring around the duel tighter (especially the back), and confirm the dance animation randomizer is active.
- **Result:** four new multiplier tunables in `Source/JamGame/Data/JamCrowdTuning.h` (`DuelRingBackScale` 0.7, `DuelRingSideScale` 0.9, `DuelRingAngleScale` 0.9, `DuelRingStaggerScale` 1.2) applied in `AJamHordeActor::DuelWingOffset` (`Source/JamGame/Run/JamHordeActor.cpp`) after the saved `DA_CrowdTuning` values are read, so the saved asset cannot override them. Back radius 480 to 336 cm, side radius 520 to 468 cm, half angle 95 to 85.5 deg, gap 35 to 31.5 deg, outer row scale 1.1 to 1.2, effective min radius 450 to 315 cm. Randomizer audit: no code change needed (see below).
- **Verified by:** nothing run. Source read only, NOT compiled, NOT run in PIE. Spacing worked out by hand (12 slots, about 9 deg apart, alternating rows: centres at least about 100 cm apart).
- **What went wrong:** the first two edits of `JamHordeActor.cpp` failed with EPERM because a compile was holding the file; the edit was applied by a retry write loop once the lock cleared (CRLF kept).
- **Corrected by:** self.
- **Human decision needed or taken:** none. If the crowd looks too packed or too loose, change the four multipliers in `DA_CrowdTuning` or the header defaults.
- **Not done / not verified:** compile; PIE look (camera framing of the back arc, no clipping into the hero); whether the saved `DA_AnimSet` for the duel enemy has `CrowdDance` Clip plus Alternates filled (data asset, not readable here). `DuelSlotJitterCm` (45 cm outward only) left unchanged.
- **Screenshot:** none

## Randomizer findings
Path at runtime: `AJamHordeActor::StartAudience` -> `BuildDanceOrder(ZombieAnimSet, FreshStream)` -> per slot `DanceOrder[AudienceSlotOrder[i % SlotCount] % DanceOrder.Num()]` -> `JamZombie::PlayLoop` (random start time and play rate between min(PlayRateMin, 0.9) and max(PlayRateMax, 1.1)). `ZombieAnimSet` comes from `UJamCharacterSettings::DuelEnemy.AnimSet`, entry `EJamAnim::CrowdDance`: `Clip` plus `Alternates`. Seed is fresh per duel (clock), slots are shuffled too. No early return, no flag, no off-by-one found. If `CrowdDance` has no clip, it falls back to `ResolveCrowdLoopClip` (CrowdDance, CrowdCheer, FightIdle, Idle); only if none exists is the pose static.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 12:43 | Audit of ring layout, camera and randomizer | Claude Sonnet, Grep/Read, TASK-082 | Layout in `DuelWingOffset` + `JamCrowd::AudienceRingSlot`; randomizer active in code | source read only |
| 12:44 | Edit `JamCrowdTuning.h`, `JamHordeActor.cpp` | Claude Sonnet, Edit/PowerShell write, TASK-082 | 4 multipliers added and applied; first cpp edits failed (file locked by compile), retry succeeded | NOT compiled, NOT run |

No keys, account identifiers, signed URLs or private perk codes. Log self-edits are covered by this entry; do not recurse.
