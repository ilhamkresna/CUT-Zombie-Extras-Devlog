# 2026-11-14 20:25 WIB — art-pipeline — Meshy B15 filler and B16 garage env prep (Blender only)

- **Who / tool:** asset prep worker, Claude Haiku 5.5 in Claude Code; Blender 5.1 CLI (`prep_meshy_env.py`, ad-hoc bpy measure and render scripts in the session scratchpad, not committed)
- **Task:** TASK-070 §E (ground cover and garage source props) · **Branch / commit:** main, see commit below
- **Asked for:** Add 22 B15 ground-cover slots and 4 B16 garage slots to `tools/assets/meshy_env_manifest.json` with real sizes, run the prep, make one contact sheet, list rejects, measure the garage bay opening.
- **Result:** 26 slots added to the manifest (254 to 280). 25 prepped to `raw_assets/MESHY-ENV-PREP/ground_cover/` and `garage_source/` (FBX plus base-colour PNG). `garage_parking_entrance` failed prep: raw GLB bbox is 1.00 x 0.064 x 0.183 m, a sliver that cannot be fitted, so it is rejected and needs regeneration. Contact sheet: `Devlog/media/20261114_meshy_b15_b16_contact.png`.
- **Fit results (bbox after prep, m, Blender Z-up frame):** round bushes 1.1 tall (1.39 and 1.21 wide); tall bush 2.2; hedge 0.94 x 3.00 x 0.69 (height bound short of 1.0 because length 3 m binds); grass 0.60; flower bed 0.58 x 0.49 x 0.40 (2 m length not reached); rocks 0.40; planter 1.0; trash bags 0.8; newspaper 0.4; fire barrel 1.1; sign 1.0; cart 1.0; tyres 0.6; crates 0.7; tumbleweed+cactus 1.0; ranch fence 1.77 x 0.35 x 1.1 (3 m length not reached); mushrooms 0.7; bones 0.5; crystal 1.0; vent 0.6; zombie hand 0.9. Uniform scale means a `length` key only binds when it is the smaller factor, so flower bed, fence and hedge are shorter than the requested length.
- **Garage bays (prepped at 9.5 m facade height):** bus depot 4.36 m wide (Y) x 9.5 m; warehouse dock 9.89 x 10.08 x 9.5 m; studio vehicle bay 13.3 x 13.1 x 9.5 m. Ray-cast bay estimate for the studio bay: about 6.1 m wide x 8.6 m tall, open on both Y ends (matches the render). Warehouse dock: about 9.8 m wide x 9.4 m tall, roll-up door open. Bus depot: unreliable (ray-cast reports about 65 percent open across the full width, which the render does not support). Render suggests about 3.3 m wide, height not measured. Bay opening is NOT verified against the 3.6 m tall x 3.2 m wide requirement for the bus depot; the bus depot needs a proper front-elevation check. Facing: the render puts the open face on the -Y side in Blender coordinates for all three. The mapping from Blender axes to UE axes after FBX export was not verified in UE.
- **Verified by:** Blender prep report (scale and bbox per slot), ray-cast measurement (studio bay, warehouse dock), one contact-sheet render (visual only). Not in UE, not on device.
- **What went wrong:** garage_parking_entrance GLB is degenerate (see above). Hedge, flower bed and fence do not reach their 3 m or 2 m length keys with the uniform-scale rule. Bus depot bay measurement failed. The first contact-sheet framing cut off the second row, re-rendered.
- **Corrected by:** self (re-render with wider camera)
- **Human decision needed or taken:** Decide whether to regenerate `garage_parking_entrance`, and whether the hedge, flower bed and fence should be accepted at their current length. Decide the bus depot bay check.
- **Credits (Meshy, from raw_assets tasks.json):** B15 330 (balance 507 to 177), B16 60. Not regenerated in this session.
- **Not done / not verified:** asset_manifest.csv not updated; garage facing not rotated (prep does not rotate facing); bus depot bay height and width; UE import; the contact-sheet rejects are visual only.
- **Screenshot:** `Devlog/media/20261114_meshy_b15_b16_contact.png`

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| ~20:25 (approx) | Manifest edit | Python json append, B15 22 slots and B16 4 slots | 280 slots, tools/assets/meshy_env_manifest.json | Verified (parsed) |
| ~20:25 (approx) | Prep | blender -b -P tools/blender/prep_meshy_env.py --only (26 slots) | 25 ok, 1 failed (garage_parking_entrance) | Verified (Blender report) |
| ~20:25 (approx) | Garage bay ray-cast audit | bpy ray_cast on prepped FBX | studio bay 6.1 x 8.6 m; bus depot unreliable | Partial |
| ~20:25 (approx) | Contact sheet render | bpy Workbench render, one PNG | Devlog/media/20261114_meshy_b15_b16_contact.png | Visual check only |
