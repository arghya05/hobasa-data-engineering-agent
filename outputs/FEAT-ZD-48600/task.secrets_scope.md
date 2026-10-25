# task.secrets_scope: FEAT-ZD-48600

Secrets scope for the Data Engineering lane of FEAT-ZD-48600 ("Show which overdue invoices belong to vendors that usually pay late"). This is DE-02, Access to secrets (attempt 2). The step checks which secrets the lane's tasks need and whether they are available with access permissions, and asks for access where needed. Only secret **names** are recorded here, never values. There is no gate: automated checks, the independent judge and sampled human evaluation review this output.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DE-02 Access to secrets (attempt 2) | record:brief |
| Trigger | Task creation (feat.tasks from DES-04 Task Creation) | record:brief; feat.tasks |
| Inputs | feat.tasks (b0fd4d2); org.permission_table (restricted, passed by reference only, not opened) | record:brief |
| Repository | ap_pipeline, branch `feature/story` | journey:task.repo_access; tool:repo-access |
| Data Engineering tasks in scope | TSK-01 (STORY-01, optional `paid_date`), TSK-02 (STORY-02, `delays_by_vendor`) | feat.tasks (section 1) |
| Result | **Ready. No secrets are required, none are granted, and no access request is needed.** | tool:repo-access; feat.tasks |

## 1. Secrets needed per task

| Task | What the task does | External system or credential touched | Secrets needed | Source |
|---|---|---|---|---|
| TSK-01 | `to_model` in `src/ap_pipeline/transform.py` reads an optional `paid_date` and rejects invalid values with reason `'paid_date'` | None. `ingest.py`, `pipeline.py` and `schedule.json` are on the "Must not change" list; coding builds on synthetic v3 rows | None | feat.tasks (TSK-01) |
| TSK-02 | New pure function `delays_by_vendor` in new `src/ap_pipeline/delays.py` | None. "No I/O; do not change `records`" | None | feat.tasks (TSK-02) |

Both tasks use only the Python 3.9 standard library and choose no new technology (feat.tasks, introduction: feat.design DD-1). Databricks is not applicable to ap_pipeline, and DE-01 found no workspace credential to check (journey:task.repo_access, P-1 and section 2).

## 2. Secrets scope by environment (names only)

| Environment | Secret names granted | Secret names required | Gap | Source |
|---|---|---|---|---|
| Local development and CI (`/usr/bin/python3 ci.py` on `feature/story`) | None | None | None | tool:repo-access; journey:task.repo_access (P-5, P-7) |
| Integration (`dev`) and released (`main`) branches | None | None | None | tool:repo-access; feat.tasks (section 2) |

The permission table states that no secrets are required or granted for ap_pipeline (tool:repo-access, which reports the permission table). This output does not open or quote org.permission_table, because it is restricted and passed by reference only (record:brief).

## 3. Access requests

| # | Secret name | Environment | Requested from | Status |
|---|---|---|---|---|
| — | None | — | — | No request raised: no task in scope needs a secret (sections 1 and 2) |

## 4. Observations (non-blocking)

| ID | Observation | Interim handling | Source |
|---|---|---|---|
| SS-1 | Feed v3 in production (PQ-6) gates the release of TSK-01, not its coding. Coding builds on synthetic v3 rows, so it needs no feed credential. | No secret for coding. If the release needs a feed credential, DE-02 runs again on that re-entry trigger. | feat.tasks (TSK-01 "Depends on"); record:brief (trigger: "Task creation, or any re-entry trigger") |
| SS-2 | No input names a secret for any later step of this lane. | None granted now. If a later step needs a secret, DE-02 runs again on that re-entry trigger. | tool:repo-access; record:brief (trigger: "Task creation, or any re-entry trigger") |

## 5. Validation check

| Check | Result |
|---|---|
| Every Data Engineering task checked for secret needs | Pass (TSK-01, TSK-02; section 1) |
| Secrets required vs. granted, per environment | Pass: none required, none granted, no gap (section 2) |
| Secret values in this output | None. Names only, and there are no names to list |
| Restricted input handled by reference | Pass: org.permission_table not opened (section 2) |
| Data read by this step | feat.tasks, journey:task.repo_access and the tool view only |
| Blocking questions | None |


[Tool result] [ap_pipeline] Tool registry: no secrets required or granted (permission table)


[Tool result] [ap_pipeline] Tool registry: no secrets required or granted (permission table)