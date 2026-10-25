# DE-02 Access to secrets (Data Engineering agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Task creation, or any re-entry trigger ¹  
**Action:** Check the access for required secrets and ask for access as needed  
**Outcome expected:** Secrets are available with access permissions

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| feat.tasks | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-02/feat.tasks.md | hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md | b0fd4d2 | DES-04 |
| org.permission_table | not applicable or by reference | — | — | — |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| task.secrets_scope | hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.secrets_scope.md |

## Code

Repository `ap-pipeline-sample`, branch `feature/story`, commit `3fcbe1b`

```
no file changes in this step
```

## How the agent worked

- Approach: Attempt 2 fixes the G1 refusal. The SS-1 row said that "this lane never reads source-system production data", and neither cited source states that general rule. I removed the sentence. SS-1 now says only what feat.tasks supports: feed v3 in production (PQ-6) gates the release of TSK-01, not its coding, and coding builds on synthetic v3 rows. The re-entry handling is quoted from the brief's trigger ("Task creation, or any re-entry trigger"). I then checked every other statement the same way. I rewrote SS-2 because it named later lane steps (DE-13, DE-14) and email/Teams notifications that no cited source supports. It now says only that no input names a secret for any later step, citing tool:repo-access and the brief's trigger. I also replaced the "Production data read: None" validation row with the items this step actually read. The rest is unchanged. I re-checked the two Data Engineering tasks (TSK-01, TSK-02) in feat.tasks, which I fetched with context_get in attempt 1 for a receipt. I used the approved DE-01 output task.repo_access as background. The permission table's verdict comes from the tool view, because org.permission_table is restricted and passed by reference only. The outcome is that no secret is required, none is granted, there is no gap and no access request is raised. I rewrote the output file in full and kept the tool-result line the spine had appended.
- Tools used: Skill (data-engineering-steps), mcp__hobasa-context__context_list, mcp__hobasa-context__context_get (feat.tasks, task.repo_access), Read (current task.secrets_scope.md), Grep (ap_pipeline checkout, secret-reference cross-check; not cited), Write (task.secrets_scope.md)
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | 1 | {'correct': 5, 'complete': 4, 'grounded': 4, 'useful': 4} |
| 2 | resumed | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- The output cites journey:task.repo_access (P-1, P-5, P-7 and section 2) several times, but the DE-01 output is not among the files listed as read for this step. Its content does match those citations (the Databricks P-1 not-applicable finding, branch `feature/story`, `/usr/bin/python3 ci.py`, and 'no workspace credential to check'). Fetch it through context_get, or list it as read, so the read check counts the citation.
- Section 2's row for the Integration (`dev`) and released (`main`) branches cites feat.tasks section 2. That section only names the branches. The 'none required or granted' part rests on tool:repo-access alone, which is scoped to the repository and not to each environment. Say plainly that the per-environment 'None' is inferred from the repository-wide tool view.
- SS-1 says DE-02 'runs again on that re-entry trigger' if the release needs a feed credential. Say who raises that trigger, or list it as a non-blocking open question with an owner, so a future feed v3 (PQ-6) credential need is not lost.
- The trailing '[Tool result] ... no secrets required or granted' lines are duplicated at the end of the file. If the agent wrote them rather than the harness, remove them.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.

## Human evaluation

| Role | By | Decision | Scores | Comment or reason |
|---|---|---|---|---|
| Data engineer | sim.data_engineer | approve | — | automated checks and the independent judge passed |
