# 2026-11-16 08:06 WIB — nobst — floating High obstacles and unreadable bus doorway

- **Who / tool:** worker "N-obstacles", Claude Sonnet 5.5 in Claude Code; unreal-mcp on the running editor (ObjectTools, StaticMeshTools, SlateInspector Save button); static code reading
- **Task:** owner play-test complaints (obstacles float above the road; bus opening not readable) · **Branch / commit:** main, uncommitted
- **Asked for:** find why obstacles float and why the bus doorway is unclear; fix with data first, tiny C++ only if unavoidable (no compile).
- **Result:** (1) Root cause of floating: the placed-art path (`JamObstaclePlacement::BuildRow`, called from `AJamTrackSegment`) puts every High obstacle mesh at bottom Z = `HighBottomZ` (120 cm). The three High meshes (SM_ObsHighBannerBar 0..380 cm, SM_ObsHighLightRig 0..380, SM_ObsHighWestBeam 0..360) are full-height gantries with pole bases, so the whole gantry hovered 120 cm over the asphalt. Low and Block use bottom 0 and the road deck top is Z 0 (checked in the road GLB), so they were not the cause. (2) Bus: `MI_Bus3CityFrontrear` (used by both FrontOpen and RearOpen meshes) was one-sided; the doorway is a boolean-style notch with no inner cap, so its ceiling and walls are back faces and were culled (see-through / glitchy). Set Two Sided override on that MI.
- **Verified by:** nothing in-game. Numbers read through unreal-mcp (mesh bounds, DA values), asset thumbnails of the meshes, saved asset file timestamp. No compile (rebuild compiles the C++ edit). No PIE: the owner's PIE was running, so none was started.
- **What went wrong:** `AssetTools.save_assets` reported "Asset does not exist" for every path (its exists check fails in this session), so the MI was saved with the asset editor's Save button through SlateInspector instead. First guess (no shadow casting on obstacle meshes) was not the cause for floating and was not changed.
- **Corrected by:** self.
- **Human decision needed or taken:** none. Trigger boxes are unchanged (High trigger stays Z 120..220), only the art moves down to the road.
- **Not done / not verified:** no before/after game screenshot; whether the banner/rig/beam art still reads correctly as a slide-under bar at floor level (banner lower edge about 150 cm in the thumbnail, slide window is 120..220 trigger) is unverified; the two-sided look of the bus notch (lighting of flipped back faces, the door aligned to the pass lane) is unverified; High obstacles through the old fitted path (`AJamObstacle::SetArtVariants`, fallback only) still use HighBottomZ.
- **Screenshot:** `Devlog/media/20261116_nobst_before_1.png` (SM_ObsHighBannerBar: pole with base, full height), `Devlog/media/20261116_nobst_before_2.png` (bus thumbnail). No after images.

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 08:02 | Audit: JamObstacle, JamArtFit, JamObstaclePlacement, JamTrackSegment, DA_ArtSet_046, DA_HazardCatalog (BusDoorway row), DA_ThemeSet, road GLB | read via Read/Grep and unreal-mcp ObjectTools | road deck top Z 0; Low/Block bottom 0; High bottom = HighBottomZ 120 on full-height gantry meshes | read only |
| 08:03 | C++ edit: `JamTrackSegment.cpp` ~line 410 passes `0.f` instead of `ObstacleSlots[0]->HighBottomZ` to `BuildRow` (local `HighBottomZ` variable removed, no unused variable) | Edit tool, no compile | High art now stands on the road; triggers unchanged | not compiled, rebuild pending |
| 08:05 | Data edit: `/Game/Art/Env/Meshy/Bus3/MI_Bus3CityFrontrear` BasePropertyOverrides bOverride_TwoSided false->true, twoSided false->true | ObjectTools.set_properties, read back OK | back faces of the doorway render | saved via asset editor Save button (file timestamp 08:05); not seen in game |
| 08:05 | Evidence captures | EditorAppToolset.CaptureAssetImage | before_1 (banner bar), before_2 (bus) | done |
