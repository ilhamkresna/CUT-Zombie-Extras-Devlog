# 2026-11-13 — TERRA — enemy/NPC animation benchmark

- **Who / tool:** TERRA, Codex; Meshy and Blender as recorded below
- **Task:** TASK-034
- **Asked for:** Reuse ZMB-01 for a compatible animated enemy and NPC benchmark.
- **Result:** Meshy duel and separate LOD rigs, provider animation candidates, textured benchmarks and honest REWORK evidence saved under raw_assets/ZMB-01/benchmark.
- **Verified by:** Blender 5.1 GLB reimport and sampled textured poses; numerical rig/weight budgets checked. No Unreal or device test.
- **What went wrong:** Manual LOD transfer was rejected after severe collapse; provider LOD rig replaced it. Target sweep and hook remain semantically unclear; several other roles remain missing or unaudited.
- **Corrected by:** Self with parent direction: provider LOD rig generated and numerical metrics audited; rejected manual candidate preserved.
- **Human decision needed or taken:** None.
- **Not done / not verified:** Role coverage, contact and loop quality, root correction, gameplay retiming, separate skeleton compatibility, Unreal integration, and device performance remain REWORK.
- **Screenshot:** None yet.

## Operation ledger
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| Start | Read task, project rules, dynamic plan and architecture | TASK-034; Codex | Exclusive scope confirmed; current color rule Low red, High yellow, Block orange | Source work pending |
| Start | Provider duplicate check and library discovery | Meshy list rigging and animation library | One existing rig job found; identity must be checked; library queried | No generation yet |
| Start | Rig submission pending | Meshy automatic biped rig; existing ZMB-01_zombie_extra_revised.glb; height 1.9 m; 5 credits | Existing rig identified as HERO-01; submit once | Await provider task ID |
| Start | Rig submitted | Meshy automatic biped rig, job 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42 | Provider accepted | Pending completion |
| Start | Rig status check | Meshy rig 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42 | SUCCEEDED, 5 credits; files not yet verified | Download and inspect pending |
| Start | Download rig outputs | Meshy rig 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42, raw FBX/GLB and included motions | Download pending | Inspection pending |
| Start | Download rig outputs | Meshy job 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42 | rig.fbx, rig.glb, walking and running FBX/GLB saved | Import audit pending |
| Start | Animation library and batch submission pending | Meshy rig 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42; actions 386 scream, 470 jumping down, 213 leg sweep, 174 face punch reaction, 366 falling down, 640 jump over obstacle in-place, 49 motivational cheer, 59 victory cheer, 61 happy jump, 22 funny dancing; 30 credits | Actual names from provider library; suitability and full coverage unverified | Submit pending |
| Start | Animation batch rejected | Meshy actions 386,470,213,174,366,640,49,59,61,22 | Provider rejected unavailable action 640; no job ID | Replace with library action 471; re-submit |
| Start | Animation batch submitted | Meshy actions 386,470,213,174,366,471,49,59,61,22; job 01a1d0b6-f0bb-71d5-a998-5470f3918731 | Pending | Motion validation pending |
| 00:03 | Write inspection script | Blender 5.1 Python import and measure rig and LOD | Script and JSON pending | No engine/device claims |
| 00:04 | Correct own devlog timestamp | Codex; UTC plus 7 | Renamed 1000 to 0002; initial timestamp was erroneously in future | Ledger retained |
| 00:04 | Import and metric audit | Blender 5.1 enemy_inspect.py | Source 8223 tris; rig 8303 tris, 24 deform bones, max 4 weights, 1K; LOD 2878 tris unrigged | Numerical budgets pass; weight quality, deformation and LOD transfer pending |
| 00:04 | Submit second animation batch | Meshy rig 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42; library actions 127 charged ground slam, 198 punch combo, 344 stand up 1, 113 mummy stagger, 260 push forward and stop, 261 push and walk forward, 156 stand dodge, 178 hit reaction, 298 cheer with both hands up, 14 run 2; 30 credits | Pending submission | Semantic and motion audit required |
| 00:04 | Second animation batch submitted | Meshy job 01a1d0b7-dded-7786-b064-531c210c6f4d; IDs 127,198,344,113,260,261,156,178,298,14 | Pending | Audit pending |
| 00:05 | Poll animation jobs | Meshy 01a1d0b6-f0bb-71d5-a998-5470f3918731 and 01a1d0b7-dded-7786-b064-531c210c6f4d | In progress 95% and 80%, respectively | Poll original task again |
| 00:05 | Submit targeted motion | Meshy text-to-motion prime 2 s: A cartoon zombie crouches low then sweeps one leg in a wide horizontal arc close to the ground, recovers to standing; in place. | Pending submission; 10 credits | Needs retarget and audit |
| 00:05 | Targeted sweep motion submitted | Meshy prime 01a1d0b8-847b-7453-b2d4-557fb522485c | Pending | Retarget and audit pending |
| 00:05 | Submit targeted motion | Meshy text-to-motion prime 2 s: Cartoon zombie raises both arms overhead, pauses to wind up, slams both fists downward then recovers in place. | Pending submission; 10 credits | Needs retarget and audit |
| 00:05 | Targeted overhead motion submitted | Meshy prime 01a1d0b8-abc7-72c7-a566-403ce845053e | Pending | Retarget and audit pending |
| 00:05 | Submit targeted motion | Meshy text-to-motion prime 2 s: Cartoon zombie turns torso, winds up and throws a broad horizontal hook punch at chest height, recovers in place. | Pending submission; 10 credits | Needs retarget and audit |
| 00:05 | Targeted hook motion submitted | Meshy prime 01a1d0b8-d4c5-7187-949f-9afbba0037d5 | Pending | Retarget and audit pending |
| 00:06 | Poll jobs | Meshy batch 01a1d0b6-f0bb-71d5-a998-5470f3918731 succeeded; 01a1d0b7-dded-7786-b064-531c210c6f4d 95%; prime sweep 01a1d0b8-847b-7453-b2d4-557fb522485c and overhead 01a1d0b8-abc7-72c7-a566-403ce845053e succeeded; hook 01a1d0b8-d4c5-7187-949f-9afbba0037d5 12% | First batch downloadable; retarget prime motions | Not yet motion ready |
| 00:06 | Retarget motion submission | Meshy animate rig 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42; motion 01a1d0b8-847b-7453-b2d4-557fb522485c (crouch sweep), 3 credits | Pending | Audit pending |
| 00:06 | Sweep retarget submitted | Meshy animation 01a1d0b9-33c7-72cf-bf17-11e4027e19e5 | Pending | Audit pending |
| 00:06 | Retarget motion submission | Meshy animate rig 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42; motion 01a1d0b8-abc7-72c7-a566-403ce845053e (overhead slam), 3 credits | Pending | Audit pending |
| 00:06 | Overhead retarget submitted | Meshy animation 01a1d0b9-613e-7255-8606-5e5f6920ed42 | Pending | Audit pending |
| 00:06 | Download first animation batch | Meshy job 01a1d0b6-f0bb-71d5-a998-5470f3918731 | FBX and GLB retrieval pending | Inspection pending |
| 00:06 | Download first animation batch | Meshy 01a1d0b6-f0bb-71d5-a998-5470f3918731 | batch1.glb and batch1.fbx saved | Inspection pending |
| 00:06 | Split and inspect batch 1 | Blender 5.1, batch1.glb, provider action names | Individual GLBs and action metadata pending | Reimport and visual audit pending |
| 00:07 | Split and inspect first batch | Blender 5.1 enemy_split.py; 10 provider actions | batch1_clip_00..09.glb and batch1_actions.json written; 24 fps in Blender | Unverified export, contacts and semantics |
| 00:07 | Poll jobs | Meshy batch 01a1d0b7-dded-7786-b064-531c210c6f4d, hook motion 01a1d0b8-d4c5-7187-949f-9afbba0037d5, sweep retarget 01a1d0b9-33c7-72cf-bf17-11e4027e19e5, overhead retarget 01a1d0b9-613e-7255-8606-5e5f6920ed42 | All succeeded | Download and audit pending |
| 00:07 | Retarget hook motion submission | Meshy rig 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42; motion 01a1d0b8-d4c5-7187-949f-9afbba0037d5; 3 credits | Pending | Audit pending |
| 00:07 | Hook retarget submitted | Meshy 01a1d0ba-61dc-74f3-972b-e32aac8bc033 | Pending | Audit pending |
| 00:07 | Download completed motions | Meshy batch 01a1d0b7-dded-7786-b064-531c210c6f4d, sweep 01a1d0b9-33c7-72cf-bf17-11e4027e19e5, overhead 01a1d0b9-613e-7255-8606-5e5f6920ed42 | GLB/FBX pending | Audit pending |
| 00:07 | Completed motion files downloaded | Meshy batch2, sweep, overhead jobs as above | GLB and FBX saved | Audit pending |
| 00:07 | Render color key poses | Blender 5.1 workbench texture material; batch1 and targeted overhead/sweep, rear and side frames | Contact sheets pending | Not an engine or device test |
| 00:08 | Render color key poses | Blender 5.1 enemy_poses.py | 78 textured stills for batch1, sweep, overhead and running, rear/side | Sheet assembly and human review pending |
| 00:08 | Assemble contact sheet | Pillow, textured Blender stills and action labels | Combined image pending | Visual audit pending |
| 00:08 | Assemble contact sheet | Pillow | contact_sheet.jpg saved | Human visual audit pending |
| 00:08 | Inspect contact sheet | Codex visual review of contact_sheet.jpg | Skeletal poses change, textured; jump-over-obstacle exits camera frame, targeted sweep does not visibly show leg sweep in sampled frames; overhead strike unproven; sheet dark | REWORK: moving proof, framing, semantics and contacts |
| 00:08 | Split second batch | Blender 5.1 modified enemy_split.py for batch2 | Individual action GLBs/metadata pending | Validation pending |
| 00:08 | Split second batch | Blender 5.1 enemy_split_batch2.py | batch2_clip_00..09.glb and batch2_actions.json saved | Reimport and semantics pending |
| 00:08 | Render moving benchmark | Blender 5.1 texture workbench; 16 sampled frames each rear and side of provider run; no object rotation | GIF pending | Loop/contact not yet assessed |
| 00:09 | Moving benchmark failed | Blender 5.1 enemy_motion.py | Empty scene has no world; AttributeError before render | Repair by creating world |
| 00:09 | Repair and rerender moving benchmark | Blender enemy_motion.py world created | 32 rear/side textured frames saved | GIF assembly pending |
| 00:09 | Assemble moving benchmark | Pillow from 16 sampled rear/side run frames | GIF pending | Visual audit pending |
| 00:09 | Moving benchmark assembled | Pillow | running_benchmark.gif | Visual audit pending; sampled playback approximate |
| 00:09 | Poll hook retarget | Meshy 01a1d0ba-61dc-74f3-972b-e32aac8bc033 | SUCCEEDED | Download pending |
| 00:09 | Download hook | Meshy 01a1d0ba-61dc-74f3-972b-e32aac8bc033 | GLB and FBX pending | Audit pending |
| 00:09 | Download hook | Meshy 01a1d0ba-61dc-74f3-972b-e32aac8bc033 | hook.glb, hook.fbx saved | Audit pending |
| 00:09 | Write coverage and evidence checkpoint | Codex; provider action metadata and task IDs; missing and unverified roles explicitly REWORK | JSON pending | No runtime or device validation |
| 00:09 | Coverage and evidence checkpoint written | Codex | animation_coverage.json, worker_evidence.json | All roles explicitly REWORK pending actual audit |
| 00:10 | LOD weight transfer attempt | Blender 5.1 nearest polygon interpolated weights from Meshy rig to existing 2878-triangle LOD, scale aligned by measured height | New benchmark LOD file pending; original retained | Deformation and correspondence audit required |
| 00:10 | LOD weight transfer completed | Blender 5.1 enemy_lod_transfer.py | lod_rig_candidate.glb, 3452 vertices, limit 4 weights, 371 weights limited; scale 1.05309 | Candidate only: verify alignment, skinning and clip compatibility |
| 00:10 | Measure running deformation and candidate LOD reimport | Blender 5.1; evaluate mesh frames and armature root displacement | JSON pending | Contact and correspondence still require review |
| 00:11 | Deformation script failed | Blender enemy_deform.py | Blender vertex collection slicing rejected | Repair sampling iteration |
| 00:11 | Deformation and LOD reimport audit | Blender 5.1 enemy_deform.py | Run sampled vertex change 1.225 m; 24 bones, no unweighted vertices. LOD candidate reimports 2878 tris, 24 bones, max 4 weights, zero unweighted | LOD animated deformation and root/contact remain unverified |
| 00:11 | Test LOD with run action | Blender 5.1 assign same named Meshy skeleton action to imported transferred LOD | Animated candidate GLB and metrics pending | Alignment/contact audit pending |
| 00:11 | LOD run test failed | Blender 5.1 enemy_lod_run.py | Bone-name sets equal but sampled vertex delta zero after action assignment; lod_run_candidate.glb is not valid animated proof | REWORK: repair action slot/retarget; do not accept candidate |
| 00:11 | Repair LOD action binding | Blender 5.1 enemy_lod_run.py; explicitly assign imported action slot | Pending | Sampled deformation and view required |
| 00:12 | Repair LOD action binding | Blender 5.1 enemy_lod_run.py; explicit source action slot | Sampled vertices move up to 1.709 m, same bone names, updated lod_run_candidate.glb and lod_run_metrics.json | Visual quality and contact/loop checks pending; large motion needs inspection |
| 00:12 | Render LOD animated candidate | Blender 5.1 rear/side textured sampled frames | Pending visual audit | No engine claims |
| 00:12 | LOD animated visual audit | Blender rear/side frames, lod_motion_side_08.png | Severe collapsed/stretched geometry; candidate fails deformation | REWORK; do not use; original unmodified |
| 00:12 | Repair LOD parent transform | Blender 5.1 enemy_lod_transfer.py; preserve world matrix when parenting to rig | Pending rerun and visual audit | Candidate remains REWORK |
| 00:13 | Repair LOD parent transform and rerender | Blender enemy_lod_transfer.py, enemy_lod_run.py, enemy_lod_motion.py | Preserving world matrix fixes catastrophic collapse in sampled side frame; updated candidates and 32 frames | Full deformation, contact and skeleton audit remain REWORK |
| 00:13 | Parent rejects manual LOD transfer | Parent direction after observed collapsed result; preserving candidate artifacts and recorded failure | Do not use manual LOD candidate, even though transform repair showed plausible sample | Meshy separate LOD rig and retarget required |
| 00:13 | Submit existing LOD for rigging | Meshy automatic biped rig, ZMB-01_zombie_horde_LOD3k.glb, height 1.9 m; 5 credits | Submission pending | Measure post-rig triangles, skeleton and animation |
| 00:13 | LOD provider rig submitted | Meshy 01a1d0c0-291c-76db-b21d-2b4f8d72d90b | Pending | Separate skeleton until compatibility proved |
| 00:13 | Poll LOD provider rig | Meshy 01a1d0c0-291c-76db-b21d-2b4f8d72d90b | SUCCEEDED | Download and inspect pending |
| 00:13 | Download LOD provider outputs | Meshy 01a1d0c0-291c-76db-b21d-2b4f8d72d90b | Rig/walk/run GLB and FBX pending | Audit pending |
| 00:13 | Download LOD provider outputs | Meshy 01a1d0c0-291c-76db-b21d-2b4f8d72d90b | lod_provider rig/walking/running FBX/GLB saved | Audit pending |
| 00:13 | Audit provider LOD | Blender 5.1 import metric script and textured run frames | Pending | Retarget clips only after inspection |
| 00:14 | Provider LOD metric and visual audit | Blender 5.1 enemy_lod_provider_inspect.py and textured rear/side frames | 2878 triangles, 24 bones, maximum 4 weights, zero unweighted; run sampled deformation 1.166 m. Side frame 08 intact | Still require full loop/contact and skeleton compatibility audit |
| 00:14 | Submit LOD sweep retarget | Meshy LOD rig 01a1d0c0-291c-76db-b21d-2b4f8d72d90b, motion 01a1d0b8-847b-7453-b2d4-557fb522485c; 3 credits | Pending | Provider bind, visual audit required |
| 00:14 | LOD sweep retarget submitted | Meshy 01a1d0c1-5b21-7313-aa8a-1e36c7976c38 | Pending | Audit pending |
| 00:14 | Submit LOD overhead retarget | Meshy LOD rig 01a1d0c0-291c-76db-b21d-2b4f8d72d90b, motion 01a1d0b8-abc7-72c7-a566-403ce845053e; 3 credits | Pending | Provider bind, visual audit required |
| 00:14 | LOD overhead retarget submitted | Meshy 01a1d0c1-8985-74da-941a-257cf74b3847 | Pending | Audit pending |
| 00:14 | Submit LOD hook retarget | Meshy LOD rig 01a1d0c0-291c-76db-b21d-2b4f8d72d90b, motion 01a1d0b8-d4c5-7187-949f-9afbba0037d5; 3 credits | Pending | Provider bind, visual audit required |
| 00:14 | LOD hook retarget submitted | Meshy 01a1d0c1-b741-77e1-9124-6a7a7d5b4c5c | Pending | Audit pending |
| 00:14 | Submit LOD library batch | Meshy LOD rig 01a1d0c0-291c-76db-b21d-2b4f8d72d90b; IDs 14 run 2, 113 mummy stagger, 344 stand up 1, 471 jump over obstacle, 260 push forward stop, 261 push and walk, 49 motivational cheer, 59 victory cheer, 61 happy jump, 22 funny dancing; 30 credits | Pending | Semantics and deformation audit needed |
| 00:14 | LOD library batch submitted | Meshy 01a1d0c1-f881-7108-9491-f861cf97a448; actions 14,113,344,471,260,261,49,59,61,22 | Pending | Audit pending |
| 00:15 | Poll LOD motions | Meshy sweep 01a1d0c1-5b21-7313-aa8a-1e36c7976c38, overhead 01a1d0c1-8985-74da-941a-257cf74b3847, hook 01a1d0c1-b741-77e1-9124-6a7a7d5b4c5c all succeeded; batch 01a1d0c1-f881-7108-9491-f861cf97a448 59% | Download three; poll batch | No semantic acceptance |
| 00:15 | Download LOD targeted motions | Meshy LOD sweep, overhead, hook jobs above | GLB and FBX pending | Audit pending |
| 00:15 | Download LOD targeted motions | Meshy jobs above | lod_provider_sweep, overhead, hook GLB/FBX saved | Audit pending |
| 00:15 | Update checkpoint metadata | Codex; provider IDs, action metadata, LOD job results | Coverage/evidence update pending | All roles remain REWORK |
| 00:15 | Updated checkpoint metadata | Codex | animation_coverage.json and worker_evidence.json now include provider LOD and candidate action IDs/duration/fps | All roles REWORK |
| 00:15 | Submit targeted crowd clap | Meshy prime 2 s prompt: A goofy cartoon zombie stands in place and claps both hands enthusiastically in a repeated audience applause loop. | Pending; 10 credits | Retarget and audit pending |
| 00:15 | Crowd clap submitted | Meshy prime 01a1d0c2-f8d1-760f-8ed6-38f20c82789e | Pending | Retarget and audit pending |
| 00:15 | Submit targeted fence push | Meshy prime 2 s prompt: A cartoon zombie leans forward, plants both hands against a stationary fence at chest height and repeatedly pushes it, feet planted. | Pending; 10 credits | Retarget and audit pending |
| 00:15 | Fence push submitted | Meshy prime 01a1d0c3-2518-77d7-9345-8572389bd3f7 | Pending | Retarget and audit pending |
| 00:15 | Submit targeted dodge | Meshy prime 2 s prompt: A cartoon zombie makes one quick sideways hop to its left, lands on both feet and returns to a ready stance. | Pending; 10 credits | Retarget and audit pending |
| 00:15 | Left hop submitted | Meshy prime 01a1d0c3-5005-7706-8dea-02a56030206b | Pending | Retarget and audit pending |
| 00:16 | Poll LOD batch and new motions | Meshy  01a1d0c1-f881-7108-9491-f861cf97a448 95%; clap 01a1d0c2-f8d1-760f-8ed6-38f20c82789e and fence 01a1d0c3-2518-77d7-9345-8572389bd3f7 succeeded; hop 01a1d0c3-5005-7706-8dea-02a56030206b 12% | Retarget completed motions | Audit pending |
| 00:16 | Retarget clap to LOD | Meshy LOD rig 01a1d0c0-291c-76db-b21d-2b4f8d72d90b; prime 01a1d0c2-f8d1-760f-8ed6-38f20c82789e; 3 credits | Pending | Audit pending |
| 00:16 | LOD clap retarget submitted | Meshy 01a1d0c3-b01f-7376-8d10-e5dd419a655a | Pending | Audit pending |
| 00:16 | Retarget fence push to LOD | Meshy LOD rig 01a1d0c0-291c-76db-b21d-2b4f8d72d90b; prime 01a1d0c3-2518-77d7-9345-8572389bd3f7; 3 credits | Pending | Audit pending |
| 00:16 | LOD fence retarget submitted | Meshy 01a1d0c3-de78-76e2-9709-79a39b07f0c3 | Pending | Audit pending |
| 00:16 | Assemble provider LOD motion preview | Pillow, 16 rear/side color frames from provider run | GIF pending | Loop/contact inspection pending |
| 00:16 | Provider LOD preview assembled | Pillow | lod_provider_running_benchmark.gif | Visual and contact audit pending |
| 00:16 | Poll jobs | Meshy LOD batch 01a1d0c1-f881-7108-9491-f861cf97a448, left hop motion 01a1d0c3-5005-7706-8dea-02a56030206b, LOD clap 01a1d0c3-b01f-7376-8d10-e5dd419a655a, LOD fence 01a1d0c3-de78-76e2-9709-79a39b07f0c3 all succeeded | Download and retarget hop | Audit pending |
| 00:16 | Submit LOD hop retarget | Meshy LOD rig 01a1d0c0-291c-76db-b21d-2b4f8d72d90b, motion 01a1d0c3-5005-7706-8dea-02a56030206b; 3 credits | Pending | Audit pending |
| 00:16 | LOD hop retarget submitted | Meshy 01a1d0c4-9e53-7599-9918-2cb71cc35dda | Pending | Audit pending |
| 00:16 | Download LOD batch, clap, fence | Meshy completed jobs 01a1d0c1-f881-7108-9491-f861cf97a448, 01a1d0c3-b01f-7376-8d10-e5dd419a655a, 01a1d0c3-de78-76e2-9709-79a39b07f0c3 | GLB and FBX pending | Audit pending |
| 00:17 | Download LOD batch, clap, fence | Meshy jobs above | lod_provider_batch, clap, fence GLB/FBX saved | Audit pending |
| 00:17 | Submit enemy hop retarget | Meshy rig 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42, motion 01a1d0c3-5005-7706-8dea-02a56030206b; 3 credits | Pending | Audit pending |
| 00:17 | Enemy hop retarget submitted | Meshy 01a1d0c5-161b-73d6-8300-62e0a2d53135 | Pending | Audit pending |
| 00:17 | Submit Ooh reaction | Meshy prime 2 s prompt: Cartoon zombie spectator recoils in surprise, raises both hands to mouth, then returns to standing. | Pending; 10 credits | Retarget and audit pending |
| 00:17 | Ooh motion submitted | Meshy prime 01a1d0c5-5bbb-7758-a168-4587eab1b902 | Pending | Retarget and audit pending |
| 00:17 | Submit mob motion | Meshy prime 2 s prompt: Cartoon zombie leans forward, reaches and grabs repeatedly toward a person in front, in place, no gore. | Pending; 10 credits | Retarget and audit pending |
| 00:17 | Mob motion submitted | Meshy prime 01a1d0c5-84a7-7188-b402-f46ef8c557c2 | Pending | Retarget and audit pending |
| 00:17 | Poll hop and crowd motions | Meshy LOD hop 01a1d0c4-9e53-7599-9918-2cb71cc35dda, enemy hop 01a1d0c5-161b-73d6-8300-62e0a2d53135, Ooh motion 01a1d0c5-5bbb-7758-a168-4587eab1b902, mob motion 01a1d0c5-84a7-7188-b402-f46ef8c557c2 all succeeded | Download hop and retarget crowd | Audit pending |
| 00:17 | Retarget Ooh to LOD | Meshy LOD rig 01a1d0c0-291c-76db-b21d-2b4f8d72d90b, motion 01a1d0c5-5bbb-7758-a168-4587eab1b902; 3 credits | Pending | Audit pending |
| 00:17 | LOD Ooh retarget submitted | Meshy 01a1d0c5-e716-75b7-9ca9-aa7eb99597f6 | Pending | Audit pending |
| 00:17 | Retarget mob to LOD | Meshy LOD rig 01a1d0c0-291c-76db-b21d-2b4f8d72d90b, motion 01a1d0c5-84a7-7188-b402-f46ef8c557c2; 3 credits | Pending | Audit pending |
| 00:17 | LOD mob retarget submitted | Meshy 01a1d0c6-18a2-7668-9d11-1f4b173871f1 | Pending | Audit pending |
| 00:17 | Download high and LOD hop | Meshy 01a1d0c4-9e53-7599-9918-2cb71cc35dda and 01a1d0c5-161b-73d6-8300-62e0a2d53135 | GLB and FBX pending | Audit pending |
| 00:18 | Download high and LOD hop | Meshy jobs above | enemy_hop_left and lod_provider_hop_left GLB/FBX saved | Audit pending |
| 00:18 | Split LOD provider batch | Blender 5.1 enemy_split.py adapted to LOD | Individual GLBs and metadata pending | Playback and skeleton audit required |
| 00:20 | Split LOD provider batch | Blender 5.1 enemy_lod_split.py | 10 individual GLBs and lod_provider_batch_actions.json saved | Playback/semantic audit pending |
| 00:20 | Poll LOD crowd retargets | Meshy Ooh 01a1d0c5-e716-75b7-9ca9-aa7eb99597f6 and mob 01a1d0c6-18a2-7668-9d11-1f4b173871f1 | Both succeeded | Download and audit pending |
| 00:20 | Download LOD Ooh and mob | Meshy jobs above | GLB and FBX pending | Audit pending |
| 00:20 | Download LOD Ooh and mob | Meshy jobs above | lod_provider_ooh and lod_provider_mob GLB/FBX saved | Audit pending |
| 00:20 | Render LOD candidate key poses | Blender 5.1 provider batch plus targeted clap, fence, Ooh, mob, hop, overhead, sweep, hook; textured rear/side | Contact frames pending | Not yet semantic acceptance |
| 00:21 | Render LOD candidate key poses | Blender 5.1 enemy_lod_poses.py | 108 textured frames saved | Contact sheet and semantic audit pending |
| 00:21 | Assemble LOD contact sheet | Pillow; labelled provider frames | Pending | Visual audit pending |
| 00:21 | LOD contact sheet assembled | Pillow | lod_provider_contact_sheet.jpg | Visual audit pending |
| 00:21 | Inspect LOD contact sheet | Codex visual review lod_provider_contact_sheet.jpg | Provider run, get-up, cheer and dance poses visible; target clap subtle, Ooh lacks hands-to-mouth, sweep lacks obvious sweep, fence hands not proven fixed; obstacle/push can move out of framing | All candidate semantics/contact/timing remain REWORK; inspect motion before acceptance |
| 00:22 | Submit targeted right hop | Meshy prime 2 s prompt: Cartoon zombie takes a short leap sideways to its right with both feet off the ground, lands and recovers to a fighting stance. | Pending; 10 credits | Retarget and audit pending |
| 00:22 | Right hop submitted | Meshy prime 01a1d0c8-2d70-7196-92e5-c4e476745c7d | Pending | Retarget and audit pending |
| 00:22 | Submit improved sweep | Meshy prime 2 s prompt: Fighter squats deeply, extends one leg straight sideways near the floor, rotates hips to sweep the extended leg across the floor in a wide arc, then stands. | Pending; 10 credits | Retarget and audit pending |
| 00:22 | Improved sweep submitted | Meshy prime 01a1d0c8-5dd8-718a-9f0d-296841581353 | Pending | Retarget and audit pending |
| 00:22 | Poll rework motions | Meshy right hop 01a1d0c8-2d70-7196-92e5-c4e476745c7d and sweep 01a1d0c8-5dd8-718a-9f0d-296841581353 | Both succeeded | Retarget pending |
| 00:22 | Retarget improved sweep | Meshy duel rig 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42; motion 01a1d0c8-5dd8-718a-9f0d-296841581353; 3 credits | Pending | Visual audit pending |
| 00:22 | Improved sweep retarget submitted | Meshy 01a1d0c8-b860-77ec-a738-aca334aa589c | Pending | Audit pending |
| 00:22 | Retarget right hop | Meshy duel rig 01a1d0b5-54ee-7747-b6cf-be1b5a6efe42; motion 01a1d0c8-2d70-7196-92e5-c4e476745c7d; 3 credits | Pending | Visual audit pending |
| 00:22 | Right hop retarget submitted | Meshy 01a1d0c8-e6eb-7286-8de9-d60556784fbd | Pending | Audit pending |
| 00:22 | Bounded audit direction | Parent inspected LOD sheet; prioritize low sweep and mid hook readability, front and side contact storyboard | Sweep reroll already submitted once; no endless rerolls | Honest handoff if semantics remain wrong |
| 00:22 | Poll rework retargets | Meshy sweep 01a1d0c8-b860-77ec-a738-aca334aa589c and right hop 01a1d0c8-e6eb-7286-8de9-d60556784fbd | Both succeeded | Download and inspect pending |
| 00:22 | Download rework clips | Meshy sweep and right hop jobs above | GLB/FBX pending | Visual audit pending |
| 00:22 | Download rework clips | Meshy jobs above | sweep_rework and enemy_hop_right GLB/FBX saved | Visual audit pending |
| 00:23 | Render attack storyboards | Blender 5.1; hook.glb and sweep_rework.glb; 8 uniformly timed frames front/side | Pending | Direction/contact semantic audit |
| 00:24 | Render attack storyboards | Blender enemy_attack_story.py | 32 front/side color frames, attack_storyboard_timing.json; UE +X mapping unverified | Sheet and contact interpretation pending |
| 00:24 | Assemble attack storyboards | Pillow; frame timestamps sourced from Blender | Pending | Visual audit pending |
| 00:24 | Attack storyboard assembled | Pillow | attack_storyboard.jpg | Visual audit pending |
| 00:24 | Inspect attack storyboard | Codex visual review attack_storyboard.jpg | Sweep rework extends leg while torso tips/flips around 0.83-1.13 s; horizontal near-ground contact not established. Hook twists and reaches, no clear chest-height impact moment. Source +X facing and target retime unverified | Both remain REWORK; bounded handoff instead of further reroll |
| 00:24 | Update evidence to reflect provider LOD and attack failure | Codex; worker_evidence.json and animation_coverage.json | Removed stale LOD unrigged statement; attack storyboard and precise rework recorded | Parent audit pending |

