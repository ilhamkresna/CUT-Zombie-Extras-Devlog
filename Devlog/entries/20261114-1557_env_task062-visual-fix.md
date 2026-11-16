# 2026-11-14 15:57 WIB (machine clock) — env — TASK-062 visual fix (textures, night lighting, ground, composition)

- **Who / tool:** visual-fix worker, Claude Haiku 5.5 in Claude Code (subagent of the TASK-062 orchestrator); UE 5.8 Python headless (`UnrealEditor-Cmd -run=pythonscript`), game launched with `UnrealEditor.exe -game -windowed -ResX=540 -ResY=960 -ExecCmds="Jam.Start"`, PrintWindow capture.
- **Task:** TASK-062 visual fix (orchestrator review of `20261114_task062_density_2.png`) · **Branch / commit:** main, see commit "TASK-062 visual fix: ..." (hash in the report).
- **Asked for:** (1) Meshy props show their textures; (2) night lighting applied; (3) ground dark asphalt; (4) composition: poles 1600 cm, tall clutter off the sidewalk, facades closer and continuous; screenshots v2 at about 6 s and 15 s.
- **Result:**
  - **Textures:** the parent material graph is not readable from Python (`expressions` is protected). New shared parent `Content/Art/Env/Meshy/M_MeshyProp` (Lit, TextureSampleParameter2D "BaseColor" to Base Color, Roughness 0.8, Specular 0.2). All 212 `MI_*` under `/Game/Art/Env/Meshy` reparented to it and set to their `T_<stem>_BC`; read-back verified (`raw_assets/MESHY-ENV-PREP/ue_mi_texture_fix_report.json`: fixed 212, unmatched 0, verify_failed 0, failed 0). Script: `tools/ue/fix_meshy_mi_textures.py`.
  - **Lighting:** `tools/ue/apply_night_lighting.py` rewritten with 5.8 names (`fog_inscattering_luminance`, `directional_inscattering_luminance`; the old `inscattering_color` never existed). Fog density 0.03, falloff 0.2, start 2500 cm; moon 2.0 lux, (185,166,255), no shadows; SkyAtmosphere rayleigh scale 0.05 with purple tint; SkyLight 0.6 navy lower hemisphere; one unbound PostProcessVolume, saturation 1.05, auto exposure (histogram) with -1 EV compensation; 0 point or spot lights in L_Run.
  - **Ground:** `M_GroundNight` (new, Lit, VectorParameter "BaseColor", ScalarParameter "Roughness") built; `MI_Ground_Night` parented to it, colour (42,35,56) sRGB as linear (0.0232, 0.0168, 0.0395), roughness 0.95, read back.
  - **Composition (data only, DA_ArtSet_046, no C++ change, script `tools/ue/tune_task062_composition.py`):** pole spacing 1000 to 1600 cm; facade inner offset 800 to 450 cm (front at |Y| 900 = lane edge 450 + 450); parking inner offset 400 to 350 cm; BackFacadeScale 1.2 to 1.5; vegetation 0..2 to 0..1. Clutter over 250 cm tall (40 pieces across 4 sets, e.g. StudioSpotlight 580 cm, PhoneBooth 519 cm) moved to CrowdProps (L2). Kept clutter is at most 250 cm. Report: `raw_assets/MESHY-ENV-PREP/tune_task062_composition_report.json`.
  - **Screenshots (540x960, PrintWindow, client area):** `Devlog/media/20261114_task062_density_v2_1.png` (run, about 6.2 s after launch), `Devlog/media/20261114_task062_density_v2_2.png` (run, about 9.4 s); `Devlog/media/20261114_task062_density_v2_contact.png` (14 frames, 3.0 to 13.4 s).
- **Verified by:** headless UE Python runs exit 0 with the per-item read-back above (not compile, not automation). Game frames in the editor-built `-game` session (not packaged, not a device). No C++ changed, so no build and no automation tests were run in this pass.
- **What went wrong:**
  - First lighting run used `inscattering_color` (unset) and manual exposure: the whole run frame was black, HUD only.
  - Manual exposure (PostProcessVolume) at EV 0, +2.5 and +5 stayed black; the bias did not visibly brighten the scene. Reverted to histogram auto exposure with -1 EV compensation, which gives a readable world. This is the third lighting iteration; the brief allowed two. The extra run was a diagnosis, not a tuned result.
  - The earlier pastel frame was most likely auto exposure lifting a very dim scene (sun 2 lux), not the fog.
  - The game run ends about 7 s after Jam.Start in this session (run-over recap shows RUNTIME 00:07); the 15 s frame requested is therefore the wrap screen and was not used. Frame 2 is at about 9.4 s.
- **Corrected by:** self (exposure path above); the two-iteration limit was exceeded by one lighting run, stated here.
- **Human decision needed or taken:** none. Config/DefaultEngine.ini not staged (other work).
- **Not done / not verified:**
  - The frame is still a lavender-purple haze overall, not a deep night. Rayleigh tint and fog luminance are the likely cause; not tuned further.
  - Prop textures: colour accents are visible (red camera rig, green obstacles, lit windows); the facade fronts read grey-violet under the haze. No close-up check of the texture resolution.
  - Facade yaw (front toward the road, `FacadeYawOffsetDeg` 0) is not confirmed in these frames.
  - L2 vehicles/crowd at 350 cm inner offset may overlap the facade row at 450 cm (footprint not measured).
  - Not built, no automation tests run in this pass, not played to a finish, no Android device test, no performance numbers.
  - `populate_task062_dressing.py` rebuilds the layer lists from the manifest: re-running it undoes the clutter move and the layout values. Re-run `tune_task062_composition.py` after it. `import_meshy_env.py` still parents new imports to `M_HeroPreview_BaseColor`; re-run `fix_meshy_mi_textures.py` after any new Meshy import.
- **Screenshot:** `Devlog/media/20261114_task062_density_v2_1.png`, `Devlog/media/20261114_task062_density_v2_2.png`, contact sheet `Devlog/media/20261114_task062_density_v2_contact.png`.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 15:47 | Headless inspect of parent and MIs | UnrealEditor-Cmd -run=pythonscript, scratchpad inspect_mats.py | parent expressions protected; 212 MIs use BaseColorTexture | read-only, verified |
| 15:48 | Headless probe of L_Run lighting props | UnrealEditor-Cmd, scratchpad probe_light.py | 5.8 names confirmed; no PostProcessVolume in L_Run | read-only, verified |
| 15:49 | tools/ue/fix_meshy_mi_textures.py (new M_MeshyProp, 212 MIs) | UnrealEditor-Cmd -run=pythonscript | fixed 212, unmatched 0, verify_failed 0 | script exit 0, read-back verified |
| 15:50 | tools/ue/apply_night_lighting.py rewritten, run | UnrealEditor-Cmd -run=pythonscript | exit 0; M_GroundNight and MI_Ground_Night set | script exit 0, read-back verified |
| 15:52 | Game capture (manual EV 0) | PowerShell PrintWindow, -game 540x960 | world black | failed, reverted |
| 15:53 | Lighting re-run, manual EV +2.5 | UnrealEditor-Cmd -run=pythonscript | black at about 15 s | failed |
| 15:54 | Series capture, 14 frames | scratchpad capture_series.ps1 | black during run | diagnostic |
| 15:55 | Lighting re-run, manual EV +5 | UnrealEditor-Cmd -run=pythonscript | still black | failed |
| 15:56 | Lighting re-run, auto histogram -1 EV | UnrealEditor-Cmd -run=pythonscript | world visible | partly applied |
| 15:55 | tools/ue/tune_task062_composition.py on DA_ArtSet_046 | UnrealEditor-Cmd -run=pythonscript | layout set, 40 tall clutter moved to CrowdProps | script exit 0, report written |
| 15:57 | Screenshots v2 (14-frame series, copied 2 + contact sheet) | PowerShell PrintWindow | run frames at about 6.2 s and 9.4 s | visual check only |
