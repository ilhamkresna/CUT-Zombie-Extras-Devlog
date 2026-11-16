# TASK-080 — Firebase and Play Games runtime integration

- **Time:** 2026-11-16 04:06 WIB (source freeze; see timestamps in operations below).
- **Owner/tool:** Luna / Codex; PowerShell, `apply_patch`, Firebase CLI, gcloud, official UE/Firebase source inspection.
- **Prompt:** Integrate Firebase Analytics + Crashlytics and Google Play Games login + leaderboard in the existing UE 5.8 Android game. User uploads the AAB. Use real editable console IDs, never fabricate success, and keep credentials private. Browser account setup is authorized only through the existing in-app profile; do not open external browser profiles.
- **Result:** Added Android Firebase bridge, UPL Gradle wiring, Android GameInstance subsystem and editable settings, Google Play auto-login and best-score async flush, and run lifecycle hooks. Retrieved registered Firebase Android config into ignored `Build/Android/google-services.json`; confirmed package match without logging identifiers or keys. Enabled Crashlytics and Analytics Admin APIs. Frozen source was handed to Terra for the combined build.
- **Verification:** Read UE 5.8.4 Android Runtime Settings and Google Play leaderboard interfaces; confirmed async flush callback semantics. Confirmed UPL file-copy syntax against engine Firebase plugin. `git diff --check` passed. No build or device tests run by Luna.
- **Account blockers:** Analytics Admin inventory returns HTTP 403 after API enablement; no Analytics property link was verified. Google Play Games app ID, leaderboard ID and signing SHA-1 are not available/verified. Edge was briefly opened before the owner corrected the browser instruction; operating-system computer use was stopped, and no account setup was done there. Supported in-app browser control was unavailable afterward.
- **Limitations:** Do not report service integration as end-to-end complete until the combined build, signed-device login/leaderboard test and Firebase Analytics/Crashlytics console receipts are observed. The Firebase JSON stays ignored/local. No AAB upload or publish performed.

## Operation ledger

| Time WIB | Operation | Result |
|---|---|---|
| 04:06 | Freeze Java/C++/UPL/module/run-hook source and send hashes to native owner | Terra notified; combined build pending. |
| 04:07:48 | Write TASK-080 report, Devlog and queue row | Documentation complete; `git diff --check` clean for scoped files. |
| 04:08 | Fix first combined-build descriptor failure in `JamGame.uproject` | Replaced invalid `TargetAllowList: ["Android"]` with `PlatformAllowList: ["Android"]`; parsed JSON, confirmed enabled Android plugin and recalculated descriptor hash. Build retry is pending Terra. |
| 04:09 | Fix module rules compile failure reported by Terra | Added the missing `using System.IO;` required for `Path.Combine` in the Android UPL receipt registration. Terra was asked to retry; C++/Java compilation had not started yet. |
| Later checkpoint (exact time not recorded) | Fresh build/lease/worktree checkpoint | UBT third attempt was active; HEAD was `f5d2855`. My relative-path check incorrectly reported the lock absent. Explicit absolute check at 04:21:24 confirmed `Saved/unreal.lock` exists and names `ORCH_TERRA_VISUAL`; no lease action was taken. |
| 04:21:53 | Search for existing Riftborn Play Games IDs in the reference game, targeted Claude history, Firebase CLI and supported account tools | No PGS identifiers or project integration were found in the reference sources; Firebase listed two Riftborn Android app registrations with package namespaces different from CUT. Riftborn Cloud has Games/Android Publisher APIs enabled, but no app/leaderboard enumeration path without a known console application ID. No account UI was configured. IDs were not printed or copied. |
| 05:03 | Implement Blueprint-callable Play Games user actions and title-widget hooks | Added manual sign-in/retry, login/status delegates, mapping-validated leaderboard UI launch and the title widget’s switching action/status bindings. Scoped diff-check passed. No build or editor operation by Luna. |
| 05:03 | Prepare guarded saved-title UMG edit payload | Added `tools/ue/task080_playgames_title_ui.py`; Python AST parse passed. Native owner will inspect/run it under the existing lease. |
| 05:05 | Update TASK-080 report and UI flow draft after the green baseline build | Recorded that build3 at 04:51 predates the new C++ UI changes; next combined build and saved WBP_Title readback remain pending. |

Earlier Firebase CLI, gcloud and engine-source inspection steps were completed in this session; their individual wall-clock times were not recorded, so this log does not assign them times. No generated media/assets were created, so no asset-manifest row applies.
