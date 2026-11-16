# 2026-11-14 10:15 WIB — logic — TASK-031 duel ambush origins (code only)

- **Who / tool:** logic worker, Claude Sonnet 5.5 in Claude Code (temporary worktree)
- **Task:** TASK-031 · **Branch / commit:** worktree-agent-a673c138e64141fcc, commit "TASK-031: duel ambush origins (not compiled)"
- **Asked for:** implement TASK-031 in C++ without building: hidden enemy, seeded ambush origin, parabolic intro flight, fallback, console hooks, headless tests.
- **Result:** new Duel/JamAmbushRules (pure seeded pick, fallback, flight arc); EJamAmbushOrigin and FJamAmbushAnchor in JamTypes.h; AJamTrackSegment::GetAmbushAnchors with greybox Roof/Cover defaults; AJamTrackManager::CollectAmbushAnchors; AJamDuelManager picks at BeginDuel, flies the enemy in TickIntro (real time), OnAmbush broadcast once, ReleaseDuelRules hides the enemy on every exit path; tunables in UJamDuelTuning; Jam.Ambush and Jam.AmbushDebug (non-shipping); JamGame.Ambush.* automation tests (4).
- **Verified by:** NOT compiled, nothing run. Code review only. Integration build pending.
- **What went wrong:** first scripted edit failed on a shell heredoc error (no file changed); re-run clean.
- **Corrected by:** self.
- **Human decision needed or taken:** none
- **Not done / not verified:** compile, automation tests, PIE flight look, Android. Door anchors have no greybox source (data only). Debug anchor boxes are DrawDebugBox, not meshes.
- **Screenshot:** none
