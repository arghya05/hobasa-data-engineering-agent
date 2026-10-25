# task.secrets_scope: FEAT-ZD-48600

Secrets scope for the Data Engineering tasks of FEAT-ZD-48600 (ZD-48600, "Show which overdue invoices belong to vendors that usually pay late"). Step: DE-02 Access to secrets (attempt 1). This file lists secret **names only**. It contains no secret values. The restricted permission table is used by reference only, through the step's tool view.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DE-02 Access to secrets (attempt 1) | record:brief |
| Owner / gate | Data engineer / no gate: automated checks, the independent judge and sampled human evaluation | record:brief |
| Inputs | feat.tasks (from hobasa-design-agent, commit b0fd4d2); org.permission_table (restricted, by reference) | record:brief |
| Repository in scope | ap_pipeline | feat.tasks (section 1, TSK-01 and TSK-02) |

## 1. Data Engineering tasks in scope

| Task | Story | Files in budget | External access needed | Source |
|---|---|---|---|---|
| TSK-01 | STORY-01 | `src/ap_pipeline/transform.py`, `src/ap_pipeline/__init__.py` (version step only), `tests/test_pipeline.py` | None. Coding uses synthetic feed v3 rows. Feed v3 in production (PQ-6) gates release, not coding | feat.tasks (TSK-01) |
| TSK-02 | STORY-02 | New `src/ap_pipeline/delays.py`, new `tests/test_delays.py` | None. This is a pure function with "No I/O" | feat.tasks (TSK-02) |

TSK-03 to TSK-08 belong to the Data Science, Backend, Frontend and Testing lanes, so this agent does not assess them (feat.tasks, section 1).

## 2. Secrets required

**None.** TSK-01 and TSK-02 need no secret in any environment.

Evidence:

1. Both tasks change only Python source and its unit tests. TSK-02 is "No I/O", and TSK-01 builds on synthetic rows. `ingest.py`, `pipeline.py` and `schedule.json` are on TSK-01's "Must not change" list (feat.tasks, TSK-01 and TSK-02).
2. The pipeline entry point `run(csv_text)` receives the feed as text. It calls `parse_feed`, `to_model` and `quality_report` and opens no connection (file:ap_pipeline/src/ap_pipeline/pipeline.py).
3. Ingestion parses CSV text in memory with the standard-library `csv` and `io` modules. It reads no file, URL or credential (file:ap_pipeline/src/ap_pipeline/ingest.py).
4. The transform imports only `datetime`, `decimal` and `typing`. The data-quality report imports nothing. Both work on in-memory rows only and read no environment variable (file:ap_pipeline/src/ap_pipeline/transform.py; file:ap_pipeline/src/ap_pipeline/dq.py).
5. The package file contains only a docstring and `__version__ = "0.4.0"`. TSK-01 will step this version up (file:ap_pipeline/src/ap_pipeline/__init__.py; feat.tasks, section 2).
6. CI runs locally as `/usr/bin/python3 ci.py` (file:ap_pipeline/CLAUDE.md). `ci.py` compiles the code, runs the unit tests, checks coverage against `COVERAGE_MIN = 90.0` and runs static checks. It reads no environment variable or credential and makes no network call (file:ap_pipeline/ci.py).
7. The schedule file contains only `frequency`, `time` and `timezone` (daily, 02:00, Asia/Kolkata) and references no credential (file:ap_pipeline/src/ap_pipeline/schedule.json).
8. A search of the ap_pipeline checkout for `secret`, `token`, `password`, `api_key`, `environ`, `getenv` and `credential` found no matches (file:ap_pipeline, Grep over the repository).
9. The ap_pipeline tool registry states that no secrets are required or granted for this repository, based on the permission table (tool:repo-access).

## 3. Secrets granted (names only)

| Environment | Secret name | Granted | Source |
|---|---|---|---|
| Development (`feature/<story>`, local CI) | none | Not needed | tool:repo-access; file:ap_pipeline/CLAUDE.md |
| Integration (`dev`) | none | Not needed | tool:repo-access; file:ap_pipeline/CLAUDE.md |
| Production (`main`) | none | Not needed | tool:repo-access; file:ap_pipeline/CLAUDE.md |

**Required compared with granted:** nothing is required and nothing is granted. There is **no gap**, so **no access request is raised**. There is also no excess access to flag for revocation.

## 4. Data access boundary

- Coding and tests use synthetic data only (`SYN-INV-*`, "Synthetic Vendor &lt;letter&gt;"). No source-system production data is read (feat.tasks, TSK-01 dependencies and TSK-08 implementation notes).
- Feed v3 arriving in production (PQ-6) gates release only. This agent does not need a production feed credential for it (feat.tasks, TSK-01).

## 5. Re-entry

Run DE-02 again if a later step adds a connection. Examples are a file or network source in `ingest.py`, a scheduling workspace, or a transport layer. For this feature, feat.tasks keeps `ingest.py`, `pipeline.py` and `schedule.json` unchanged. It also says a transport layer would re-trigger DES-01 (feat.tasks, TSK-01 and TSK-06).

## 6. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| SSQ-1 | `schedule.json` defines a daily run. No input says which runtime executes the schedule, or whether that runtime needs its own credential to read the production feed. Does DE-13 scheduling need a secret? | TSK-01 and TSK-02 do not need one because they do not change the schedule. Check again at DE-13 | Data engineering lead |

## 7. Validation check

| Check | Result | Source |
|---|---|---|
| Every DE task assessed for secrets | Pass (TSK-01, TSK-02) | feat.tasks |
| No secret values in this file | Pass (names only; none required) | record:brief |
| Required compared with granted | Pass: none required, none granted, no gap | tool:repo-access |
| No production data used | Pass (synthetic only) | feat.tasks |
| Blocking questions | None | — |


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_pipeline] Tool registry: no secrets required or granted (permission table)
