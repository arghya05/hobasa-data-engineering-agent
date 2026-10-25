# task.secrets_scope: FEAT-ZD-48600

Secrets scope for the Data Engineering lane of FEAT-ZD-48600 ("Show which overdue invoices belong to vendors that usually pay late"). The step is DE-02, Access to secrets. It has no human gate. Automated checks, the independent judge and sampled human evaluation review it. This file lists secret **names only**. It holds no secret values.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DE-02 Access to secrets | record:brief |
| Trigger | Task creation (feat.tasks from DES-04) | record:brief; feat.tasks |
| Owner | Data engineer | record:brief |
| Inputs | feat.tasks (hobasa-design-agent, commit b0fd4d2); org.permission_table (restricted, passed by reference only, read through the tool view) | record:brief |
| Background read | feat.design (DES-02); feat.clarifications (DES-03) | journey:feat.design; journey:feat.clarifications |
| Lane tasks in scope | TSK-01 (STORY-01) and TSK-02 (STORY-02), both in ap_pipeline | feat.tasks (section 1) |

## 1. Result

| Repository | Task | Secrets required | Secrets granted | Access request needed | Source |
|---|---|---|---|---|---|
| ap_pipeline | TSK-01: optional `paid_date` in `transform.to_model` | None | None | No | feat.tasks (TSK-01); tool:repo-access; file:ap_pipeline/src/ap_pipeline/transform.py |
| ap_pipeline | TSK-02: new pure function `delays.delays_by_vendor` (no I/O) | None | None | No | feat.tasks (TSK-02); tool:repo-access |

**Granted secret names:** none.

**Outcome:** this lane's tasks need no secret, and the permission table grants none for ap_pipeline. No access request is raised (tool:repo-access; feat.tasks).

## 2. Evidence

| Check | Finding | Source |
|---|---|---|
| Permission table | "No secrets are required or granted for this repository" | tool:repo-access |
| Code that reads secrets | A case-insensitive search of the ap_pipeline checkout (excluding `.git`) for `environ`, `getenv`, `secret`, `token`, `password`, `api_key`, `credential`, `dbutils`, `databricks`, `http`, `urlopen` and `socket` found no match | file:ap_pipeline/src/ap_pipeline/ingest.py; file:ap_pipeline/src/ap_pipeline/transform.py; file:ap_pipeline/src/ap_pipeline/dq.py; file:ap_pipeline/src/ap_pipeline/pipeline.py; file:ap_pipeline/ci.py |
| Data source | The pipeline takes the feed as text: `run(csv_text)` calls `parse_feed`, then `to_model`, then `quality_report`. It opens no connection to a source system | file:ap_pipeline/src/ap_pipeline/pipeline.py; file:ap_pipeline/src/ap_pipeline/ingest.py |
| Task scope | TSK-01 changes only `transform.py`, `__init__.py` (version step) and `tests/test_pipeline.py`. TSK-02 adds `delays.py` and `tests/test_delays.py`, and its notes say "No I/O". Neither task touches `ingest.py`, `pipeline.py` or `schedule.json` | feat.tasks (TSK-01, TSK-02) |
| Design: purity | NF-10: "Every new function is pure and in-memory: no I/O, no network, no mutation of inputs" | journey:feat.design (A5) |
| Design: storage | DD-1: "No new repository, service, runtime, library or storage"; the Data boundary excludes "Storage, database, schema migration" | journey:feat.design (A1, A7) |
| Design: secrets | SE-4, "Secrets and personal data": "None added"; tests and examples use synthetic data | journey:feat.design (A6) |
| Clarifications: test data | CL-X9: "Synthetic only ... no secrets or personal data" | journey:feat.clarifications (section 2) |
| CI | `ci.py` runs compile, unit tests, a coverage gate and static checks locally. It reads no environment variable or credential | file:ap_pipeline/ci.py |

## 3. Environments

The inputs name no deployment environments for ap_pipeline. feat.tasks names only the branches `main` (released), `dev` (integration) and `feature/<story>`. The permission table grants no secret for this repository, so the scope is "none" for every branch and environment of this lane (feat.tasks (section 2); tool:repo-access).

## 4. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| SSQ-1 | DE-13 schedules the pipeline (`schedule.json`: daily, `02:00`, `Asia/Kolkata`; unchanged by this feature per feat.design B1.1). No input names the scheduler platform, or says whether it needs a credential. Will DE-13 need a secret that this scope does not list? | No secret now. Re-run DE-02 (re-entry trigger) if DE-13 names a scheduler that needs a credential | Data engineering lead |

No blocking questions.


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_pipeline] Tool registry: no secrets required or granted (permission table)
