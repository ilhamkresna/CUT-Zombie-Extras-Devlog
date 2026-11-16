# 2026-11-14 04:25 WIB — LUNA — Blender cleanup inventory

- **Who / tool:** LUNA read-only inventory worker, Codex PowerShell.
- **Task:** Owner-requested Blender test-output inventory · **Branch / commit:** main; no commit created.
- **Asked for:** Identify exact project-contained old Blender-generated test renders, frame/video outputs, caches and backups for root review before any deletion.
- **Result:** Inventory in progress. Scope is limited to explicit project paths and Blender-output locations named in project logs.
- **Verified by:** Project-log search and resolved-path/file-size listing pending.
- **What went wrong:** Nothing so far.
- **Corrected by:** Not applicable.
- **Human decision needed or taken:** Root must audit candidate paths and authorize exact deletions before any removal.
- **Not done / not verified:** No file has been deleted, moved, renamed or compressed. Protected source assets, Unreal content and unrelated user files are out of scope.
- **Screenshot:** none.

## Operation ledger
| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 04:25 | Read-only scope preflight | Codex PowerShell, owner Blender cleanup request | Confirmed explicit project root `E:\Project Game\Epic-MegaJam-2026`; no broad user-disk search. | Project-log paths and candidate inventory pending |
| 04:26 | Restricted worktree-cache and Blender-temp path inventory | Codex PowerShell, owner cleanup request | Root reported MAIN 2.62 GiB raw_assets and 6.52 GiB Sandbox, mostly Unreal caches; scope narrowed to immediate `.claude/worktrees` and project-log-evidenced Blender temp dirs. | Worktree cache sizes and exact in-project Blender temp paths pending |

## Read-only inventory result for manual review

The task-026 records say the selected crane candidate is `raw_assets/PROP-CRANE/components_v4/v4d/crane_v4.blend/.glb/.fbx`; earlier whole-mesh/procedural, cut-base v3 and damaged-lug v4 experiments were rejected. The following eight named frame folders contain 120 PNGs each. Their names and iteration history identify them as superseded render candidates for owner review. They total 960 PNGs / 446,595,368 bytes (about 426 MiB). The current v4d render folder is kept as evidence. Candidate folders are listed only for manual review; no deletion authorization was inferred.

| Manual-review candidate folder | PNG count | Bytes |
|---|---:|---:|
| `E:\Project Game\Epic-MegaJam-2026\raw_assets\PROP-CRANE\frames` | 120 | 73,550,197 |
| `E:\Project Game\Epic-MegaJam-2026\raw_assets\PROP-CRANE\frames_candidate` | 120 | 55,368,908 |
| `E:\Project Game\Epic-MegaJam-2026\raw_assets\PROP-CRANE\frames_candidate_v2` | 120 | 55,752,762 |
| `E:\Project Game\Epic-MegaJam-2026\raw_assets\PROP-CRANE\frames_candidate_v3` | 120 | 56,576,374 |
| `E:\Project Game\Epic-MegaJam-2026\raw_assets\PROP-CRANE\frames_repaired` | 120 | 73,550,139 |
| `E:\Project Game\Epic-MegaJam-2026\raw_assets\PROP-CRANE\components_v4\frames` | 120 | 44,011,093 |
| `E:\Project Game\Epic-MegaJam-2026\raw_assets\PROP-CRANE\components_v4\v4b\frames` | 120 | 43,900,320 |
| `E:\Project Game\Epic-MegaJam-2026\raw_assets\PROP-CRANE\components_v4\v4c\frames` | 120 | 43,885,575 |
| Preserve current candidate evidence: `E:\Project Game\Epic-MegaJam-2026\raw_assets\PROP-CRANE\components_v4\v4d\frames` | 120 | 43,839,349 |

`Sandbox/ENV_QC_backing` holds three accepted final-repair view images totaling 795,033 bytes; the audit cites these views, so keep them. No files in other asset sources were proposed for removal.

A targeted size inventory of immediate named cache folders (`Build`, `Builds`, `Binaries`, `DerivedDataCache`, `Intermediate`, `Saved`) under MAIN `.claude/worktrees` found 107 folders, 26,690 files and a logical size sum of 126,594,383,876 bytes (about 117.9 GiB; this sum may include hardlink duplicates). These are Unreal/worktree build caches, not Blender leftovers. Do not remove worktrees or these cache folders as part of this Blender cleanup. Largest measured folder: `E:\Project Game\Epic-MegaJam-2026\.claude\worktrees\agent-a0b726b9ed8158811\Intermediate` at 7,895,013,848 bytes; next `agent-a4c61c4c5e2b8e0bf\Intermediate` at 5,505,443,279 bytes.

No deletion, move, rename or compression was performed. Main `raw_assets`, source models/rigs/texture bakes, `.blend` sources, provider downloads and Unreal Content remain protected. Root independently reported MAIN raw_assets at 2.62 GiB and Sandbox at 6.52 GiB, mostly Unreal PipeTest caches; those broader totals were not remeasured in this pass.

| Time WIB | Edit / generation / audit | Prompt/model/tool and safe task ID | Outputs / actual result | Verified stage / pending |
|---|---|---|---|---|
| 04:29 | Read-only targeted render-folder and worktree-cache sizes; manual handoff | Codex PowerShell, owner cleanup request | Measured eight superseded crane frame folders (960 PNGs, 446,595,368 bytes), current v4d render evidence (120 PNGs, 43,839,349 bytes), accepted ENV repair views (3 files, 795,033 bytes), and 107 named worktree cache folders (126,594,383,876 logical bytes). | Paths resolved inside MAIN. Blender frame folders are owner-review candidates only; v4d evidence, accepted views, source blends, Unreal caches and all other user files were not deleted. No deletion authorized/executed. |

**Inventory outcome:** Potential old-crane-frame recovery is 446,595,368 bytes (about 426 MiB), below the reported tens-of-GB storage target. Worktree cache total is logical size, not verified physical reclaimable size. Root/owner manual selection remains pending.
