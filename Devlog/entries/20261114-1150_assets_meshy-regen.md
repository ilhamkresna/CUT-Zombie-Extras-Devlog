# 2026-11-14 11:50 WIB — assets — Meshy regen (rig hero + zombie, animations)

- **Who / tool:** Meshy worker, Claude Haiku 5.5 in Claude Code; Meshy REST API via stdlib Python (`tools/meshy/regen_20261114.py`); Blender 5.1 CLI for the bone check.
- **Task:** Meshy regen 2026-11-14 (no TASK id) · **Branch / commit:** main, see commits `2064d7b`, `579ac39` and the commit for this entry.
- **Asked for:** Auto-rig the hero and zombie preview sources, apply the approved animation slots (cap 50, then 120 with extras), download FBX/GLB to `raw_assets/MESHY-REGEN-20261114/`, and run a bone-name check.
- **Result so far:**
  - Rigs: hero and zombie both SUCCEEDED. Files: `raw_assets/MESHY-REGEN-20261114/{hero,zombie}/rigged_character.fbx` and `.glb`.
  - Zombie library animations SUCCEEDED, FBX and GLB saved under `raw_assets/MESHY-REGEN-20261114/zombie/`: `crowd_dance_a` (64 All Night Dance), `crowd_dance_b` (591 Hip Hop Dance), `crowd_dance_c` (23 Funny Dancing 2), `fight_idle` (89 Combat Idel), `wind_up` (127 Charged Ground Slam), `attack_overhead` (128 Heavy Hammer Swing), `attack_hook` (193 Left Hook from Guard), `attack_sweep` (213 Leg Sweep), `dodge_hop` (156 Stand Dodge).
  - Hero stumble: action 674 "Stumble Walk" was rejected by the API ("no longer available"). Nothing was spent on it. The script now uses 562 "Stumble Walk" instead. The rejected entry was removed from `tasks.json` so the next run creates 562. The owner runs it.
  - Dizzy from the library: skipped, no in-place match for drunk, daze, dazed, wobble, dizzy or stagger idle.
  - Extras pending the owner's run: library 24 "Funny Dancing 3", 63 "Arm Circle Shuffle", 77 "Magic Genie" (9 credits). Text-to-motion prime, 4 s, for twerk, robot, sprinkler, wings and dizzy (50 credits), each applied to the zombie rig with `motion_task_id` (15 credits). Prompts are stored verbatim in `tasks.json` when created. Owner-run total would be 37 + 3 (hero stumble) + 9 + 50 + 15 = 114, under the 120 cap.
- **Verified by:** Meshy API responses (task statuses, balance); Blender 5.1 CLI bone check (below).
- **Bone check (Blender CLI, free):** all five FBX files read as 24 bones with identical names: hero rigged, zombie rigged, zombie `crowd_dance_a`, and the older Meshy references `raw_assets/ZMB-01/benchmark/walking.fbx` and `rig.fbx`. The first 10 names are `Hips, Spine02, Spine01, Spine, neck, Head, head_end, headfront, LeftShoulder, LeftArm`. The names match the older Meshy rig.
- **Balance:** 424 before (registry key, checked after the key fix), 387 after the zombie animations. Ledger spent: 37 (2 rigs at 5 = 10, plus 9 zombie animations at 3 = 27; hero 674 spent 0).
- **What went wrong:** The first run read the old 2-credit key from the process environment. Fixed: the script now reads only `HKCU\Environment` and aborts if the balance is below 45 (or 125 for extras). The hero action 674 was rejected by the API. The first zombie-source attempt was `rig.glb`, which is the GLB sibling of the `rig.fbx` that the UE import used, and it is already rigged. The owner has not chosen a static source.
- **Corrected by:** Orchestrator (key source, cap, extras, action change to 562); self (script fix).
- **Human decision needed or taken:** Owner runs the script for the pending items (hero 562, extras). Paid steps are not to be run by an agent.
- **Not done / not verified:**
  - Hero stumble 562, library extras, text-to-motion clips and their apply steps.
  - Download of the hero stumble clip.
  - Whether the clips loop (the library does not say).
  - Unreal re-import or in-engine check.
  - Visual check of the motions.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 11:20 | Free library search (MCP `meshy_list_animations`) for 11 slots | Meshy MCP, free | Candidate list sent to orchestrator | Done |
| 11:40 | Wrote `tools/meshy/regen_20261114.py` | Claude Haiku 5.5, Write | Stdlib-only script | Run 1 failed (old key, 402) |
| 11:55 | Run 2, hero + zombie rigs (registry key, balance 424) | Meshy REST `POST /openapi/v1/rigging` x2 | Both SUCCEEDED, downloaded | Done |
| 12:00 | Run 2, hero 674 animation | Meshy REST `POST /openapi/v1/animations` | HTTP 400 "no longer available" | Rejected, 0 spent; replaced by 562 in script |
| 12:05 | Run 3, zombie library animations x9 | Meshy REST `POST /openapi/v1/animations` (3 cr each) | 9 SUCCEEDED, FBX+GLB saved | Done; balance 387 after |
| 12:10 | Blender CLI bone check (free) | Blender 5.1 `-b -P` script (in scratchpad) | 24 bones, names identical across 5 FBX | Done |

## Update — final outcome (orchestrator, Claude Opus 5.5, 14 Nov)
- The paid extras were blocked for the worker by the auto-mode permission classifier; the owner ran `python -X utf8 tools/meshy/regen_20261114.py` personally (3 times; the script is idempotent and created no duplicates).
- Verified by the orchestrator from the Meshy balance endpoint and `raw_assets/MESHY-REGEN-20261114/tasks.json`: balance 424 -> 310, **114 credits** spent in total, 25 tasks all SUCCEEDED, one task per slot.
- Added in the owner run: hero_stumble = 562 "Stumble Walk" (674 had been rejected as unavailable); library dances 24 "Funny Dancing 3", 63 "Arm Circle Shuffle", 77 "Magic Genie"; text-to-motion (prime, 4 s) + apply to zombie rig for crowd_dance_twerk / robot / sprinkler / wings and dizzy, prompts verbatim as stored in tasks.json.
- These replace the 4 blocked ANIM_05 imports (hero_stumble + 3 dances) for gameplay use; the blocked Review packages were left untouched.
- Five text-to-motion results checked as distinct files (md5). Bone check: 24 identical Meshy bone names on hero rig, zombie rig and clips (Blender CLI).
- Not verified here: how the clips look in PIE (see TASK-022 / 022b entries).
