# DE-04 · Identify the sources/pipelines to be changed (Ingestion) ⁵

**Agent:** Data Engineering · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** Data engineer: approve by sim.data_engineer

**What this step does:** Read the existing sources, pipelines and understand it.
Identify the sources or pipelines that needs to be created/updated with minimum lines of code for the ingestion layer
Access for the secrets in all the environments ⁸

**Expected outcome:** The list of sources, pipelines needs to be created/updated with minimum lines of code for the ingestion layer

**Started by:** Previous outcome

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `feat.tasks` | Engineering tasks with goal, AC, dependencies and size budget | Design agent · DES-04 | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-04/feat.tasks.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.tasks.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.pdf) |
| 2 | `feat.design` | HLD, LLD and contracts | Design agent · DES-02 | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-04/feat.design.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.design.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-02_feat-design.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-02_feat-design.pdf) |
| 3 | `org.data_model` | Data Model and Knowledge Graph | organisation source | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-04/org.data_model.md) · [original](https://github.com/arghya05/hobasa-agentic-sdlc/blob/main/features/FEAT-ZD-48600/org-overrides/org.data_model.md) |
| 4 | `task.repo_access` | Repository (and Databricks) availability and write access | Data Engineering agent · DE-01 | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-04/task.repo_access.md) · [original](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/outputs/FEAT-ZD-48600/task.repo_access.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-engineering-agent/main/docs/FEAT-ZD-48600/DE-01_task-repo_access.html) · [PDF](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/docs/FEAT-ZD-48600/DE-01_task-repo_access.pdf) |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `task.components` | Components, layers, sources or pipelines to change | [Markdown](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/outputs/FEAT-ZD-48600/task.components.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-engineering-agent/main/docs/FEAT-ZD-48600/DE-04_task-components.html) · [PDF](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/docs/FEAT-ZD-48600/DE-04_task-components.pdf) |

**Code:** repository [ap-pipeline-sample](https://github.com/arghya05/ap-pipeline-sample) · branch [feature/story](https://github.com/arghya05/ap-pipeline-sample/tree/feature/story) · commit [3fcbe1b](https://github.com/arghya05/ap-pipeline-sample/commit/3fcbe1b)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | — | — |
| 2 | continued | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 5} |

Improvements the independent judge suggests (non-blocking):

- Section 5 concludes that the lane needs no secret access in any environment, but the evidence only covers the repository code. feat.design A4/DQ-1 says no input states who calls the stages in production or how the feed CSV is delivered. Production feed delivery may need credentials that the code never touches. State this limit next to the conclusion and tie it to DQ-1, not only to CQ-DE4-3.
- No input names the environments (dev, test, prod, and so on), so 'all environments' is never listed. Raise it as an open question or name the environments explicitly, so the secrets check can be traced per environment.
- DE-02 (task.secrets_scope) is marked done in JOURNEY.md. The output already flags that it was not cross-checked (CQ-DE4-3). It would help to say that DE-08 reads task.secrets_scope, so the reconciliation will happen there.
- Section 2 describes the change as 'Vendor invoice feed v2 → v3'. Note that feed v3 is a release-gating external dependency (feat.design Part C) next to that row, not only in CQ-DE4-1, so readers see that the source change is not yet in production.

## How the agent worked

- **Approach:** I read the four contracted inputs (feat.tasks, feat.design, org.data_model, task.repo_access). I took the ap_pipeline code from the step's tool view, and read data/sample_feed.csv and README.md from the checkout. I searched src/ for secret, token, password and environment-variable patterns to cover the workbook's secrets action. I traced how the new feed v3 column paid_date flows through the existing pipeline. csv.DictReader in ingest.parse_feed passes any header column through, so the smallest ingestion change is none. All changes fall in transform.py (TSK-01), the new delays.py (TSK-02), the version step and the tests, all inside the feat.tasks file budgets. Code facts cite the tool view (tool:repo); only files actually read with Read are cited as file:. I did not use CodeGraph or the API connectors because the tool view held all the code.
- **Tools used:** Skill (data-engineering-steps), Read, Glob, Grep, Write
- **Helper subagents:** none
- **Connector calls:** none

## Human evaluation

| Role | By | Decision | Scores (correct, complete, grounded, useful) | Comment or reason |
|---|---|---|---|---|
| Data engineer | sim.data_engineer | approve | — | automated checks and the independent judge passed |

## Files in this folder (audit trail)

- `brief_attempt<N>.md`: the exact brief the agent received
- `result_attempt<N>.json`: the agent's structured answer (approach, tools, claims with their sources, open questions)
- `checks_attempt<N>.json`: what the automated guardrail checks found
- `judge_attempt<N>.json`, `judge_brief_attempt<N>.md`: the independent judge
- `outputs/`: the outputs as approved
