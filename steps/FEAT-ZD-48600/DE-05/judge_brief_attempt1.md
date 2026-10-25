# Evaluate step DE-05: Data Model Changes for FEAT-ZD-48600

- Workbook action: Update the data model based on the source systems and ingested data
- Workbook outcome: Updated data model

## Outputs to evaluate
- `hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/feat.data_model_change.md`

## Inputs the agent was given
- task.components: `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-05/task.components.md`
- org.data_model: `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-05/org.data_model.md`

## Other GitHub files the agent read (valid sources when cited as file:<repository>/<path>)
- `~/.claude/projects/-Users-arghyamukherjee-Documents-automaiton-final-document-e2e-run-runs-run-20261025-141046-feat-zd-48600-workspace-ap-pipeline/35c90e0a-7fd0-4890-99b6-1520e28a0fda/tool-results/hook-76767c9d-0f83-4c5d-9e01-b5f7c126257f-1-additionalContext.txt`
- `hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/feat.data_model_change.md`
- `hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md`

Shared memory and conventions: `memory/MEMORY.md` and `CLAUDE.md` in the SDLC repository.

## The step brief the agent was given (a valid source, cited as record:brief)

# Step DE-05: Data Model Changes (attempt 1) for FEAT-ZD-48600

- Workbook trigger: Previous outcome
- Workbook action: Update the data model based on the source systems and ingested data
- Workbook outcome: Updated data model
- Owner of this step: Data engineer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- task.components (Components, layers, sources or pipelines to change): `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-05/task.components.md` (from hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.components.md at commit e526c4e)
- org.data_model (Data Model and Knowledge Graph): `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-05/org.data_model.md` (from hobasa-agentic-sdlc/features/FEAT-ZD-48600/org-overrides/org.data_model.md at commit 3b03f15)

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- feat.data_model_change (Data Model change for the feature): write `hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/feat.data_model_change.md`

## Code
Work in repository `ap-pipeline-sample` (Python 3.9 (standard library)). You may change only: src/ap_pipeline/. Shell commands allowed in this step: `/usr/bin/python3 ci.py`, `git status`, `git diff`, `git log`. Run the CI yourself with `/usr/bin/python3 ci.py` from the repository root (or delegate to the ci-runner subagent) until it passes. Do not commit or push; the spine does.

## How to work
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier, ci-runner (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:pypi (Python package metadata: latest version, licence, release dates (GET /pypi/<package>/json)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

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


Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (BLOCKING: only when you cannot finish), summary.