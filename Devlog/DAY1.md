# Day 1 — 12 November 2026 — "Roll camera": a greybox runner, a duel skeleton, and a lot of honest red ink

> Director's commentary. On this film the director is one human; the crew is AI agents. This log says who did what, what broke, and what nobody has checked yet.

## Goal for today
Theme announced: **DIRECTOR'S CUT**. Pick the concept, write the design, and get a playable greybox of the 3-lane runner on PC and an Android test build.

Concept chosen by the human: *CUT! Zombie Extras* — you are the stunt double in a cheap zombie B-movie whose extras will not stop acting. Outrun the horde; when a zombie jumps you, the director slates a "deleted scene" duel.

## How the crew is organised
| Role | Who | Does | Does not |
|---|---|---|---|
| Director / Team Owner | human | concept, every design decision, asset generation on web tools, device tests, submission | — |
| Orchestrator | Claude (Fable 5.2) in Claude Code | design spec, C++ contracts, task packets, code audit, merges | write gameplay code |
| Executors | Claude Haiku subagents, one git worktree each | implement exactly one task packet, compile, write a report | touch files outside the packet, merge, push |
| Asset lane | separate agent sessions + Blender scripts, Rodin, Meshy | props, crane kit, hero rig prep (see `asset_manifest.csv`) | gameplay logic |

The rule that shaped the day: an executor's report is a claim, not proof. Every branch is read by the orchestrator before it reaches `main`, and "compiles" is never written as "tested".

## What shipped (all merged to `main`)
| Task | What it is | Executor | Audit |
|---|---|---|---|
| 001 | Runner core: lanes, jump, slide, camera, game mode, greybox map created by a headless Python script | Haiku | rework x1, then pass |
| 002 | Touch swipe + tap input | Haiku | pass |
| 003 | Endless pooled track, Low / High / Block obstacles, film reels, 13 patterns + validator | Haiku | pass |
| 004 | Chance / chase system and a greybox horde | Haiku | pass |
| 005 | Title -> run -> game over -> retry, score, saved best, debug HUD | Haiku | pass |
| 007 | Duel state: trigger, intro/outro, duel camera, scene records | Haiku | rework x1, then pass |
| 008 | Duel attacks: three telegraphed attacks, counters, lane prop, slow-motion tutorial | Haiku | pass |
| 010 | Clapperboard slate ("DELETED SCENE n / TAKE t", "CUT! / PRINT IT!") | Haiku | pass (light) |
| 021 | Horde members that vault, duck, crash, fall and get vaulted over (cosmetic) | Haiku | pass |

Design documents written and revised three times as the human added requirements: production spec, C++ architecture contract, 20+ task packets, and a plan for dynamic studio hazards (crane sweeps, tripod, falling crate, road blast), ambush duels and roadside crowds.

Builds: Windows packaged build 613 MB, zip 343 MB. First Android APK of the empty project: 111 MB.

**Not verified yet, stated plainly:** nobody has played any of this. Every task above passed a compile and a line-by-line code audit only. No PIE session and no phone test has been reported. The Android build of the actual game code has not succeeded yet (see below). Tap-to-hit in the duel is still a placeholder ("tap to win") until task 009 lands.

## What broke and how we fixed it
- **The "editor" that was not an editor.** The human saw Unreal running in Task Manager and asked why the agent could not connect to it. It was a headless Android cook the agent itself had suggested running, with 0.6 GB of RAM left. Lesson: check process command lines before guessing; one heavy Unreal job at a time on a 16 GB machine.
- **Live Coding blocked every executor.** The orchestrator opened the editor so the human could test; its Live Coding session made every other build fail with "Unable to build while Live Coding is active". Two executors stopped and reported instead of killing the process (good). Fix: Live Coding disabled in the per-user editor settings.
- **Four parallel compiles = one slow compile.** Four executors building at once stretched a 4-minute build to 27 minutes. New rule: at most two executors, builds one at a time.
- **Windows said yes, Android said no.** The first real Android compile failed on a loop that always exits on its first pass (`-Werror,-Wunreachable-code-loop-increment`). MSVC never complained, so five audits missed it. Fix landed with task 008; every task must now pass the Android compile before merge.
- **A generated security token almost got committed.** A headless editor run wrote an Android file-server block with a token into the config. The audit caught it; the executor's report had wrongly claimed only one section changed.
- **Start key = jump.** The key that started the run also triggered a jump on the same frame, and the code comment claimed the opposite. Caught in audit, fixed in rework.
- **Soft-lock by design order.** The duel state shipped before attacks and hits existed, so a duel at the 30-second mark could never end on a phone. Caught in audit; a clearly marked temporary "tap to win" was added until task 009.
- **The orchestrator's own mistakes.** Miscounted its own pattern table (wrote 14, listed 13); committed a queue file with merge-conflict markers and fixed it one commit later; wrote "the editor is open" before confirming the port was listening.
- **Usage limits.** The 5-hour limit cut three executors off mid-task twice. Work survived because every executor commits to its own branch; they were resumed from where they stopped.

## How AI was used
| Tool / model | Used for | What needed human or orchestrator correction |
|---|---|---|
| Claude (Fable 5.2), Claude Code | design spec, contracts, packets, audits, merges, build orchestration | human overruled or redirected scope several times; three self-inflicted errors listed above |
| Claude Haiku subagents (9 tasks) | all gameplay C++ so far | 2 of 9 tasks needed rework after audit; several reports had small inaccuracies; none could run PIE |
| Unreal Editor headless (Python, UBT, UAT) | map creation, compiles, packaging | Android packaging flag was missing from the pre-jam script |
| Asset lane (Rodin, Meshy, Blender scripts) | environment props, obstacle props, crane kit | tracked in `asset_manifest.csv` and the asset lane's own entries |
Who decided what: the human chose the concept and answered every question the orchestrator put to them (same-lane tap rule, behind-the-player duel camera, endless structure with a "Director's Cut" recap, low-end phone target) and wrote the requirements for animation, hazards, ambushes and crowds. Within those requirements the human explicitly delegated the remaining design decisions to the orchestrator, which made them and flagged each one for veto: for example a single colour language for warnings (red = jump, yellow = slide, orange = change lane), hazards whose phases follow the runner's distance instead of timers, and fallen zombies simulated in formation space so they stay on screen.

## Prompt of the day
Part of the brief every executor received (it is the reason reports can be trusted at all):
```
Compile until it succeeds ... Never claim the build passed unless you saw it succeed; paste the final
lines of the build output into the report. You cannot run PIE. In the report, mark each acceptance
criterion as either "verified by compile/code reading" or "needs human PIE test" - be honest.
... If you are stuck on the same compile error after 4 attempts, stop and report exactly what the
error is rather than guessing further.
```
What we changed during the day: added "do not launch the editor", "do not kill any process", an explicit file-ownership list per task (after two tasks collided in the same file), and a list of constructs Android's compiler rejects.

## Screenshots / clips
- None yet. Day 2 starts with the first human play session; screenshots go in `screenshots/day2_*.png`.

## Cut from scope (and why)
- Nothing cut yet. Deferred on purpose: real road pits, physics destruction, extra hazard variants (each is one data row once the shared system exists), fully random duel timing.
- Cut order agreed if time runs out: power-up -> Brute zombie -> roadside crowds -> road blast -> falling crate -> horde obstacle reactions -> enemy dodge -> crowd variety -> mobbed animation -> Retake.

## Plan for tomorrow
1. Human plays the greybox in the editor and on the phone; fix what the first real test finds.
2. Finish the duel (tap hit, whiff, KO) and get the first Android APK of the real game.
3. Dynamic hazard core with headless automation tests, then crane and tripod.
4. Move content values into saved Data Assets and Blueprints (the "Godot-like" workflow the human asked for) before art is wired in.

## Asset lane supplement — coordinator backfill, late 12 November

OpenAI Codex planned and audited the asset work, with gpt-6-luna assigned only easy dust/mask sources and a simple crate, and gpt-6.1-terra assigned mechanical props, Unreal VFX authoring and the hero rig/animation benchmark. Built-in OpenAI image generation produced gameplay concept boards; the tool did not report its underlying image model. Those boards illustrate the proposal, not a playable build. Meshy produced model/rig/motion outputs; historical ENV facades used Rodin before the owner changed the policy to Meshy only. Blender was retained as a fallback for geometry/pivot/texture/root-motion corrections and inspection.

The human rejected the first empty-looking preview and the cable obstacle, then introduced low/high crane sweeps, hidden rooftop/behind-prop ambushes and zombies breaking through roadside fences before becoming cheering/dancing duel spectators. The coordinator's earlier20 extra PBR material prompts were outside the manual and were rejected. The real missing environment batch was five models across ENV-01–04; offline sheets and measurements were reviewed, including repair of a hallucinated diner rear with a small plywood backing.

Parallel production then produced a963-triangle/512-map crate, coherent16-frame dust sources, a Meshy-rigged10050-triangle hero (24 deform bones, max4 weights) and21 approximate animation mappings. Several motions needed correction: the first slide was a lunge, root translation pushed clips out of frame, and timing/contact audits remain incomplete. A single generated crane could not be reliably cut into articulated parts; separate Meshy base/boom/camera models led to a3638-triangle candidate. That is an explicit temporary authoring exception, not a passed3k mobile LOD. JSON/animation source handoffs are not saved Unreal Blueprint integration.

The VFX worker saved actual dust texture/material/system and an early blast pilot in Unreal. A coordinator editor inspection found a SubUV stack warning even though the compile summary looked clean; the worker reported fixing the manual4x4 setup. Fire/floor cues, motion proof, full integration, cook/device and production-ready acceptance are still open. Worker turns were interrupted by an agent model-usage limit; that is separate from Meshy credits. A Meshy API key was configured only in environment storage, with free API balance validation and7 no-network tests; no key values are included in this record.

Evidence (labels matter): [AI concept board](screenshots/day1_dynamic_studio_concept.png), [offline ENV sheet](screenshots/day1_env_batch_final.png), [crate source audit](screenshots/day1_crate_source_audit.png), [dust source progression](screenshots/day1_dust_source_sheet.png), [hero pose candidates](screenshots/day1_hero_jump_candidates.jpg), [crane low candidate](screenshots/day1_crane_low_candidate.png), [crane high candidate](screenshots/day1_crane_high_candidate.png), [actual Niagara editor audit](screenshots/day1_niagara_editor_audit.png). None is Android gameplay proof. Detailed retrospective entries are under Devlog/entries/20261112-2349_assets_*.md. Every further edit/generation now requires a contemporaneous operation ledger, including failures and validation stage.
