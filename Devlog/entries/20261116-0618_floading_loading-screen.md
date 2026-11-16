# 2026-11-16 06:18 WIB — floading — Android launch splash (clapperboard logo on dark night)

- **Who / tool:** worker "F-loading", Claude Sonnet in Claude Code; Python/PIL script (no image model used)
- **Task:** free text (owner spec: loading screen = existing clapperboard logo centered on dark night background, nothing else) · **Branch / commit:** main, uncommitted
- **Asked for:** (1) Android launch splash from the existing logo, enable bShowLaunchImage; (2) in-game loading overlay only if quick and safe.
- **Result:** `Build/Android/res/drawable/splashscreen_portrait.png` (1080x1920) and `splashscreen_landscape.png` (1920x1080): `T_UI_Logo` source PNG resized to its true aspect (1.4328, 860x600) centered on a dark navy gradient with a soft vignette. Filenames match the engine's `Engine/Build/Android/Java/res/drawable/` (splashscreen_portrait.png / splashscreen_landscape.png, referenced by splashscreen_p.xml / splashscreen_l.xml). `Config/DefaultEngine.ini`: added `bShowLaunchImage=True` (plus a comment) in the AndroidRuntimeSettings section; no other keys touched. In-game overlay SKIPPED: the project has a single map (L_Run, title is a widget in it); the only level transition is `RetryRun` (OpenLevel same map), which destroys widgets, and a persistent loading overlay across OpenLevel needs the MoviePlayer module (JamGame.Build.cs change) = rebuild risk.
- **Verified by:** nothing on device. Visual check of the PNG preview only (`Devlog/media/20261116_floading_1.png`, half-size portrait preview). Filenames checked against the engine folder by listing.
- **What went wrong:** nothing technical. Note: `/Build/` is in `.gitignore`, so the PNGs are not committed; they exist on disk and UBT reads project `Build/Android/res` at package time. The engine default for bShowLaunchImage is already true; the explicit key only pins it.
- **Corrected by:** n/a
- **Human decision needed or taken:** none. If the splash must survive a clean checkout, force-add or relocate the PNGs.
- **Not done / not verified:** splash appearance on a real device / packaged APK; in-game loading overlay (skipped, see above); no editor or compile used.
- **Screenshot:** `Devlog/media/20261116_floading_1.png`

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 06:17 | Composite splash PNGs (portrait + landscape) | Python PIL from raw_assets/ui_w3/out/T_UI_Logo.png; no generation | Build/Android/res/drawable/splashscreen_portrait.png, splashscreen_landscape.png; preview Devlog/media/20261116_floading_1.png | Preview eyeballed; device pending |
| 06:18 | Config/DefaultEngine.ini: bShowLaunchImage=True | Edit tool | key added in [/Script/AndroidRuntimeSettings.AndroidRuntimeSettings] | text diff only; packaged build pending |
| 06:19 | asset_manifest.csv row LOAD-SPLASH-20261116 | shell append | row added (source: T_UI_Logo / fal nano-banana-2.1, derived) | n/a |
| 06:20 | In-game overlay evaluated and skipped | grep of OpenLevel/travel + Build.cs | single map; needs MoviePlayer module = Build.cs change | skipped by rule |

No keys, account identifiers, signed URLs or private perk codes. Log self-edits are covered by this entry; do not recurse.
