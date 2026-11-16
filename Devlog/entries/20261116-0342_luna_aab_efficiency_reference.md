# 2026-11-16 03:42 WIB — Luna — AAB efficiency reference

- **Who / tool:** Luna explorer; PowerShell read-only inspection of `E:\Project Game\TOWER DEFENSE ANDROID` and the current Epic MegaJam project.
- **Goal / prompt:** Read the reference project's `AGENTS.md`/`CLAUDE.md`, build scripts, release docs/Devlogs, and relevant Claude sessions if present. Extract real transferable UE Android SDK/NDK/Gradle incremental/cook/package/cache/`-NoUBA`/signing lessons, distinguish its engine/project assumptions, and write a concise reference report in the current Epic project. Do not read `third_party`, secrets or modify the reference repo. Keep the active Firebase/Play ownership scope coordinated; native builds remain with the separate visuals owner.
- **Result:** Created `tasks/AAB-EFFICIENCY-REFERENCE-20261116.md`. The reference is Godot 4.6.1, not Unreal. Its notes document a real prior AAB (1.15 GB) and emulator smoke test, while the earlier release README still says AAB/SDK blocked. The specific template, Gradle AAR, signing preset and export steps are not transferable to UE; only toolchain pinning, cache preservation, minimizing unused assets and artifact smoke tests are.
- **Verified by:** Read-only file inspection of the listed docs/scripts and exact `production/reports/alpha-build-1-notes-2026-10-11.md`; confirmed no `CLAUDE.md` and only `.claude/launch.json`, so no local Claude session transcript was available. Did not execute scripts/builds/tests or print private keystore/SHA material.
- **What went wrong / limits:** Godot documentation contradicts its dated actual-build report. It provides no UE `-NoUBA` guidance. No current reference-project build was attempted; repo instructions explicitly require owner authorization for Android build/install.
- **Still unverified:** Current SDK setup, current AAB reproducibility, and exact UE package-size filter behavior (must inspect the Epic project's own pipeline).

## Operation ledger

| Time WIB | Operation | Prompt/model/tool | Actual outcome |
|---|---|---|---|
| 03:40 | Read reference instructions and release kit | Luna / PowerShell | Read AGENTS, technical preferences, laptop migration guide, release README, plugin/template scripts and static verifier; no CLAUDE.md found. |
| 03:41 | Read actual alpha-build notes and local Claude directory | Luna / PowerShell | Found recorded AAB size/emulator notes; no Claude transcripts in reference project. No script/build run. |
| 03:42 | Synthesize transferable steps and report | Luna / PowerShell | Created `tasks/AAB-EFFICIENCY-REFERENCE-20261116.md`; no source/config/artifact edits and no manifest entry. |
