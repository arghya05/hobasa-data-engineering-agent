# task.secrets_scope: FEAT-ZD-48600

Secrets scope for the Data Engineering tasks of FEAT-ZD-48600 (ZD-48600, "Show which overdue invoices belong to vendors that usually pay late"). Step: DE-02 Access to secrets. This file lists secret **names only**. No secret value appears here, and the restricted permission table is used by reference only.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DE-02 Access to secrets | record:brief |
| Owner / gate | Data engineer / no gate: automated checks, independent judge, sampled human evaluation | record:brief |
| Inputs | feat.tasks (from hobasa-design-agent, commit b0fd4d2); org.permission_table (restricted, by reference) | record:brief |
| Repository in scope | ap_pipeline | feat.tasks (section 1, TSK-01 and TSK-02) |

## 1. Data Engineering tasks in scope

| Task | Story | What it touches | External access it needs | Source |
|---|---|---|---|---|
| TSK-01 | STORY-01 | `src/ap_pipeline/transform.py`, `src/ap_pipeline/__init__.py` (version step), `tests/test_pipeline.py` | None. Built on synthetic feed v3 rows. Feed v3 in production (PQ-6) gates release, not coding | feat.tasks (TSK-01) |
| TSK-02 | STORY-02 | New `src/ap_pipeline/delays.py`, new `tests/test_delays.py` | None. Pure function, "No I/O" | feat.tasks (TSK-02) |

The other tasks (TSK-03 to TSK-08) belong to the Data Science, Backend, Frontend and Testing lanes. They are out of scope for this agent (feat.tasks, section 1).

## 2. Secrets required

**None.** TSK-01 and TSK-02 need no secret in any environment.

Evidence:

1. Both tasks change only Python code and its unit tests. TSK-02 is explicitly "No I/O", and TSK-01 builds on synthetic rows (feat.tasks, TSK-01 and TSK-02).
2. The pipeline entry point takes the feed as text. `run(csv_text)` calls `parse_feed`, then `to_model`, then `quality_report`, and opens no connection (file:ap_pipeline/src/ap_pipeline/pipeline.py).
3. Ingestion uses the standard library `csv` module to parse CSV text held in memory. It reads no file, URL or credential (file:ap_pipeline/src/ap_pipeline/ingest.py).
4. The transform and data-quality modules work on in-memory rows only. They import only `datetime`, `decimal` and `typing` and read no environment variable (file:ap_pipeline/src/ap_pipeline/transform.py; file:ap_pipeline/src/ap_pipeline/dq.py).
5. The package file holds only the docstring and `__version__ = "0.4.0"`, which TSK-01 steps up (file:ap_pipeline/src/ap_pipeline/__init__.py; feat.tasks, section 2).
6. CI runs locally as `/usr/bin/python3 ci.py` (file:ap_pipeline/CLAUDE.md). `ci.py` compiles the code, runs the unit tests, measures coverage against a minimum of 90.0 and runs static checks. It reads no environment variable or credential and makes no network call (file:ap_pipeline/ci.py).
7. The schedule file contains only `frequency`, `time` and `timezone` (daily, 02:00, Asia/Kolkata), with no credential reference (file:ap_pipeline/src/ap_pipeline/schedule.json).
8. The ap_pipeline tool registry says no secrets are required or granted for this repository (tool:repo-access, from the permission table).

## 3. Secrets granted (names only)

| Environment | Secret name | Granted | Source |
|---|---|---|---|
| Development (feature branch, local CI) | none | Not needed | tool:repo-access |
| Integration (`dev`) | none | Not needed | tool:repo-access |
| Production (`main`) | none | Not needed | tool:repo-access |

Required versus granted: nothing is required and nothing is granted. There is **no gap**, so **no access request** is raised. There is also no excess access to flag for revocation. The branch names come from file:ap_pipeline/CLAUDE.md.

## 4. Data access boundary

- Coding and tests use synthetic data only (`SYN-INV-*`, "Synthetic Vendor &lt;letter&gt;"). This task reads no source-system production data (feat.tasks, TSK-01 dependencies and TSK-08 implementation notes).
- Feed v3 arriving in production (PQ-6) gates release only. It does not require this agent to hold a production feed credential (feat.tasks, TSK-01).

## 5. Re-entry

Re-check this scope (re-run DE-02) if a later step adds a connection, for example a file or network source in `ingest.py`, a Databricks workspace for scheduling, or a transport layer. feat.tasks lists `ingest.py`, `pipeline.py` and `schedule.json` as "Must not change" for TSK-01, and says a transport layer would re-trigger DES-01 (feat.tasks, TSK-01 and TSK-06).

## 6. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| SSQ-1 | `schedule.json` defines a daily run. The inputs do not say which runtime executes the schedule, or whether that runtime needs its own credential for the production feed. Does DE-13 scheduling need a secret? | Not needed for TSK-01 or TSK-02, which do not change the schedule. Re-check at DE-13 | Data engineering lead |

## 7. Validation check

| Check | Result | Source |
|---|---|---|
| Every DE task assessed for secrets | Pass (TSK-01, TSK-02) | feat.tasks |
| Secret values absent from this file | Pass (names only; none required) | record:brief |
| Required vs granted compared | Pass: none required, none granted, no gap | tool:repo-access |
| Production data not used | Pass (synthetic only) | feat.tasks |
| Blocking questions | None | none |


[Tool result] [ap_pipeline] Tool registry: no secrets required or granted (permission table)