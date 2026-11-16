# 2026-11-14 18:54 WIB — assets — Meshy intro clips (hero + NPC), config-driven run

- **Who / tool:** Meshy worker, Claude Haiku 5.5 in Claude Code; Python script `tools/meshy/clips_from_config.py` (stdlib, Meshy REST API)
- **Task:** Meshy intro clips 20261114 (config `tools/meshy/intro_clips_20261114.json`) · **Branch / commit:** main / see commit
- **Asked for:** Generalise `zombie_clips_20261114.py` into a config-driven script; run the approved paid set (new hero rig, existing NPC rig, 3 library actions, 6 text-to-motion prime 4 s clips applied to the named rig); download applied FBX; bone check against `raw_assets/MESHY-REGEN-20261114/zombie/fight_idle.fbx`.
- **Result:**
  - Script `tools/meshy/clips_from_config.py` (`--config <json>`; registry-only key via `key_var`; balance gate `min_balance`; hard `cap`; idempotent ledger `out_dir/tasks.json`; uncertain/rejected entries stop the run).
  - All 9 planned clips applied: `hero_sit_idle`, `hero_sit_to_stand`, `hero_run_leap` (library action 402 "Run and Leap", non-in-place, replacement for rejected 656), and all six NPC text-to-motion clips (`npc_film_idle`, `npc_boom_idle`, `npc_makeup_idle`, `npc_clipboard_idle`, `npc_bitten_fall`, `npc_panic_run`).
  - `hero_run_leap` (action 402) downloaded to `raw_assets/MESHY-INTRO-CLIPS-20261114/hero/hero_run_leap.fbx`.
  - Downloads in `raw_assets/MESHY-INTRO-CLIPS-20261114/`: `hero/rigged_character.fbx`, `hero/hero_sit_idle.fbx`, `hero/hero_sit_to_stand.fbx`, `npc/rigged_character.fbx`, `npc/npc_{film_idle,boom_idle,makeup_idle,clipboard_idle,bitten_fall,panic_run}.fbx`, raw motions in `raw_assets/MESHY-INTRO-CLIPS-20261114/raw_motion/`. Ledger `raw_assets/MESHY-INTRO-CLIPS-20261114/tasks.json` (ignored folder).
- **Verified by:** Blender 5.1 background import (`blender -b --factory-startup`), bone count and names: `hero/rigged_character.fbx`, `npc/rigged_character.fbx`, `hero/hero_sit_idle.fbx`, `hero/hero_run_leap.fbx` and `MESHY-REGEN-20261114/zombie/fight_idle.fbx` each have 24 bones, names identical to the hero rig. Credits from the Meshy balance endpoint (number only): 405 before, 313 after, 92 spent. Nothing verified in Unreal or on device.
- **What went wrong:**
  1. Run 1 stopped at `hero_run_leap` (action 656): HTTP 400 "This animation is no longer available". Nothing was charged for it (balance went 405 to 394 for the 11 credits of the two hero actions before the stop).
  2. Run 2 reran the same config minus action 656 (scratch copy, not committed). The ledger keeps the rejected entry `hero_hero_run_leap`.
  3. The regen ledger already holds a hero rig (`hero_rig`, same source GLB, 1.8 m) created earlier; config asked for a new rig from source, so a second hero rig was created (5 credits).
  4. Run 3 (orchestrator-chosen replacement): config `hero_run_leap` set to action 402 "Run and Leap"; the rejected ledger entry was removed from `tasks.json` so the slot could be created; reran the real config (no scratch copy). Only the new slot cost credits.
- **Corrected by:** orchestrator chose the replacement action (402); self updated config and reran.
- **Human decision needed or taken:** Taken: orchestrator picked action 402 as the replacement for the retired 656 (non-in-place, so the cinematic director must cancel root motion).
- **Not done / not verified:**
  - Action 402 is non-in-place: root motion is present in the clip; the cinematic director must cancel it.
  - No Unreal import, no in-engine check, no device check.
  - The duplicate hero rig from the regen ledger was not reused (config said create new); could save 5 credits if the owner prefers.
  - `zombie_clips_20261114.py` kept unchanged (not deleted).
  - Balance after is 316 (ledger).

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| run 1 | Rig `hero_rig` (new, source `HERO-01_recolor_raw.glb`, height 1.8 m) | Meshy rigging API, ledger entry `hero_rig` | SUCCEEDED, rigged FBX downloaded | Downloaded only; not opened in engine |
| run 1 | Reuse `npc_rig` from `raw_assets/MESHY-INTRO-B12-20261114/tasks.json` slot `npc_cameraman_rig` | Existing Meshy rigging task, no charge | SUCCEEDED, rigged FBX downloaded | Downloaded only |
| run 1 | Library `hero_sit_idle` (action 364, "Sit on Chair Arms Crossed") and `hero_sit_to_stand` (action 53, "Sit to Stand Transition Male") | Meshy animations API, 3 cr each | SUCCEEDED, FBX downloaded to `hero/` | Downloaded only |
| run 1 | Library `hero_run_leap` (action 656, "Run and Leap (in place)") | Meshy animations API | HTTP 400 "no longer available"; ledger `rejected`; not charged | Pending: needs replacement action id from owner |
| run 2 | Run 2 (scratch config without action 656) | Same script | Text-to-motion prime, 4 s, 6 clips, applied to `npc_rig`; 6 apply calls | Downloaded only |
| after run 2 | Bone check, Blender 5.1 `-b` | `bone_check.py` (scratchpad) | 24 bones each, names equal to `fight_idle.fbx` | Verified in Blender only |
| run 3 | Library `hero_run_leap` replaced: action 402 "Run and Leap" (non-in-place), 3 cr | Meshy animations API, ledger entry `hero_hero_run_leap` | SUCCEEDED, FBX downloaded to `hero/hero_run_leap.fbx`; 24 bones, names equal to `fight_idle.fbx` (Blender) | Downloaded and bone-checked only |

### Prompts (verbatim from `tools/meshy/intro_clips_20261114.json`)
- `npc_film_idle`: A film crew cameraman stands in place filming with a heavy camera on the right shoulder, slowly panning left and right, small weight shifts, seamless loop.
- `npc_boom_idle`: A film crew boom operator stands in place holding a long boom pole up overhead with both hands, adjusting it slightly, small weight shifts, seamless loop.
- `npc_makeup_idle`: A makeup artist stands in place gently dabbing powder with a brush in the right hand toward someone's face at chest height, small steps, seamless loop.
- `npc_clipboard_idle`: A production assistant stands in place reading a clipboard held in the left hand and writing with the right hand, glancing up now and then, seamless loop.
- `npc_bitten_fall`: A person is grabbed by the shoulder from behind, recoils in cartoon shock with arms flailing, then falls backwards onto the ground and lies still.
- `npc_panic_run`: A person panics and runs away in place, waving both arms above the head in cartoon terror, seamless loop.

### Credits
- Balance before 405, after 313 (ledger: 92 spent, cap 95). Breakdown: hero rig 5, three hero library actions 9, six text-to-motion 60, six apply 18 = 92 (the rejected action 656 cost 0).

No keys, signed URLs or account identifiers in this entry. Task ids are kept only in the ignored ledger file.
