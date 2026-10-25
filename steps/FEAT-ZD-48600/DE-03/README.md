# DE-03 · Database changes

**Agent:** Data Engineering · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** Data engineer: approve by sim.data_engineer

**What this step does:** Check the required database changes are completed

**Expected outcome:** Required Database changes are completed

**Started by:** Task creation, or any re-entry trigger ¹

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `feat.tasks` | Engineering tasks with goal, AC, dependencies and size budget | Design agent · DES-04 | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-03/feat.tasks.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.tasks.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.pdf) |
| 2 | `feat.design` | HLD, LLD and contracts | Design agent · DES-02 | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-03/feat.design.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.design.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-02_feat-design.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-02_feat-design.pdf) |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `task.db_changes` | Completed database changes | [Markdown](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/outputs/FEAT-ZD-48600/task.db_changes.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-engineering-agent/main/docs/FEAT-ZD-48600/DE-03_task-db_changes.html) · [PDF](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/docs/FEAT-ZD-48600/DE-03_task-db_changes.pdf) |

**Code:** repository [ap-pipeline-sample](https://github.com/arghya05/ap-pipeline-sample) · branch [feature/story](https://github.com/arghya05/ap-pipeline-sample/tree/feature/story) · commit [3fcbe1b](https://github.com/arghya05/ap-pipeline-sample/commit/3fcbe1b)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 0 | 0 | {'correct': 5, 'complete': 5, 'grounded': 5, 'useful': 4} |

Improvements the independent judge suggests (non-blocking):

- The repository-search row cites 'file:ap_pipeline (Grep and Glob over the checkout)'. That is not a file:<repository>/<path> citation. I re-ran the search for the listed terms and the migrations glob, and both came back empty, so the claim is correct. It should still cite the tool view (tool:repo-access) or list the files that were searched, so a reviewer can trace it.
- The statement that DE-05 owns recording paid_date in the Data Model is correct per the data-engineering-steps SKILL.md (DE-05 Data Model Changes, which writes feat.data_model_change). The output should cite that skill file instead of leaving the claim without a source.
- This step's scope is ap_pipeline. The output could say in one line why the other lanes' repositories (ap_aging, ap_forecast, ap_portal) are out of scope for DE-03. feat.design A1 lists storage, database and schema migration as out of scope for the whole feature, which covers this.

## How the agent worked

- **Approach:** I read the two contracted inputs, feat.tasks and feat.design, to find any database or storage change in the Data Engineering lane (TSK-01 and TSK-02). I checked those findings against the tool view. I also read the ap_pipeline code (pipeline.py, transform.py, README.md), listed the checkout's files, and searched it for database libraries, DDL and migrations. Shell commands are not allowed in this step, so I used only Glob, Grep and Read. Every input agrees that no database change is needed, so the output records that no change was required and nothing is pending. No connectors were needed.
- **Tools used:** Skill:data-engineering-steps, Read, Glob, Grep, Write
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
