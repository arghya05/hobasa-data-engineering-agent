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
| org.data_model | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-04/org.data_model.md | hobasa-agentic-sdlc/features/FEAT-ZD-48600/org-overrides/org.data_model.md | 3b03f15 | source |
| task.repo_access | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-04/task.repo_access.md | hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.repo_access.md | aac590a | DE-01 |

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

- Approach: I followed the DE-04 procedure in the data-engineering-steps skill. First I read the four contracted inputs: feat.tasks, feat.design, org.data_model and task.repo_access. Then I read the existing ap_pipeline sources and pipelines: the tool view of ingest, transform, dq, pipeline, schedule.json, __init__ and the tests, plus ci.py and data/sample_feed.csv, which I opened with Read and Glob. The G9 hook blocks shell commands in this step.

I took background from journey:feat.clarifications (the CL-DE answers) and journey:task.db_changes, both read through context_get with receipts. For secrets in all environments I used the approved DE-02 task.secrets_scope file. I classified each existing element by layer and chose the smallest change set. There is no new source or pipeline, and parse_feed is unchanged because it is header-driven. The optional column is let in with an additive update to transform.to_model (TSK-01). The new delays.py (TSK-02) is listed and left to DE-07 for detail.

The output file already held a stale DE-07 version from an earlier run, and I overwrote it with the DE-04 content. A verifier subagent then checked the draft. It found three issues, which I fixed: the source for "empty when unpaid" is now record:memory, the line about CI and tests is now labelled as agent judgement, and I added the context_list citation for the input commits. No connectors were needed.
- Tools used: Skill (data-engineering-steps), Read, Glob, mcp__hobasa-context__context_list, mcp__hobasa-context__context_get (feat.clarifications, task.db_changes), mcp__hobasa-context__memory_search, Agent (verifier), Write, Edit
- Helper subagents: Agent
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- The output contradicts itself on whether ingestion needs code. The Result row and IN-2 say "The ingestion layer needs no code change", but the IN-3 row files the `transform.to_model` update under the "Ingestion → model boundary" layer. feat.design B1.1 and HC-1 treat that function as transform. State plainly whether DE-04 treats `to_model` as in scope for ingestion or hands it to DE-07, so the two steps do not both claim it.
- Section 5 (secrets) does not carry SSQ-1 from task.secrets_scope section 6. SSQ-1 asks whether the runtime that runs `schedule.json` needs its own credential for the production feed. That is directly relevant to the workbook's "Access for the secrets in all the environments". Production is marked "None / None / None" without this caveat.
- RAQ-1 from task.repo_access is not carried: `ci.py` imports the third-party `coverage` package, and nobody has confirmed it is installed for `/usr/bin/python3`. The output relies on the CI coverage gate (section 4), so the open dependency should be noted.
- EX-1 says "The repository holds no file, URL, connector or credential for it" in the same cell that cites `data/sample_feed.csv`, a file of that feed. Narrow the claim to "no production file path, URL, connector or credential" so it does not contradict itself.
- The requirement text "optional paid_date column (ISO date, empty when unpaid)" is cited as record:memory (journey requirement). It actually comes from the hook-injected journey context, not from memory/MEMORY.md. A more precise citation label would help reviewers trace it.
- IN-7 and the section 4 file budget restate DE-07 transformation detail (`delays_by_vendor` signature, ordering, purity). That is fine for completeness, but a brief note that DE-07 is the authoritative owner would avoid divergence when DE-07 extends this file.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.

## Human evaluation

| Role | By | Decision | Scores | Comment or reason |
|---|---|---|---|---|
| Data engineer | sim.data_engineer | approve | — | automated checks and the independent judge passed |
