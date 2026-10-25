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
| 1 | new | 2 | — | — |
| 2 | continued | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

Improvements the independent judge suggests (non-blocking):

- RAQ-1 and RAQ-3 each give an interim answer that is really a default the agent picked: 'Work on feature/story as provided' and 'The spine pushes and sets the upstream'. No source says the spine sets the upstream. hobasa-agentic-sdlc/CLAUDE.md says nothing is assumed. These should be plain questions to the owner with no interim decision, or BLOCKING ones if they matter before DE-08.
- RAQ-1: one shared branch named `feature/story` for both STORY-01 and STORY-02 departs from the 'feature branches per story' rule in org.engineering_stack and feat.tasks section 2. Consider asking the spine operator to confirm this before coding instead of marking it non-blocking.
- RAQ-2 says 'This step allows no shell commands' but cites no source. The spine's hook enforces it (G9), but it is not among the cited sources. Cite the restriction or reword it as an observation.
- The 'Push to remote' row says 'agents do not push', citing record:brief. DE-10 in data-engineering-steps SKILL.md lists 'Push the code to central/main repository' as the lane's action. Say that the push happens at DE-10 and the spine carries it out, so a reader does not see a conflict.
- Row 'Databricks access: Not applicable' rests on org.engineering_stack and the tool view. Say who confirms that the lane has no Databricks dependency in future steps (for example DE-13 scheduling), because the workbook outcome expects Databricks to be available.

## How the agent worked

- **Approach:** I read both contracted inputs (feat.tasks, org.engineering_stack). The step hook blocks shell commands in DE-01, so I checked ap_pipeline by reading its git metadata (.git/HEAD, .git/config, the HEAD, dev and feature/story reflogs) and its repository files with Read. The files were __init__.py, transform.py, ingest.py, dq.py, pipeline.py, schedule.json, ci.py, README.md, CLAUDE.md and tests/test_pipeline.py. I matched them against the tool view and the TSK-01 and TSK-02 budgets in feat.tasks. Every file cited in the output was read in this session. I used no connectors because the inputs and checkout held the needed facts.
- **Tools used:** Skill:data-engineering-steps, Read, Glob, Write
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
