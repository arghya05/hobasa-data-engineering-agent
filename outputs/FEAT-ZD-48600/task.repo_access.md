# task.repo_access: FEAT-ZD-48600

Coding prerequisites for the Data Engineering lane of FEAT-ZD-48600 ("Show which overdue invoices belong to vendors that usually pay late"). The step is DE-01, Checking the coding prerequisites. It has no human gate. Automated checks, the independent judge and sampled human evaluation review it.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DE-01 Checking the coding prerequisites | record:brief |
| Trigger | Task creation (feat.tasks from DES-04) | record:brief; feat.tasks |
| Owner | Data engineer | record:brief |
| Inputs | feat.tasks (hobasa-design-agent, commit b0fd4d2), org.engineering_stack (hobasa-agentic-sdlc, commit e8afce8) | record:brief |
| Lane tasks in scope | TSK-01 (STORY-01, size M) and TSK-02 (STORY-02, size S), both in ap_pipeline | feat.tasks (section 1) |

## 1. Result

| Prerequisite (workbook action) | Status | Evidence | Source |
|---|---|---|---|
| Databricks access | **Not applicable.** No repository uses Databricks or a database, and ap_pipeline uses no Databricks or other workspace | org.engineering_stack: "No repository uses Databricks or a database"; tool view: "No Databricks or other workspace is used by this repository" | org.engineering_stack; tool:repo-access |
| Repository availability | **Available.** ap_pipeline is checked out locally. It was cloned from `https://github.com/arghya05/ap-pipeline-sample.git` | `.git/config` remote `origin`; `.git/logs/HEAD` clone entry | tool:repo-access; file:ap_pipeline/.git/config; file:ap_pipeline/.git/logs/HEAD |
| Repository creation | **Not needed.** The repository already exists, with branches `main` and `dev` that track `origin`. `dev` was created from `origin/dev` | `.git/config` branch entries; `dev` reflog | file:ap_pipeline/.git/config; file:ap_pipeline/.git/logs/refs/heads/dev; org.engineering_stack |
| Working branch | **Ready.** `feature/story` is checked out at commit `3fcbe1b`. It was created from `dev`, which is also at `3fcbe1b` | `.git/HEAD` = `ref: refs/heads/feature/story`; HEAD log "checkout: moving from dev to feature/story" | tool:repo-access; file:ap_pipeline/.git/HEAD; file:ap_pipeline/.git/logs/HEAD; file:ap_pipeline/.git/logs/refs/heads/feature/story |
| Write access | **Granted.** The agent identity has write access, and the working tree is clean | Tool view | tool:repo-access |
| Push to remote | **Spine only.** The spine commits and pushes after checks and evaluation; agents do not push | Step brief | record:brief |

**Outcome:** the repository is available on `feature/story`, and the agent has write access. Databricks is not part of this lane's stack, so no Databricks access is required (org.engineering_stack; tool:repo-access).

## 2. Repository baseline for TSK-01 and TSK-02

| Item | Observed | Matches feat.tasks / org.engineering_stack | Source |
|---|---|---|---|
| Language | Python 3.9 (standard library) | Yes | file:ap_pipeline/CLAUDE.md; org.engineering_stack; tool:repo-access |
| CI command | `/usr/bin/python3 ci.py` runs compile, unittest, a coverage gate (`COVERAGE_MIN = 90.0`) and static checks. The static checks reject `print`, `eval`, `exec` and bare `except`, and lines longer than 140 characters | Yes (CI bar in feat.tasks section 2) | file:ap_pipeline/ci.py; file:ap_pipeline/README.md; org.engineering_stack; feat.tasks |
| Package version | `__version__ = "0.4.0"` in `src/ap_pipeline/__init__.py` | Yes. feat.tasks gives ap_pipeline `0.4.0` today and assigns the version step to TSK-01 | file:ap_pipeline/src/ap_pipeline/__init__.py; feat.tasks (section 2) |
| TSK-01 target function | `to_model(rows: List[Dict[str, str]]) -> Tuple[List[Dict], List[Dict]]` returns `(records, rejected)`. A single `try` block builds `id`, `vendor`, `due_date`, `amount` and `paid`, and the rejection reason is the exception class name. The function does not handle `paid_date` | Yes. This is the signature and return shape that TSK-01 must keep | file:ap_pipeline/src/ap_pipeline/transform.py; feat.tasks (TSK-01) |
| TSK-01 test file | `tests/test_pipeline.py` exists with `test_run_maps_and_rejects`, `test_paid_flag` and `test_missing_column_rejected`. It calls `ap_pipeline.pipeline.run` | Yes. It is the test file assigned to TSK-01 | file:ap_pipeline/tests/test_pipeline.py; feat.tasks (section 2) |
| TSK-02 files in budget | New `src/ap_pipeline/delays.py` and new `tests/test_delays.py` | Created by TSK-02 as planned | feat.tasks (TSK-02) |
| Must-not-change files | `ingest.py` (`parse_feed`), `dq.py` (`quality_report`), `pipeline.py` (`run`: ingest, then transform, then the data-quality report) and `schedule.json` (daily, `02:00`, `Asia/Kolkata`) | Yes. These files are on the TSK-01 must-not-change list | file:ap_pipeline/src/ap_pipeline/ingest.py; file:ap_pipeline/src/ap_pipeline/dq.py; file:ap_pipeline/src/ap_pipeline/pipeline.py; file:ap_pipeline/src/ap_pipeline/schedule.json; feat.tasks (TSK-01) |

## 3. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| RAQ-1 | feat.tasks and org.engineering_stack call for one feature branch per story (`feature/<story>`). The checkout has a single branch, literally named `feature/story`, for both lane stories (STORY-01 and STORY-02). Should the lane use one branch per story, or keep the single `feature/story` branch? | Work on `feature/story` as provided. Commit messages start with the story ID, so both stories stay traceable | Data engineering lead / spine operator |
| RAQ-2 | `ci.py` imports `coverage`, which is not in the Python standard library. This step allows no shell commands, so it could not confirm that `coverage` is installed for `/usr/bin/python3` | Confirm at the first CI run (DE-08 / DE-09) | Data engineering lead |
| RAQ-3 | `.git/config` sets an upstream only for `main` and `dev`. `feature/story` has none. Will the spine set the upstream when it pushes (DE-10)? | The spine pushes and sets the upstream | Spine operator |

No blocking questions.


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_pipeline] Branch feature/story at 3fcbe1b; write access confirmed
