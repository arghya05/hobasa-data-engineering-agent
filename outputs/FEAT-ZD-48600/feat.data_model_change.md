# feat.data_model_change: FEAT-ZD-48600

Data Model change for the Data Engineering lane of FEAT-ZD-48600 ("Show which overdue invoices belong to vendors that usually pay late"). This is DE-05, Data Model Changes, attempt 2. The workbook action is "Update the data model based on the source systems and ingested data". There is no gate: automated checks, the independent judge and sampled human evaluation review this output (record:brief).

| Field | Value | Source |
|---|---|---|
| Trace ID | FEAT-ZD-48600 | record:brief |
| Step | DE-05 Data Model Changes, attempt 2 | record:brief |
| Inputs | task.components (a126bc4, from DE-04), org.data_model (a6b3e2a) | record:brief |
| Background read | journey:feat.design (DES-02 output, approved by the Architect and UX lead on the journey board: DC-1, DC-2, B1.2 to B1.4, DD-6, DD-7) | journey:feat.design; record:memory (JOURNEY.md) |
| Code read | ap_pipeline source and tests, as given in the step's tool view | tool:repo |
| Source system | Vendor invoice feed (CSV with a header row), v2 today, v3 adds the optional `paid_date` column | task.components (IN-1); org.data_model |
| **Result** | **The registered Invoice entity already holds `paid_date` (date or None); this step confirms it field by field against the feed and the code, and adds two items the registered model does not yet hold: the reject reason value `paid_date` on the Rejected row, and the derived Delay history structure produced by `delays_by_vendor`.** No field is removed or retyped; no storage or schema migration is involved | org.data_model; task.components (section 5, TC-1, TC-2, IN-6); journey:feat.design (A1, B1.3) |

## 1. Baseline: registered model and what the code holds today

| Element | Registered model (org.data_model) | Code today (`transform.to_model`) | Gap | Source |
|---|---|---|---|---|
| Invoice `id` | str | `r["invoice_id"].strip()` | None | org.data_model; tool:repo (`transform.py`) |
| Invoice `vendor` | str | `r["vendor"].strip()` | None | org.data_model; tool:repo (`transform.py`) |
| Invoice `due_date` | date | `date.fromisoformat(r["due_date"])` | None | org.data_model; tool:repo (`transform.py`) |
| Invoice `amount` | Decimal, 2 dp | `Decimal(r["amount"])` | None | org.data_model; tool:repo (`transform.py`); record:memory (D-2026-044) |
| Invoice `paid` | bool | `r.get("paid", "false").strip().lower() == "true"` | None | org.data_model; tool:repo (`transform.py`) |
| Invoice `paid_date` | date or None, "added in feed v3" | **Not produced**; the module docstring lists only `id, vendor, due_date, amount, paid` | Registered, not yet implemented; implemented by TC-1 (TSK-01) in a later Data Engineering coding step | org.data_model; tool:repo (`transform.py`); task.components (TC-1, section 3) |
| Rejected row | Not described | `{"row": r, "reason": type(e).__name__}` | `reason` gains the value `paid_date` (section 2.3) | tool:repo (`transform.py`); task.components (TC-1) |
| Delay history | Not described | Does not exist | New derived structure (section 2.4) | task.components (TC-2); journey:feat.design (DC-2, B1.3) |

Package version today: `0.4.0` (tool:repo, `__init__.py`).

## 2. Data model changes

### 2.1 Source contract: vendor invoice feed row (v2 → v3)

| Column | v2 | v3 | Type in the feed | Rule | Source |
|---|---|---|---|---|---|
| `invoice_id`, `vendor`, `due_date`, `amount`, `paid` | Present | Present, unchanged | str (CSV text) | Unchanged | task.components (section 1, IN-1); tool:repo (`tests/test_pipeline.py`) |
| `paid_date` | Absent | **Added, optional** | ISO date string or blank | Missing key (v2) or `None` (short row) or blank → no date; ingestion needs no code change because `csv.DictReader` keys rows by header | task.components (IN-1, IN-2); journey:feat.design (B1.3 "Feed row") |

### 2.2 Entity Invoice (DC-1): no new field beyond the registered model

| Field | Type | Nullable | Derivation from the feed row | Change | Source |
|---|---|---|---|---|---|
| `id` | str | No | `invoice_id`, stripped | None | org.data_model; tool:repo |
| `vendor` | str | No | `vendor`, stripped | None | org.data_model; tool:repo |
| `due_date` | date | No | ISO date (no `strip()` added) | None | org.data_model; task.components (rules carried for coding) |
| `amount` | Decimal, 2 dp | No | `Decimal(...)`; never float | None | org.data_model; record:memory (D-2026-044, L-011) |
| `paid` | bool | No | `paid` text equals `true`, case-insensitive; missing → false | None | org.data_model; tool:repo |
| `paid_date` | date | **Yes (None)** | `None` when missing, `None` or blank; otherwise the ISO date after `strip()`; a non-blank invalid value rejects the row | **Implemented with TC-1** (already registered) | org.data_model; journey:feat.design (B1.2 `_parse_paid_date`, B1.4); task.components (TC-1) |

Rules for the record (from journey:feat.design; DD-6 and DD-7 are still marked pending there):

- The record stays a plain `dict`, not a class; every existing key keeps its name and type; `paid_date` is present on **every** accepted record, including feed v2 rows (value `None`) (journey:feat.design DC-1; task.components TC-1).
- `paid_date` is validated only after the existing field checks pass, so an existing reject reason wins over `paid_date` (journey:feat.design DD-6).
- An unpaid record may carry a valid `paid_date`; it is kept (interim, pending OQ-B6) (journey:feat.design DD-7).
- A paid record may have `paid_date = None`; it is kept and is not rejected (journey:feat.design B1.4).
- No rule requires `paid_date` to be on or after `due_date`; an earlier date gives a negative delay that is kept (journey:feat.design DC-2).

### 2.3 Rejected row: new reason value

| Field | Type | Change | Source |
|---|---|---|---|
| `row` | dict (the feed row) | None | tool:repo (`transform.py`); journey:feat.design (B1.3) |
| `reason` | str | **New value `paid_date`** (constant `PAID_DATE_REASON`) beside today's exception type names (`KeyError`, `ValueError`, `InvalidOperation`, `AttributeError`) | task.components (TC-1); tool:repo (`transform.py`) |

The data-quality report needs no change: `reject_reasons` is the sorted set of every rejected row's `reason`, so `paid_date` appears in it on its own (task.components IN-4; tool:repo `dq.py`).

### 2.4 New derived structure: Delay history (DC-2)

| Attribute | Definition | Source |
|---|---|---|
| Producer | `delays.delays_by_vendor(records)`, a new pure module with no I/O | task.components (TC-2); journey:feat.design (B1.2) |
| Shape | `Dict[str, List[int]]`: key is the Invoice `vendor`, value is that vendor's list of `delay_days` | journey:feat.design (DC-2, B1.3) |
| `delay_days` | `(paid_date - due_date).days`, calendar days, int, may be negative | journey:feat.design (B1.2, NF-2, DC-2); record:memory (D-2026-031) |
| Membership | Only records where `paid` is truthy **and** `paid_date` is not `None`; a vendor with no such record has no key | journey:feat.design (B1.2, B1.5) |
| Order | Values in feed order; vendors in first-seen order | journey:feat.design (B1.2) |
| Inputs not modified | The record list is not changed | journey:feat.design (B1.2) |
| Persistence | None; in memory only. No storage, database or schema migration | task.components (IN-6); journey:feat.design (A1) |
| Consumer | ap_forecast: one vendor's list is the `history` argument of `vendor_risk` | journey:feat.design (DC-2) |

### 2.5 Knowledge-graph relations added for this feature (ap_pipeline side)

| From | Relation | To | Via | Source |
|---|---|---|---|---|
| Feed row (v3) | maps to | Invoice | `transform.to_model` | task.components (TC-1) |
| Feed row (v3) | rejected as | Rejected row (`reason = paid_date`) | `transform.to_model` | task.components (TC-1) |
| Invoice | aggregated by `vendor` into | Delay history | `delays.delays_by_vendor` | task.components (TC-2); journey:feat.design (DC-2) |
| Delay history | consumed by | ap_forecast `vendor_risk` | DC-2 | journey:feat.design (A2, DC-2) |
| Invoice | consumed by | ap_aging `overdue_with_risk` (reads `id`, `vendor`, `due_date`, `amount`, `paid`; ignores `paid_date`) | DC-1 | journey:feat.design (DC-1, B3.3) |

## 3. Compatibility

| Check | Result | Source |
|---|---|---|
| Feed v2 (no `paid_date` column) still loads | Yes: `paid_date = None` on every record | task.components (IN-1); journey:feat.design (NF-4) |
| Existing keys, types and return shapes | Unchanged; `to_model` signature and `(records, rejected)` shape kept | task.components (rules carried for coding); journey:feat.design (NF-4) |
| Downstream consumer of Invoice (ap_aging) | Unaffected; the extra `paid_date` key is ignored | journey:feat.design (B3.3) |
| Monetary type | `amount` stays Decimal | record:memory (D-2026-044, L-011) |

## 4. Schema validation rules (for the later unit-test step)

| Rule | Expected | Source |
|---|---|---|
| Every accepted record has exactly the keys `id`, `vendor`, `due_date`, `amount`, `paid`, `paid_date` | Pass | org.data_model; journey:feat.design (DC-1) |
| `paid_date` is a `date` or `None` | Pass | org.data_model |
| Blank or missing `paid_date` | `None`, row accepted | journey:feat.design (B1.4) |
| Non-blank invalid `paid_date` (for example `"2026-13-01"`, `"not-a-date"`) on an otherwise valid row | Rejected, reason `paid_date`, listed in `reject_reasons` | journey:feat.design (B1.4, B1.5) |
| Bad `due_date` and bad `paid_date` together | Reason `ValueError` | journey:feat.design (B1.5, DD-6) |
| Delay history values are `int` and keys are vendors with at least one paid record carrying a `paid_date` | Pass | journey:feat.design (DC-2, B1.5) |
| `amount` is `Decimal`, never `float` | Pass | record:memory (D-2026-044) |

## 5. Code in this step

This step changes the data model only and changes no code. The tool view shows `transform.py` without `paid_date` and no `delays.py` module (tool:repo). The model change is implemented by TC-1 and TC-2 in the later Data Engineering coding step, within the file budgets DE-04 listed (task.components section 3). Agent judgement: the `transform.py` module docstring lists the model fields, so it should gain `paid_date` with TC-1 (tool:repo `transform.py`).

## 6. Open questions (non-blocking)

| ID | Question | Interim answer | Owner (proposed) |
|---|---|---|---|
| DMQ-A | Should the registered org.data_model be extended with the Rejected row `reason` value `paid_date` and the Delay history structure (sections 2.3 and 2.4)? Today it describes only the Invoice entity | Treat this document as the feature-level model change; data modelers decide whether to register them org-wide | Data engineering lead; data modelers |
| DMQ-B | Carried CQ-I1: is the feed v3 column spelled exactly `paid_date`? | Assume `paid_date` as in org.data_model | Data engineering lead / feed owner |
| DMQ-C | Carried OQ-B6 (DD-7): should an unpaid record keep a valid `paid_date`? | Kept; ignored by the Delay history | Product / Architect |
| DMQ-D | Carried DQ-4: should `delays_by_vendor` be re-exported from `ap_pipeline/__init__.py`? | No; imported as `ap_pipeline.delays.delays_by_vendor` | Architect |

## 7. Validation check

| Check | Result | Source |
|---|---|---|
| Data model updated from the source system and ingested data | Pass: feed v3 contract (2.1), Invoice (2.2), Rejected row (2.3), Delay history (2.4), relations (2.5) | task.components; org.data_model |
| Conflict with the registered model or an approved output | None: `paid_date` matches org.data_model and DC-1; additions are new structures, not changes | org.data_model; journey:feat.design; task.components (section 5) |
| Production or source-system data read | None; only inputs, the tool view and synthetic test data | record:brief |
| Blocking questions | None | — |
