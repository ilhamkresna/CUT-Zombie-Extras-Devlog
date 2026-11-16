# 2026-11-16 06:09 WIB - eandroid - Android compile fix: wrong OnlineSubsystem header name

- **Who / tool:** Sonnet worker "E-androidfix", Claude Sonnet 5.5 in Claude Code
- **Task:** Release blocker, Android compile of JamGame fails on `Interfaces/OnlineLeaderboardsInterface.h` not found - **Branch / commit:** main, uncommitted
- **Asked for:** Find why the Android build cannot find the OnlineSubsystem header, make the minimal fix, verify with an Android-only compile (no cook, no packaging).
- **Result:** Build.bat JamGame Android Development now Result: Succeeded. Three fixes: (1) wrong header name in JamAndroidServicesSubsystem.cpp (OnlineLeaderboardsInterface.h -> OnlineLeaderboardInterface.h, singular in UE 5.8); (2) 5.8 API drift in the same file: LeaderboardNames.Add/RatedStat take FString, flush-complete handler takes FName by value (cpp + Android-guarded .h declaration); (3) removed invalid top-level irebaseCrashlytics {} Gradle block from Source/JamGame/JamGame_ANDROID_UPL.xml (Gradle error line 34).
- **Verified by:** Android compile + Gradle assembleDebug, log Saved/Logs/e_android_compile_20261116.log, Result: Succeeded, 319 s. Win64 not built.
- **What went wrong:** Three layered failures, each hidden by the previous one (header, then API drift, then Gradle). Hypothesis from the brief (missing OnlineSubsystem module / plugin) was wrong. Build.cs already lists OnlineSubsystem + OnlineSubsystemGooglePlay for Android and the uproject enables OnlineSubsystemGooglePlay for Android.
- **Corrected by:** self; searched the engine tree: the header is named `OnlineLeaderboardInterface.h` (singular) in UE 5.8, at Plugins/Online/OnlineSubsystem/Source/Public/Interfaces/.
- **Human decision needed or taken:** none
- **Not done / not verified:** Win64 editor target not built by this worker (editor in use). No cook, no packaging, no device test.
- **Screenshot:** none

## Operation ledger (append while working)
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 06:09 | Audit: searched UE_5.8 for OnlineLeaderboard*.h; checked Build.cs, uproject | Sonnet 5.5, E-androidfix | Only `OnlineLeaderboardInterface.h` (singular) exists in OnlineSubsystem/Public/Interfaces; Build.cs/uproject already correct | Found, not yet fixed |
| 06:10 | Edit JamAndroidServicesSubsystem.cpp line 15: include `OnlineLeaderboardsInterface.h` -> `OnlineLeaderboardInterface.h` | Sonnet 5.5 | Edit applied | Compile pending |
| 06:11 | Android compile #1 (`Build.bat JamGame Android Development`) | UBT, log Saved/Logs/e_android_compile_20261116.log | FAILED, but header now found; 3 new errors surfaced in same file: LeaderboardNames.Add(FName), RatedStat = FName (5.8 uses FString wrappers), CreateUObject signature (delegate param is `FName` by value, handler had `const FName&`) | Failed (visible) |
| 06:13 | Edit JamAndroidServicesSubsystem.cpp lines 334-335 (FName -> FString), cpp line 292 + .h line 59 handler param `const FName&` -> `FName` (header decl is inside `#if PLATFORM_ANDROID`) | Sonnet 5.5 | Edits applied | Compile pending |
| 06:13 | Android compile #2 (same command, same log path, overwritten) | UBT | All 9 C++ actions compiled incl. JamAndroidServicesSubsystem.cpp (C++ blocker fixed). Build then FAILED in the Gradle APK step: `Script 'Z:\app\buildAdditions.gradle' line: 34 ... Could not find method firebaseCrashlytics()` (top-level `firebaseCrashlytics {}` block from JamGame_ANDROID_UPL.xml is invalid in Crashlytics plugin 3.x; extension lives on build types) | Failed (visible), new blocker |
| 06:15 | Edit Source/JamGame/JamGame_ANDROID_UPL.xml: removed the top-level `firebaseCrashlytics { nativeSymbolUploadEnabled false }` block (default is no native symbol upload anyway) | Sonnet 5.5 | Edit applied | Compile pending |
| 06:19 | Android compile #3 (same command and log path) | UBT + Gradle | Result: Succeeded (319 s). C++ compiled, Gradle `:app:assembleDebug` BUILD SUCCESSFUL, code-only debug APK step completed (no cook, no BuildCookRun) | Android compile PASS |
