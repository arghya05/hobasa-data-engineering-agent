# Step DE-04: Identify the sources/pipelines to be changed (Ingestion) ⁵ (attempt 1) for FEAT-ZD-48600

- Workbook trigger: Previous outcome
- Workbook action: Read the existing sources, pipelines and understand it.
Identify the sources or pipelines that needs to be created/updated with minimum lines of code for the ingestion layer
Access for the secrets in all the environments ⁸
- Workbook outcome: The list of sources, pipelines needs to be created/updated with minimum lines of code for the ingestion layer
- Owner of this step: Data engineer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- feat.tasks (Engineering tasks with goal, AC, dependencies and size budget): `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-04/feat.tasks.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md at commit b0fd4d2)
- feat.design (HLD, LLD and contracts): `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-04/feat.design.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md at commit 722a98f)
- org.data_model (Data Model and Knowledge Graph): `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-04/org.data_model.md` (from hobasa-agentic-sdlc/features/FEAT-ZD-48600/org-overrides/org.data_model.md at commit e8afce8)
- task.repo_access (Repository (and Databricks) availability and write access): `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-04/task.repo_access.md` (from hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.repo_access.md at commit daae080)

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.components (Components, layers, sources or pipelines to change): write `hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.components.md`


## How to work
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:pypi (Python package metadata: latest version, licence, release dates (GET /pypi/<package>/json)), mcp:codegraph (Code intelligence over the repository: a symbol's source, its callers and callees, the blast radius of a change and the tests it affects (codegraph_explore, codegraph_node and related tools; pass projectPath = the repository checkout)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

## Tool view (cite as tool:repo)
--- ap_pipeline/src/ap_pipeline/__init__.py ---
"""AP pipeline: ingest the vendor invoice feed, map it to the Data Model, report data quality."""
__version__ = "0.4.0"

--- ap_pipeline/src/ap_pipeline/dq.py ---
"""Data-quality report for one pipeline run."""


def quality_report(records, rejected) -> dict:
    total = len(records) + len(rejected)
    return {"rows": total, "loaded": len(records), "rejected": len(rejected), "reject_reasons": sorted({r["reason"] for r in rejected})}

--- ap_pipeline/src/ap_pipeline/ingest.py ---
"""Ingestion: parse the vendor invoice feed (CSV with header)."""
import csv
import io
from typing import Dict, List


def parse_feed(csv_text: str) -> List[Dict[str, str]]:
    return [dict(r) for r in csv.DictReader(io.StringIO(csv_text))]

--- ap_pipeline/src/ap_pipeline/pipeline.py ---
"""One run: ingest -> transform -> data-quality report."""
from .dq import quality_report
from .ingest import parse_feed
from .transform import to_model


def run(csv_text: str):
    records, rejected = to_model(parse_feed(csv_text))
    return records, quality_report(records, rejected)

--- ap_pipeline/src/ap_pipeline/schedule.json ---
{
  "frequency": "daily",
  "time": "02:00",
  "timezone": "Asia/Kolkata"
}

--- ap_pipeline/src/ap_pipeline/transform.py ---
"""Transformation: map feed rows to the Invoice Data Model (id, vendor, due_date, amount, paid)."""
from datetime import date
from decimal import Decimal, InvalidOperation
from typing import Dict, List, Tuple


def to_model(rows: List[Dict[str, str]]) -> Tuple[List[Dict], List[Dict]]:
    """Return (records, rejected); a row is rejected when a required field is missing or invalid."""
    records, rejected = [], []
    for r in rows:
        try:
            records.append({"id": r["invoice_id"].strip(), "vendor": r["vendor"].strip(), "due_date": date.fromisoformat(r["due_date"]),
                            "amount": Decimal(r["amount"]), "paid": r.get("paid", "false").strip().lower() == "true"})
        except (KeyError, ValueError, InvalidOperation, AttributeError) as e:
            rejected.append({"row": r, "reason": type(e).__name__})
    return records, rejected

--- ap_pipeline/tests/__init__.py ---

--- ap_pipeline/tests/test_pipeline.py ---
import unittest
from decimal import Decimal

from ap_pipeline.pipeline import run

FEED = "invoice_id,vendor,due_date,amount,paid\nINV-1,V-100,2026-09-01,120.00,false\nINV-3,V-100,bad,10.00,false\n"


class PipelineTest(unittest.TestCase):
    def test_run_maps_and_rejects(self):
        records, report = run(FEED)
        self.assertEqual(records[0]["amount"], Decimal("120.00"))
        self.assertEqual(report, {"rows": 2, "loaded": 1, "rejected": 1, "reject_reasons": ["ValueError"]})

    def test_paid_flag(self):
        records, _ = run("invoice_id,vendor,due_date,amount,paid\nI,V,2026-01-01,1,TRUE\n")
        self.assertTrue(records[0]["paid"])

    def test_missing_column_rejected(self):
        records, report = run("invoice_id,vendor,due_date\nI,V,2026-01-01\n")
        self.assertEqual(report["rejected"], 1)


if __name__ == "__main__":
    unittest.main()


Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (anything missing or unclear: BLOCKING: <what you need and who should answer>; never assume), summary.