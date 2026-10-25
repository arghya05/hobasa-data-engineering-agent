# task.secrets_scope: FEAT-ZD-48600 (Data Engineering lane)

Secrets scope for the Data Engineering tasks of FEAT-ZD-48600, from DE-02 Access to secrets. The file lists secret names only. No secret values are read, stored or shown here.

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DE-02 Access to secrets | record:brief |
| Owner / gate | Data engineer / no gate (automated checks, independent judge, sampled human evaluation) | record:brief |
| Inputs | feat.tasks (from hobasa-design-agent at commit b0fd4d2); org.permission_table (restricted, read through the tool view only) | record:brief |
| Repository | ap_pipeline | feat.tasks (section 1) |

## 1. Result

**No secrets are needed, and none are granted.** The Data Engineering tasks run without secret access, so there is nothing to request.

| Check | Result | Source |
|---|---|---|
| Secrets required by the DE tasks | None | feat.tasks (TSK-01, TSK-02); file:ap_pipeline/src/ap_pipeline/ingest.py; file:ap_pipeline/src/ap_pipeline/pipeline.py |
| Secrets granted for ap_pipeline | None | tool:repo-access (org.permission_table) |
| Gap between required and granted | None. Nothing to request | tool:repo-access; feat.tasks |
| Access request raised | No, because nothing is missing | tool:repo-access; feat.tasks |

## 2. Tasks in scope and their secret needs

| Task | Story | What it changes | Outside systems it reaches | Secrets needed | Source |
|---|---|---|---|---|---|
| TSK-01 | STORY-01 | `src/ap_pipeline/transform.py`: optional `paid_date` parsing in `to_model`, with reason `'paid_date'` for invalid values; the version step in `__init__.py` | None. It parses the feed records it receives, and coding builds on synthetic v3 rows | None | feat.tasks (TSK-01) |
| TSK-02 | STORY-02 | New `src/ap_pipeline/delays.py`: `delays_by_vendor`, a pure function with "No I/O" | None | None | feat.tasks (TSK-02) |

feat.tasks section 1 assigns TSK-03 to TSK-08 to the Data Science, Backend, Frontend and Testing Agent lanes, outside ap_pipeline. This file does not cover them.

## 3. Evidence from the code

- `ingest.parse_feed(csv_text: str)` parses CSV text with `csv.DictReader` over `io.StringIO`. It opens no connection and reads no credential (file:ap_pipeline/src/ap_pipeline/ingest.py).
- `pipeline.run(csv_text: str)` chains `parse_feed`, `to_model` and `quality_report` on the text it is passed. It calls no source system (file:ap_pipeline/src/ap_pipeline/pipeline.py).
- A case-insensitive search of the ap_pipeline checkout for `os.environ`, `getenv`, `secret`, `token`, `password`, `api_key`, `credential`, `http`, `urllib` and `socket` found no matches (file:ap_pipeline/src/ap_pipeline/ingest.py; file:ap_pipeline/src/ap_pipeline/pipeline.py; file:ap_pipeline/src/ap_pipeline/schedule.json).
- The repository uses the Python 3.9 standard library, and CI runs with `/usr/bin/python3 ci.py` (file:ap_pipeline/CLAUDE.md).
- The tasks choose no new technology: only the Python 3.9 standard library and Node.js 22 CommonJS (feat.tasks, introduction, citing feat.design DD-1).

## 4. Secrets scope by environment (names only)

| Environment / branch | Secret names granted | Source |
|---|---|---|
| `feature/<story>` (development) | None | tool:repo-access; feat.tasks (section 2, branch and commit) |
| `dev` (integration) | None | tool:repo-access; file:ap_pipeline/CLAUDE.md |
| `main` (released) | None | tool:repo-access; file:ap_pipeline/CLAUDE.md |

No input names deployment environments other than these branches. This file therefore scopes secrets per branch. See open question SQ-DE-2.

## 5. Data guardrail

Feed v3 in production (PQ-6) gates release, not coding. Coding builds on synthetic v3 rows (feat.tasks, TSK-01). No production source-system data is needed, so no source-system credential is needed either.

## 6. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| SQ-DE-1 | The pipeline has a schedule (`daily` at `02:00`, `Asia/Kolkata`, in `schedule.json`). Will scheduling the pipeline or sending its status notifications need a scheduler or notification credential? | Not in scope for TSK-01 or TSK-02, and the permission table grants none. Re-check this step on a re-entry trigger if a later step needs one | Data engineering lead |
| SQ-DE-2 | Are there deployment environments beyond the `main`, `dev` and `feature/<story>` branches that need their own secret scope? | None are named in the inputs, so the scope is by branch | Data engineering lead |


<!-- tool results recorded by the spine -->
## Tool results (recorded by the spine from the real tools)
[Tool result] [ap_pipeline] Tool registry: no secrets required or granted (permission table)
