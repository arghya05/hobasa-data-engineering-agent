# task.db_changes: FEAT-ZD-48600

Completed database changes for FEAT-ZD-48600 (ZD-48600, "Show which overdue invoices belong to vendors that usually pay late"). This is DE-03 Database changes (attempt 1).

**Result: this feature needs no database changes, so there is nothing left to do. The step is complete and nothing has to be applied.**

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DE-03 Database changes (attempt 1) | record:brief |
| Workbook action | Check the required database changes are completed | record:brief |
| Owner / gate | Data engineer / no gate: automated checks, independent judge, sampled human evaluation | record:brief |
| Inputs | feat.tasks (b0fd4d2), feat.design (722a98f) | record:brief |
| Data Engineering tasks in scope | TSK-01 (STORY-01, `paid_date` in `to_model`), TSK-02 (STORY-02, `delays_by_vendor`) | feat.tasks (section 1) |
| Repository and runtime | ap_pipeline, Python 3.9 standard library only | feat.tasks (section 1); feat.design (DD-1, B1) |

## 1. What the design says about storage

| Check | Finding | Source |
|---|---|---|
| Storage in scope | The design's data boundary covers one new record key `paid_date`, one new reject reason `'paid_date'`, an in-memory delay history, risk map and overdue rows, and CT-1 JSON. It lists "Storage, database, schema migration" as **out of this design** | feat.design (A1, Boundary "Data") |
| New technology or storage | DD-1: no new repository, service, runtime, library or storage | feat.design (A7, DD-1) |
| Purity of new functions | NF-10: every new function is pure and in-memory, with no I/O, no network and no mutation of inputs | feat.design (A5, NF-10) |
| Platform trigger | Storage or a network call from the libraries would reopen DES-01. The design triggers none of these | feat.design (A8) |
| Invoice record shape | DC-1 is a plain Python `dict`, not a class; `paid_date` is added as a key (a `date` or `None`) | feat.design (A3 DC-1; B1.3) |
| Delay history | DC-2 is an in-memory `Dict[str, List[int]]` | feat.design (A3 DC-2; B1.3) |

## 2. What the Data Engineering tasks change

| Task | Files in budget | Database object touched | Source |
|---|---|---|---|
| TSK-01 | `src/ap_pipeline/transform.py`; `src/ap_pipeline/__init__.py` (version step only); `tests/test_pipeline.py` | None | feat.tasks (TSK-01) |
| TSK-02 | New `src/ap_pipeline/delays.py`; new `tests/test_delays.py` | None | feat.tasks (TSK-02) |

TSK-01's "must not change" list keeps `ingest.py`, `dq.py`, `pipeline.py` and `schedule.json` as they are (feat.tasks TSK-01; feat.design B1.1). No task in feat.tasks lists a DDL file, a migration script or a table.

## 3. What the repository holds (code read)

| Item | Finding | Database access | Source |
|---|---|---|---|
| Repository | ap_pipeline has no database, has no migrations and needs none | None | tool:repo-access |
| `ingest.parse_feed` | Parses CSV text in memory with `csv.DictReader` over `io.StringIO` | None | file:ap_pipeline/src/ap_pipeline/ingest.py |
| `transform.to_model` | Builds plain `dict` records inside a `try` block and rejects rows on `(KeyError, ValueError, InvalidOperation, AttributeError)`. TSK-01 adds the `paid_date` parse after that block | None | file:ap_pipeline/src/ap_pipeline/transform.py; feat.tasks (TSK-01) |
| `dq.quality_report` | Counts rows, loaded and rejected, and lists sorted `reject_reasons`, so the new reason `'paid_date'` appears without any change to this function | None | file:ap_pipeline/src/ap_pipeline/dq.py; feat.design (HC-3) |
| `pipeline.run` | `to_model(parse_feed(csv_text))`, then `quality_report`. Returns values and writes nothing | None | file:ap_pipeline/src/ap_pipeline/pipeline.py |
| `schedule.json` | daily, 02:00, Asia/Kolkata; no change | None | file:ap_pipeline/src/ap_pipeline/schedule.json; feat.design (B1.1) |
| `__init__.py` | `__version__ = "0.4.0"`; version step only (TSK-01) | None | file:ap_pipeline/src/ap_pipeline/__init__.py; feat.tasks (section 2) |

A search of the ap_pipeline checkout for database terms (`sqlite`, `sql`, `database`, `migrat`, `cursor`, `connect(`) found no matches. Feed v2, which has no `paid_date` column, must still load. TSK-01 handles the optional column inside `to_model`, so no schema has to change (feat.tasks TSK-01; feat.design NF-4, B1.4).

## 4. Database change register

| ID | Change | Required | Status | Source |
|---|---|---|---|---|
| — | No database, table, column, index, view or migration change | No | Not applicable: nothing to apply or roll back | feat.design (A1, DD-1, NF-10); feat.tasks (TSK-01, TSK-02); tool:repo-access |

None of these apply: a rollback plan, migration scripts, an environment promotion order or post-change validation queries.

## 5. Effect on later Data Engineering steps

- DE-05 (Data Model Changes): the logical Invoice record gains `paid_date` (a `date` or `None`), and there is a new reject reason `'paid_date'`. These are changes to the registered data model, not to a physical database. They belong to DE-05, not to this step (feat.design B1.3, DC-1).
- DE-08 (Coding): this step adds no database prerequisite to TSK-01 or TSK-02.

## 6. Decisions and memory checked

Decisions D-2026-031, D-2026-044 and D-2026-050 and lessons L-007 and L-011 cover day counts, Decimal amounts and leaving paid invoices out. None of them needs storage, so none conflicts with a no-database outcome (record:memory).

## 7. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| DBQ-1 | Carried from feat.design DQ-1: no input names the production component that calls the stages and carries CT-1. A database change would only be needed if that host later stored records, overdue rows or risk levels | None in this feature. Adding storage would reopen DES-01 (feat.design A8) | Architect |

## 8. Validation check

| Check | Result | Source |
|---|---|---|
| Required database changes identified | None required | feat.design (A1, DD-1); tool:repo-access |
| Required database changes completed | Complete (nothing to apply) | tool:repo-access |
| No production or source-system data used | Pass: only the step's inputs, the repository code and the tool view were used; no source system was queried | record:brief |
| Conflict with an approved output | None | feat.design; feat.tasks |
| Blocking questions | None | — |


[Tool result] [ap_pipeline] No database in this repository's architecture; no migrations required