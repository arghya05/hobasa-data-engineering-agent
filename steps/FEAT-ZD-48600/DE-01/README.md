# DE-01 · Checking the coding prerequisites ⁹

**Agent:** Data Engineering · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** Data engineer: approve by sim.data_engineer

**What this step does:** Check for access to Databricks, repo availability, repo creation and write access for the same

**Expected outcome:** Databricks and Repo is available with write access

**Started by:** Task creation, or any re-entry trigger ¹

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `feat.tasks` | Engineering tasks with goal, AC, dependencies and size budget | Design agent · DES-04 | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-01/feat.tasks.md) · [original](https://github.com/arghya05/hobasa-design-agent/blob/main/outputs/FEAT-ZD-48600/feat.tasks.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-design-agent/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.html) · [PDF](https://github.com/arghya05/hobasa-design-agent/blob/main/docs/FEAT-ZD-48600/DES-04_feat-tasks.pdf) |
| 2 | `org.engineering_stack` | Repositories, languages, versions and exact commands | organisation source | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-01/org.engineering_stack.md) · [original](https://github.com/arghya05/hobasa-agentic-sdlc/blob/main/org/org.engineering_stack.md) |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `task.repo_access` | Repository (and Databricks) availability and write access | [Markdown](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/outputs/FEAT-ZD-48600/task.repo_access.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-engineering-agent/main/docs/FEAT-ZD-48600/DE-01_task-repo_access.html) · [PDF](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/docs/FEAT-ZD-48600/DE-01_task-repo_access.pdf) |

**Code:** repository [ap-pipeline-sample](https://github.com/arghya05/ap-pipeline-sample) · branch [feature/story](https://github.com/arghya05/ap-pipeline-sample/tree/feature/story) · commit [3fcbe1b](https://github.com/arghya05/ap-pipeline-sample/commit/3fcbe1b)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | 1 | {'correct': 5, 'complete': 4, 'grounded': 4, 'useful': 4} |
| 2 | continued | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

Improvements the independent judge suggests (non-blocking):

- Write access and the clean working tree rest only on tool:repo-access and the spine's tool result. Nothing in the checkout confirms them, because `.git/config` has no upstream for `feature/story`. RA-1 says this, but the outcome line could say directly that remote push access to origin has not been tested yet and will only be tested when the spine first pushes.
- The branch is named literally `feature/story`, while the convention in org.engineering_stack and ap_pipeline/CLAUDE.md is a 'feature branch per story' (`feature/<story>`). Row 24 still marks the branch model as 'Matches the convention'. That conflicts a little with RAQ-1. The row could say 'partially matches; per-story naming is pending RAQ-1'.
- RAQ-2 names DE-08/DE-09 as the steps where `coverage` will be confirmed. That is consistent with the step list in the data-engineering skill (DE-08 Coding, DE-09 Unit Test Cases). Citing that skill file would make the reference traceable.
- The Databricks check is marked not applicable based on org.engineering_stack and the tool view. Saying explicitly that the workbook outcome 'Databricks ... available with write access' is therefore met as N/A, not as a pass, would help the people at the gate.

## How the agent worked

- **Approach:** I followed the DE-01 procedure in the data-engineering-steps skill. First I read both inputs. feat.tasks puts the DE lane tasks TSK-01 and TSK-02 in ap_pipeline, and org.engineering_stack states there is no Databricks or database. Shell commands are not allowed in this step, so I checked the repository by reading its files: the git metadata (HEAD, the refs for main, dev and feature/story, config), CLAUDE.md, README.md, ci.py, the version file, every source module, the existing test file and the synthetic sample feed. I combined these with the tool:repo-access view, which reports availability, write access, the clean tree and the branch. Each repository fact in the output cites the file:ap_pipeline/<path> I read for it; tool:repo-access is cited only for what the tool view states.
- **Tools used:** Skill:data-engineering-steps, Read, Glob, Write, tool:repo-access (step tool view)
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
