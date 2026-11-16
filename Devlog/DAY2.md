# Day 2 — 13 November 2026 — asset continuation (WIB)

> In-progress record, not a release announcement. Production readiness requires real scene/data integration and cooked-device evidence.

## Goal for today

Continue hero and enemy/NPC animation plus mechanical assets in parallel, using Meshy only for new provider work. Keep content editable through Unreal scenes, Blueprint properties, Data Assets, Curve and Niagara parameters.

## Work in progress

- Hero027: source handoff Review / REWORK. Existing Meshy rig reused;21 Approximate mappings and color previews. Slide and FightIdle source corrections are documented. Wrong generated spins remain rejected; Spin360 is an explicitly authored jump/yaw composition, with no saved Unreal Curve.
- Enemy/NPC034: source handoff Review / REWORK. Duel rig8303 tris and independently Meshy-rigged LOD2878 tris both have24 deform bones/max4 weights. Failed manual LOD transfer rejected. Provider LOD poses retain shape in sampled run/dance/cheer/get-up, but attack hook/sweep readability, role gaps, root/contact/timing and shared skeleton compatibility remain unresolved.
- Tripod035: Review / REWORK; component-first Meshy kit2338 evaluated tris/512 maps. Coherent sampled closed/open poses; open footprint smaller than requested. Phase mapping corrected in source metadata, not implemented Blueprint.
- Simple equipment case036: LUNA source PASS only,880 tris/512 map, both-format bottom-pivot reimport. Uniform scaling yields1.806 x1.194 x2.050m, a disclosed source deviation. Initial local axis/pivot fault corrected and failed export preserved.
- Roadside fence/gate037: TERRA source handoff Review / REWORK for continuous/engine motion. Geometry2428 tris/two512maps,5 meshes/2 object actions, all96 integer frames outside5.2m roadside target. Hinged-gate variant, not broken/toppling geometry. Scene/data/crowd integration remains pending.
- VFX: dust/blast pilot authoring remains partial; no false fire/device completion. Current scene/data and renderer warning fixes are tracked separately.

## Evidence and status

See timestamped worker entries under entries/ and the coordinator source-stage audit. TASK024 source PASS is separate from engine/device gates. Crane TASK026 remains REWORK; a3638-triangle authoring candidate is not an accepted strict3k mobile asset. No device/PIE pass is implied by these raw sheets or videos.

## Human steering and tool accountability

The owner required Meshy-only generation, authorized existing credits, rejected hardcoded content values and required a devlog for every edit/generation. Keys are stored only in environment storage and never published. Seven no-network tests passed; generation retries are allowed only for genuine credit insufficiency, never rate/auth/agent usage limits or uncertain create outcomes.

OpenAI Codex remains planner/auditor, with gpt-6.1-terra handling harder animation/geometry. Earlier gpt-6-luna source/crate work is documented in Day1 retrospective entries. Meshy outputs and local corrections are attributed separately; no unreported image-model identity or award eligibility is claimed.

## Next verification

Per-clip motion/contact/timing, actual editable Unreal Blueprint/Curve/DataAsset wiring, active hazard footprint/phase alignment, pooling/reset and cooked Android/Windows tests. Record failures as they occur and do not promote sources straight to ProductionReady.

## Logic lane, audits and merges (13 Nov)

Logic was built by Claude executors (Claude Haiku in Claude Code, one git worktree per task packet) under a Claude orchestrator, which read every branch before it reached `main`. Nothing from this list has been played in PIE or on a device.

| Task | What it is | Verified by (as the entry states) |
|---|---|---|
| 029 | Hazard schedule: pure functions `IsFootprintFair`, `PickHazard`, `DecideSegment` | editor build succeeded; `JamGame.Hazard.*` 8 of 8 |
| 030 | Hazard families, catalog rows, developer settings | editor + Android compile; `JamGame.Hazard` 9 of 9 |
| 011 | Duel crowd wings around the runner | editor + Android compile only |
| 020 | Presentation core: anim set, anim instance, jump/slide styles | editor + Android compile only |
| 038 | Art assembly: 10 meshes, `DA_ArtSet` with 5 slots | compile + `ArtSetLoads` test |
| 017 | Audio set, audio subsystem, audio rules helper | editor + Android compile; `JamAudioTest` |
| 015 | Run recap data asset and pure recap rules | compile + automation |
| 018 | Pause, real clock for duel timelines | compile + `JamGame.Pause.RealClock` |
| 051 | Oncoming and side-entry traffic hazards | compile + 10 `JamGame.Hazard.*` pass |
| 016 | Game UI base classes (first pass, then revision 2) | compile + UI automation; audited by reading 14 Nov |
| 052 / 053 | Themes data types; elevated routes (deck actor) | audited by code reading; merged 14 Nov |

Audit 052 passed on code reading only (no build, no test). The 13 Nov coordinator also fast-forwarded its own preparation commits to `main` (fa49aeb), and the owner moved Unreal editor work to a single-editor lane: the root only plans and audits, and all implementation and engine actions are delegated.

Asset lane (Codex agents Terra and Luna; Meshy workers for provider generation) produced source candidates, each with an honest status. Examples:
- Hero 027 (Review / REWORK): 21 mapped clips, slide and FightIdle seam repaired; the spin is an authored jump plus visual yaw, not a saved Unreal curve.
- Enemy 034 (REWORK): manual LOD transfer rejected after collapse; a provider LOD rig replaced it.
- Hero colour 040: 9,970 triangles, 24 bones, seam-aware max weight delta 1.19e-7 (GLB).
- Tripod 035 v2: 2,338 triangles, closed 0.484 x 0.542 m, open 2.201 x 2.526 m (open footprint smaller than requested).
- Case 036: 880 triangles, 512 px map, size 1.806 x 1.194 x 2.050 m after uniform scaling (a disclosed deviation).
- Fence 037: 2,428 triangles, all 96 sampled frames above 5.2 m, hinged gate rather than a toppling piece (REWORK for continuous motion).
- Atlases 042: two 1024 px pages, eight GLB/FBX pairs, max nearest-position delta 4.3e-7 m.
- Variety 047: 11 Meshy sources with hashes; no Blender render, owner override.
- Registry 049: 32 metadata rows appended; 83 rows, 83 unique IDs, 88 source paths present.
- UI theme 039 v3c: six 1080x1920 layouts; Pillow checks text inside its rectangles at 32 px or more.

Local music toolchain (audio lane): ACE-Step 1.5 installed at `D:\AudioTools` on the owner's request (first install was on E:, moved and venvs rebuilt). QA script tested on three synthetic files only.

Meshy: about 36 ledger entries on 13 Nov. Twelve record a credit check. Several are still "submission pending" or "provider response received; poll and audit before acceptance", so no per-job outcome is claimed here.

## What broke and how we fixed it (13 Nov)
- Usage limits cut several executor sessions (029, 016). Resumed from the WIP commit; the worktree was clean, so nothing was lost.
- Android build failed on `-Werror,-Wunreachable-code-loop-increment` in `FindTrackManager` (011). Fixed and rebuilt both targets.
- `OnLanded` shadowed `ACharacter::OnLanded` (020). Renamed to `OnJamLanded`.
- Audio path quoting: `Build.bat` path with spaces split by Bash (017). Re-run through PowerShell.
- Tripod feet penetrated the ground in v1; fixed in v2. Hub reference had four lugs and was regenerated simpler.
- Fence preview scale figure intersected the gate; preview-only placement fixed.
- Case FBX pivot wrong after import; failed local export kept as evidence; Meshy N1 texture-prompt field removed (unsupported).
- Pause test expectation wrong (200 frames, 100 unpaused = 6.25 s, not 0.5 s); test corrected from the log line.
- Asset source audit: the prior worker's model-usage limit interrupted work; this was not Meshy credit exhaustion.

## Human decisions (13 Nov)
- Owner: Meshy-only generation for new provider work, existing credits authorised, no hardcoded content values, devlog for every edit.
- Owner: audio tools on `D:`; Unreal work moved to one editor lane; root no longer implements.
- Concept and direction were set on 12 Nov; the 13 Nov comedic art direction (vivid colour) replaced earlier grey/navy candidates.

## Not verified (13 Nov)
- No PIE, no Android or Windows device run for any task above.
- Logic tasks 011 and 020 were compile-only. 029, 030, 038, 017, 015, 018 and 051 were compile plus headless automation only.
- Asset sources: no Unreal import, no in-engine check, no motion/contact check of the Meshy clips beyond Blender sampling.
- Audit 052 has no build or test behind it.

## Entries (13 Nov)
Logic: [029](entries/20261113-0315_exec_task-029-hazard-schedule.md), [011](entries/20261113-0343_exec_task-011-duel-crowd.md), [030](entries/20261113-0805_exec_task-030-hazard-families.md), [020](entries/20261113-0850_exec_task-020-presentation-core.md), [038](entries/20261113-0908_exec_task-038-art-assembly.md), [017](entries/20261113-1045_exec_task-017-audio.md), [015](entries/20261113-1112_exec_task-015-recap.md), [018](entries/20261113-1120_exec_task-018-pause.md), [051](entries/20261113-1500_exec_task-051-traffic-hazards.md), [016](entries/20261113-1502_exec_task-016-ui.md), [audit 052](entries/20261113-1744_audit_task-052.md), [coordinator takeover](entries/20261113-1251_coord_unreal-takeover.md).
Assets: [hero 027](entries/20261113-0000_hero_task027-continuation.md), [enemy 034](entries/20261113-0002_terra_enemy_npc.md), [tripod 035](entries/20261113-0001_terra_task035-tripod.md), [case 036](entries/20261113-0019_assets_luna-case-task036.md), [fence 037](entries/20261113-0023_terra_task037-fence.md), [audio toolchain](entries/20261113-0230_audio_local-music-toolchain.md), [reactions 041](entries/20261113-0450_terra_reactions041.md), [hero colour 040](entries/20261113-0451_terra_task040-herocolor.md), [atlas 042](entries/20261113-0500_terra_task042-atlas.md), [variety 047](entries/20261113-1258_terra_variety047.md), [registry 049](entries/20261113-1311_luna_task-049-registry.md), [UI theme 039 v3](entries/20261113-0510_terra_task039-v3.md).

## Plan for tomorrow
Continue in [DAY3.md](DAY3.md): merge the remaining logic branches, first compile and automation pass after integration, then an actual Lit L_Run check.
