# DE-01 Checking the coding prerequisites ⁹ (Data Engineering agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Task creation, or any re-entry trigger ¹  
**Action:** Check for access to Databricks, repo availability, repo creation and write access for the same  
**Outcome expected:** Databricks and Repo is available with write access

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| feat.tasks | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-01/feat.tasks.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md | b0fd4d2 | DES-04 |
| org.engineering_stack | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-01/org.engineering_stack.md | hobasa-agentic-sdlc/org/org.engineering_stack.md | a6b3e2a | source |

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

- Approach: Attempt 4 fixes the two G1 findings. (1) I removed "The spine runs push and merge." No source says the spine merges, and section 2 now states only commit facts cited to org.engineering_stack and feat.tasks. (2) I rewrote RA-2 so it says only what tool:repo-access supports: write access is as the tool view reports, and this output claims no push or CI run of its own. I also removed other statements that no cited source backs: the 'branch the spine created' wording, 'Push is the spine's job (DE-10)', 'DE-09 confirms the CI environment' and 'DE-02 still checks the permission table'. I rewrote the file in full, without the two '[Tool result]' lines the spine had appended in earlier attempts; the spine can add its own receipt line again. All claims cite only feat.tasks, org.engineering_stack, tool:repo-access or record:brief, which keeps the G2 fix from attempt 3. No connector was needed.
- Tools used: Read, Write, mcp:hobasa-context context_list (attempt 3), Skill:data-engineering-steps (attempt 1)
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 9 | — | — |
| 2 | resumed | 11 | — | — |
| 3 | resumed | 2 | 2 | {'correct': 4, 'complete': 4, 'grounded': 4, 'useful': 4} |
| 4 | resumed | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- P-8 cites only feat.tasks for ap_pipeline being at 0.4.0. The agent also read file:ap_pipeline/src/ap_pipeline/__init__.py, which shows __version__ = "0.4.0". Citing it would ground the version in the code as well as the plan.
- P-4 (main and dev exist) and P-5 (feature/story created from dev at 3fcbe1b) could also cite the git refs the agent read: .git/refs/heads/dev is 3fcbe1bad11e…, and .git/logs/refs/heads/feature/story records 'branch: Created from HEAD' at 3fcbe1b. As written, P-4 rests on the org convention and the task file, not on the repository.
- A stray line sits at the end of the file after the validation table: '[Tool result] [ap_pipeline] Branch feature/story at 3fcbe1b; write access confirmed'. It matches the tool view but has no section or citation. Remove it or put it in a cited section.
- P-1 says Databricks is not applicable. The workbook outcome reads 'Databricks and Repo is available with write access', so it would help to state once, in the Result row, that the Databricks half of the outcome is satisfied as not applicable under org.engineering_stack ('No repository uses Databricks or a database'). That makes clear to reviewers that it was not skipped.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.

## Human evaluation

| Role | By | Decision | Scores | Comment or reason |
|---|---|---|---|---|
| Data engineer | sim.data_engineer | approve | — | automated checks and the independent judge passed |
