# Dev log entry: TASK-006 Android + Windows packaging (executor)

- **Who / tool:** executor agent, Claude Haiku 5.5 in Claude Code (isolated git worktree).
- **Task + branch/commit:** TASK-006 packaging settings, branch `task/006-packaging`. Commits: e1f2bd7 (config + report), 7f5201b, 608a515 (merge of main), final commit = this entry + report + bSupportAdMob (hash in the report).
- **Asked for:** Android portrait APK with data inside the APK, low-end Android rendering settings (30 FPS, no VSM, no dynamic shadows, no bloom/motion blur/auto exposure), Windows build still working, build times and zip sizes.
- **Result:** Android APK builds (Development, ASTC, arm64), one APK, no .obb. Windows build passes (run before the merge of main only). Zips measured, test zips deleted. Config is in `Config/DefaultEngine.ini` and `Config/Android/AndroidEngine.ini`.
- **Verified by:** real `Build-Jam.ps1` runs. Android: first build 24.0 min, rebuild after bSupportAdMob=False 1.9 min, APK 154,593,734 bytes (147 MB), Android zip 132,411,677 bytes (126 MB). Windows: 1.0 min incremental, 343 MB zip (359,669,620 bytes). Generated manifest checked for AdMob entries.
- **What went wrong:** three Android attempts were blocked by UnrealBuildTool mutex conflicts from other agents' builds (`ConflictingInstance`, and a misleading "Error_SDKNotFound" message). The first Windows cook failed because `Content/Maps/L_Run.umap` was a Git LFS pointer in a fresh worktree. The Android build then failed on a clang `-Werror` error in `JamRunnerCharacter.cpp` (game code, not config). A wait loop I wrote returned early because `tasklist` did not match in Git Bash.
- **Corrected by:** `git lfs pull` in the worktree. Waiting for the mutex with `Wait-Process`. The clang error was fixed by main (TASK-008), so I changed no `Source/` files. The AdMob SDK was in the manifest, so I set `bSupportAdMob=False` and rebuilt.
- **Human decision needed:** confirm that "Hardware target Mobile / Scalable" has no UE 5.8 config key (the closest setting is `MobileShadingPath=Forward`). Confirm that ads are off for this build.
- **Not done / not verified:** no Android device was attached, so no install, no portrait or touch test, no `stat unit` or `stat fps` numbers. Windows was not rebuilt after the merge of main.
- **Screenshot:** none.
