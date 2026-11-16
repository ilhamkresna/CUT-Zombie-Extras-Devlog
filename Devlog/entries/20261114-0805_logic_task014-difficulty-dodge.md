# 2026-11-14 08:05 WIB — logic — TASK-014 difficulty curve + enemy dodge

- **Who / tool:** logic worker, Claude Sonnet 5.5 in Claude Code
- **Task:** TASK-014 · **Branch / commit:** main, see commit "TASK-014: difficulty curve + enemy dodge"
- **Asked for:** Write the packet, add a capped difficulty curve and the enemy dodge in duels, with headless tests.
- **Result:** New FJamDifficultyCurve (Run/JamDifficultyCurve) used by the GameMode for Difficulty01, speed scale (hard cap 1.7) and duel interval. New FJamDuelDodgeRules (Duel/JamDuelDodgeRules): after a dodged Mid/Low attack the enemy rolls the scene row EnemyDodgeChance (capped by UJamDuelTuning::EnemyDodgeChanceCap 0.3), hops to a free adjacent lane at Open start, Open window +0.2 s. Seeded FRandomStream per enemy. Packet tasks/TASK-014.difficulty-dodge.md. Most curve targets (patterns, hazards, telegraph, scene table) already existed from earlier tasks and were not changed.
- **Verified by:** compile + automation tests (JamGame.Difficulty.CurveMonotonicCapped, JamGame.Duel.EnemyDodgeDeterministic and the earlier JamGame.* tests). NOT played by a human, not run in PIE or on a device.
- **What went wrong:** A first shell command with a here-doc failed to parse, so nothing was written; files were recreated with the Write tool. Editor was open at start, work paused until the owner closed it.
- **Corrected by:** self
- **Human decision needed or taken:** Coordinator ruled DESIGN_SPEC wins over PRD (dodge cap 30 %).
- **Not done / not verified:** Feel of the hop and the +0.2 s window in PIE; DodgeHop clip with real assets; chase-pressure scaling (no spec numbers, out of scope); Android.
- **Screenshot:** none

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 08:05 | C++ edits: curve, dodge rules, enemy, tests, packet | Claude Sonnet 5.5, TASK-014 | files under Source/JamGame/Run, Duel, Tests; tasks/TASK-014.difficulty-dodge.md | compile + tests (see Verified by) |

No keys, account identifiers, signed URLs or private perk codes. Log self-edits are covered by this entry; do not recurse.
