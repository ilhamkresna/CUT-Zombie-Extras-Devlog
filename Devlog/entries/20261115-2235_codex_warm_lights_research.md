# 2026-11-15 22:35 WIB — Codex warm street-light research

- **Who / tool:** Luna explorer; web search/open of Epic UE 5.8 documentation, firsthand Epic Developer Community threads, MIT GitHub repositories; local read of `Config/DefaultEngine.ini`. No code generation or engine execution.
- **Goal:** research ways to make roadside/fixture lamps cast warm light while preserving the Android Mobile Forward renderer, no Lumen/Nanite, gameplay cue semantics and runtime editability.
- **Result:** recommended a saved editable lamp composition with a warm emissive fixture everywhere and actual unshadowed Point Light components only for the near field, activated through existing segment lifecycle; far fixtures remain emissive/optional glow. Spotlights are conditional and need profile confirmation. No arbitrary light budget was selected.
- **Evidence:** project pins `MobileShadingPath=Forward`; UE 5.8 docs list forward renderer constraints and mobile light settings. Reports cover conditional local light/shadow support, emissive proxy limits, source examples/licenses and forum version caveats. Full URLs and findings are in `tasks/CODEX-WARM-LIGHTS-RESEARCH-20261115.md`.
- **Changes:** created only the research report and this entry. No project source/assets/config changes and no third-party code copied.
- **Verified:** checked official documentation search results/open pages, forum posts, and GitHub README/LICENSE metadata. The MIT repositories were research references only.
- **Still unverified:** no implementation or Android profile/performance test; root validation needed before implementation, and light count/attenuation must come from the actual shipping profile and device profiling.

## Follow-up operation — measured source configuration

| Time WIB | Operation | Evidence | Result |
|---|---|---|---|
| 22:35 | Read-only project renderer and exposure settings | `Config/DefaultEngine.ini`, `Config/Android/AndroidEngine.ini` | Confirmed `MobileShadingPath=Forward`, Android `r.DefaultFeature.AutoExposure=0`, and no explicit project override for local-light count or spotlight support. Research report notes the effective shipping-profile value remains unknown and needs inspection. |
| 22:36 | CCT/source check | UE 5.8 ULightComponent API, physical-lighting docs, DOE/ENERGY STAR, Epic forum / licensed GitHub references | Added the editable 2700–3000 K warm-white preview range, with hazard color separation and camera-transform check; no value applied in the game. |
