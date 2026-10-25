# DE-06 Data Mapping (Data Engineering agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Previous outcome  
**Action:** Update the data mapping based on the data model and fetch the latest data mappings based on the customer input  
**Outcome expected:** Updated Data mapping and transformation steps

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| feat.data_model_change | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-06/feat.data_model_change.md | hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/feat.data_model_change.md | 7b68a18 | DE-05 |
| org.customer_mappings | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-06/org.customer_mappings.md | hobasa-agentic-sdlc/features/FEAT-ZD-48600/org-overrides/org.customer_mappings.md | 3b03f15 | source |

Full brief per attempt: `brief_attempt<N>.md` (trigger event, owner answers, tool view, refusal reasons).

## Outputs (what the agent produced)

| Item | GitHub file |
|---|---|
| feat.mappings | hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/feat.mappings.md |

## Code

Repository `ap-pipeline-sample`, branch `feature/story`, commit `3fcbe1b`

```
no file changes in this step
```

## How the agent worked

- Approach: I followed the DE-06 procedure in the data-engineering-steps skill, which reads feat.data_model_change and org.customer_mappings and writes feat.mappings. I read both contracted inputs, the shared memory and journey board, and the ap_pipeline tool view. I also read data/sample_feed.csv from the checkout to see which feed version the repository exercises. I compared the vendor's v3 contract with the DE-05 model change item by item: version, columns, spelling, format, target and schedule. Then I wrote the source-to-target mapping, the processing order for each row, the derived late-payment history mapping, the schedule check and test cases for DE-08/DE-09. The output describes only what the inputs, the tool view and the cited repository file hold. It makes no statement about CI or git results, because no cited source holds them. The mapping is implemented in DE-08, so no code changed. I used no connectors, because the inputs held everything needed.
- Tools used: Skill (data-engineering-steps), Read, Glob, Write, Edit
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | 1 | {'correct': 4, 'complete': 5, 'grounded': 3, 'useful': 5} |
| 2 | resumed | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 5} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- Section 5 names the step "DE-13 Scheduling the pipelines" but cites only org.customer_mappings and schedule.json. The step name comes from the data-engineering-steps skill (SKILL.md: '## DE-13 Scheduling the pipelines') and the agent README. Cite that source, or just say 'the scheduling step'.
- Section 2 gives `amount` the target type 'Decimal, 2 dp', but the rule `Decimal(...)` does no quantizing. transform.py accepts '1' unchanged, and test_paid_flag uses amount 1. Say whether 2 dp is enforced by the mapping or is only the registered model type. Otherwise list it as an open question for DE-08/DE-09.
- Section 7 says no code changed but does not say whether CI was run in this step. The brief allows `/usr/bin/python3 ci.py`. State plainly that no CI was run (or give the real result), so the gate does not have to infer it.
- The derived Delay history rule cites record:memory D-2026-031 for calendar days. That decision is about aging buckets. The calendar-day delay rule actually comes from feat.data_model_change 2.4 (NF-2/DC-2), so cite that as the main source.
- The mapping could note that data/sample_feed.csv row INV-2 is paid=true and has no paid_date column (v2). Under the mapping it gives a paid record with paid_date=None that is kept but left out of the Delay history. That makes it a handy existing fixture for the 'paid with no paid_date' rule.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.

## Human evaluation

| Role | By | Decision | Scores | Comment or reason |
|---|---|---|---|---|
| Data engineer | sim.data_engineer | approve | — | automated checks and the independent judge passed |
