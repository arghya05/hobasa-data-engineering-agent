# DE-05 Data Model Changes (Data Engineering agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Previous outcome  
**Action:** Update the data model based on the source systems and ingested data  
**Outcome expected:** Updated data model

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| task.components | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-05/task.components.md | hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.components.md | a126bc4 | DE-04 |
| org.data_model | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-05/org.data_model.md | hobasa-agentic-sdlc/features/FEAT-ZD-48600/org-overrides/org.data_model.md | a6b3e2a | source |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| feat.data_model_change | hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/feat.data_model_change.md |

## Code

Repository `ap-pipeline-sample`, branch `feature/story`, commit `3fcbe1b`

```
no file changes in this step
```

## How the agent worked

- Approach: Attempt 2 fixes the single G1 refusal. The output cited a CI run (test count, failure count, coverage) to tool:ci, and this step does not receive a tool:ci view. I removed that sentence and every CI figure from the output and the claims. Section 5 now says only that this step changes no code, citing the tool view (tool:repo), which shows transform.py without paid_date and no delays.py. I re-fetched both contracted inputs with context_get. task.components is now at revision a126bc4 and its content is unchanged. org.data_model is still a6b3e2a. I re-fetched feat.design as background so its receipt is current. I updated the header to attempt 2 and to the new task.components revision, and rewrote feat.data_model_change.md in full. Everything else is as before: I compared the registered Invoice entity field by field with the feed v2 → v3 source contract and with transform.to_model in the tool view. I added the Rejected-row reason value 'paid_date' and the derived in-memory Delay history (DC-2), and I listed the knowledge-graph relations, the compatibility checks and the schema validation rules for unit tests. The step changes the data model only, so I made no code change; the code belongs to the TC-1 and TC-2 budgets.
- Tools used: mcp__hobasa-context__context_get (task.components, org.data_model, feat.design), Write
- Helper subagents: Agent
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | 1 | {'correct': 4, 'complete': 4, 'grounded': 4, 'useful': 4} |
| 2 | resumed | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- The brief says to run `/usr/bin/python3 ci.py` until it passes. The output says no code was changed, which matches the checkout: `src/ap_pipeline/` has no `paid_date` and no `delays.py`. It does not say outright that CI was not run in this step. One cited line saying so (record:brief; tool:repo) would answer the question that led to the attempt-1 refusal.
- Section 2.4 cites record:memory D-2026-031 for `delay_days` being calendar days. D-2026-031 is about aging buckets. The calendar-day rule for `delay_days` comes from feat.design NF-2, so cite that as the main source and give D-2026-031 only as background.
- DMQ-A (whether to register the Rejected-row reason and the Delay history structure org-wide) has no follow-up step. Name the step or person who would update org.data_model, so the gap is not left open for the DE coding step.
- Section 4 lists the schema validation rules but does not map them to the planned test names in feat.design B1.5 (for example `test_invalid_paid_date_rejected`, `test_existing_reason_wins`). Adding the mapping would let the later unit-test step use it directly.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.

## Human evaluation

| Role | By | Decision | Scores | Comment or reason |
|---|---|---|---|---|
| Data engineer | sim.data_engineer | approve | — | automated checks and the independent judge passed |
