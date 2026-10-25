# DE-04 Identify the sources/pipelines to be changed (Ingestion) ⁵ (Data Engineering agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Previous outcome  
**Action:** Read the existing sources, pipelines and understand it.
Identify the sources or pipelines that needs to be created/updated with minimum lines of code for the ingestion layer
Access for the secrets in all the environments ⁸  
**Outcome expected:** The list of sources, pipelines needs to be created/updated with minimum lines of code for the ingestion layer

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| feat.tasks | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-04/feat.tasks.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md | b0fd4d2 | DES-04 |
| feat.design | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-04/feat.design.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md | 722a98f | DES-02 |
| org.data_model | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-04/org.data_model.md | hobasa-agentic-sdlc/features/FEAT-ZD-48600/org-overrides/org.data_model.md | a6b3e2a | source |
| task.repo_access | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-04/task.repo_access.md | hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.repo_access.md | c7b4be8 | DE-01 |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| task.components | hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.components.md |

## Code

Repository `ap-pipeline-sample`, branch `feature/story`, commit `3fcbe1b`

```
no file changes in this step
```

## How the agent worked

- Approach: I followed the data-engineering-steps skill for DE-04. I fetched the four contracted inputs (feat.tasks, feat.design, org.data_model, task.repo_access) with context_get to get receipts. I also read the approved DE-02 and DE-03 outputs (task.secrets_scope, task.db_changes) as background. Then I read the existing ap_pipeline source, tests and schedule from the step's tool view (tool:repo) to see how the vendor CSV feed is ingested today. The question was whether feed v3, which adds the optional paid_date column, needs any ingestion code. parse_feed uses csv.DictReader, which is header-driven, so the new column arrives as a key with no code change and feed v2 keeps loading. pipeline.run, dq.quality_report and schedule.json also stay unchanged. So the ingestion-layer change list is: no source or pipeline code to create or update, plus one source-contract change (v2 to v3). For later steps I listed, without deciding them, the two components where the feature's code lands (transform.to_model with _parse_paid_date, and the new delays.py), taken from the TSK-01 and TSK-02 budgets. Secrets for all environments come from the approved DE-02 output: none are required. A verifier subagent checked the draft against the sources. I then fixed what it found: an unsupported 'empty when unpaid' phrase, unsourced statements about DE-07's role, present-tense wording for code that TSK-01 has not yet written, and two inferences that I now mark as agent judgement. Shell commands are not allowed in this step, so I used no Bash.
- Tools used: Skill:data-engineering-steps, Read, Glob, Grep, mcp:hobasa-context (context_list, context_get, memory_search), Agent:verifier, Write, Edit
- Helper subagents: Agent
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- Section 1 leaves out `data/sample_feed.csv`, which is an existing source asset in ap_pipeline. It has the v2 header `invoice_id,vendor,due_date,amount,paid`. Add it to the inventory and say whether it stays at v2 (as v2-compatibility evidence) or gets a v3 sibling for the end-to-end synthetic feed.
- Section 4 lists 'all environments' as local/CI, `dev` and `main`, which are branches. It does not cover the production runtime that will receive feed v3. Add an explicit row or open question for the production feed's delivery credential, tied to PQ-6 and DQ-1. Today this only appears in the SS-1 re-entry note.
- Section 4's statement that 'the repository holds no feed connection, credential or workspace' cites tool:repo and task.repo_access P-1. P-1 covers only Databricks and workspaces. Also cite the permission-table result reported in task.secrets_scope ('no secrets required or granted') so the credential part is directly supported.
- In IN-1, the label 'today's (v2) header' rests on feat.design B1.5, where `test_feed_v2_without_paid_date_column` loads a feed 'with today's header'. Cite B1.5 there, because the attached citations (`ingest.py`, tests) never use the word v2.
- IN-1 introduces a 'source contract' update but does not name where that contract lives, other than org.data_model. Name the artefact to update, or mark it as already covered so DE-05 does not duplicate it.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.

## Human evaluation

| Role | By | Decision | Scores | Comment or reason |
|---|---|---|---|---|
| Data engineer | sim.data_engineer | approve | — | automated checks and the independent judge passed |
