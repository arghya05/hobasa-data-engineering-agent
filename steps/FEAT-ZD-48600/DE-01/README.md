# DE-01 Checking the coding prerequisites ⁹ (Data Engineering agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Task creation, or any re-entry trigger ¹  
**Action:** Check for access to Databricks, repo availability, repo creation and write access for the same  
**Outcome expected:** Databricks and Repo is available with write access

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| feat.tasks | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-01/feat.tasks.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md | b0fd4d2 | DES-04 |
| org.engineering_stack | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-01/org.engineering_stack.md | hobasa-agentic-sdlc/org/org.engineering_stack.md | 3b03f15 | source |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| task.repo_access | hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.repo_access.md |

## Code

Repository `ap-pipeline-sample`, branch `feature/story`, commit `3fcbe1b`

```
no file changes in this step
```

## How the agent worked

- Approach: I read the two contracted inputs (feat.tasks, org.engineering_stack) and the DE-01 procedure in the data-engineering-steps skill. From feat.tasks I found that the Data Engineering tasks (TSK-01, TSK-02) touch only ap_pipeline. I took availability, branch and write access from the spine's repo-access tool view. Shell commands are blocked in DE-01, so I confirmed the branch state by reading the ap_pipeline .git HEAD and branch refs instead of running git. I confirmed Databricks is not used from org.engineering_stack and from a code search across ap_pipeline for Databricks, Spark, JDBC and database terms, which found nothing. For conventions and notes for later steps I read the repository memory (ap_pipeline/CLAUDE.md), ci.py, src/ap_pipeline/__init__.py and src/ap_pipeline/schedule.json. I did not use any connectors because the inputs and repository files held every fact I needed. The remote URL and committer details in the git log are left out of the output.
- Tools used: Skill:data-engineering-steps, Read, Glob, Grep, Write, tool:repo-access
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 2 | — | — |
| 2 | resumed | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 4, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- Section 1, 'Repository available' row: the file names it lists (`dq.py`, `ingest.py`, `pipeline.py`, `transform.py`, `tests/test_pipeline.py`, `data/sample_feed.csv`) are correct on disk. But the two sources it cites (tool:repo-access and file:ap_pipeline/CLAUDE.md) only say 'src/ap_pipeline/' and 'tests/' and name none of these files. feat.tasks names most of them, and `data/sample_feed.csv` is in no cited source. Cite feat.tasks (TSK-01 'Files in budget' and 'Must not change'), or record the directory listing as evidence. The listing also leaves out `tests/__init__.py`.
- Remove the two duplicated '[Tool result] [ap_pipeline] Branch feature/story at 3fcbe1b; write access confirmed' lines at the end of the file. They look like tool output pasted into the document by mistake, not part of the written output.
- Section 4 says 'No production data, secrets or personal data read or written' and section 2 says 'This step ran no git commands that change state'. Both cite record:brief, but the brief says neither. They are true statements about the agent's own process, so describe them that way (for example, shell commands were blocked for DE-01) instead of citing the brief.
- Section 1, Databricks row: the evidence is 'code search with no matches', but no file or tool view records that search. A rerun of the search over ap_pipeline (excluding .git) for databricks, spark, jdbc, sqlite and database found nothing, so the claim holds. Name the search terms and paths so the gate can reproduce it.
- RAQ-1 (is the `coverage` package installed?) is correctly kept as a non-blocking open question. Because CI cannot run without it (`ci.py` line 14: `import coverage`), DE-02 or the first coding step should confirm it before coding starts, not wait for DE-09/DE-10.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.

## Human evaluation

| Role | By | Decision | Scores | Comment or reason |
|---|---|---|---|---|
| Data engineer | sim.data_engineer | approve | — | automated checks and the independent judge passed |
