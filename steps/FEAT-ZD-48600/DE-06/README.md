# DE-06 Data Mapping (Data Engineering agent)

**Item:** FEAT-ZD-48600 · **status:** done
**Trigger:** Previous outcome  
**Action:** Update the data mapping based on the data model and fetch the latest data mappings based on the customer input  
**Outcome expected:** Updated Data mapping and transformation steps

## Inputs (what the agent was given)

| Item | Prepared input (this repository) | Read from (producer in GitHub) | Commit | Produced by |
|---|---|---|---|---|
| feat.data_model_change | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-06/feat.data_model_change.md | hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/feat.data_model_change.md | 379f011 | DE-05 |
| org.customer_mappings | hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-06/org.customer_mappings.md | hobasa-agentic-sdlc/features/FEAT-ZD-48600/org-overrides/org.customer_mappings.md | a6b3e2a | source |

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

- Approach: I followed the data-engineering-steps skill for DE-06. First I got receipts for both contracted inputs through context_get: feat.data_model_change at 379f011 and org.customer_mappings at a6b3e2a. I searched shared memory for mapping decisions and found none beyond D-2026-031 and D-2026-044. I read the ap_pipeline code and schedule.json from the step's tool view. Then I checked the latest customer contract (feed v3 columns, the paid_date -> Invoice.paid_date mapping and the schedule) against the Data Model change, one field at a time. I wrote feat.mappings with these parts: the customer contract compared with the model, the feed-row to Invoice mapping, ordered transformation steps, the derived Invoice to Delay-history mapping, a schedule comparison, mapping validation cases on synthetic rows, and open questions. This step is a mapping step, so I changed no code; the coding step implements it. I ran CI once on the unchanged repo and it passed, but I cite no CI figures because this step has no tool:ci view. I did not use any connector, because the inputs held every fact I needed.
- Tools used: Skill (data-engineering-steps), Read, mcp__hobasa-context__context_get, mcp__hobasa-context__memory_search, Bash (baseline CI run, no code changed), Write, Edit
- Helper subagents: none
- Connector calls: none

## Attempts (checks before any person saw the output)

| Attempt | Session | Checks failed | Judge unsupported | Judge scores |
|---|---|---|---|---|
| 1 | new | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 4} |

## Improvements the independent judge suggests (non-blocking, for the people at the gate)

- Section 6 leaves out validation cases that the cited sections already define. Missing cases: a paid record with blank `paid_date` is kept with `paid_date = None` (feat.data_model_change 2.2 / B1.4); an unpaid record with a valid `paid_date` is kept (DD-7); a `paid_date` before `due_date` gives a negative delay that is kept (2.2, 2.4); a short row where `paid_date` is `None` gives `None`. Adding these would make the mapping rules fully testable for the coding and unit-test steps.
- Section 6 has no cases for the Delay history mapping (section 4), although feat.data_model_change section 4 has one ('Delay history values are int and keys are vendors with at least one paid record carrying a paid_date'). Add cases for feed order, first-seen vendor order, exclusion of unpaid records and records without `paid_date`, and the record list staying unchanged.
- Section 8 does not carry forward DMQ-A (whether to register the `reason` value `paid_date` and the Delay history org-wide) or DMQ-D (whether to re-export `delays_by_vendor`). Neither is answered by the customer contract. Listing them as still open, or saying they are out of scope for mapping, would make the hand-off cleaner.
- MQ-B is well raised. The interim answer could also name the data-quality risk: if the feed owner sends `paid_date` on unpaid rows despite the contract, nothing in the report surfaces it. A count or warning could be considered.
- Section 4 cites D-2026-031 for `delay_days` in calendar days. That decision is about aging buckets ('calendar days after due date'), so it only indirectly supports a delay-day definition. The direct source is feat.data_model_change 2.4, which is also cited.

Details: `result_attempt<N>.json` (claims with sources), `checks_attempt<N>.json`, `judge_attempt<N>.json`.

## Human evaluation

| Role | By | Decision | Scores | Comment or reason |
|---|---|---|---|---|
| Data engineer | sim.data_engineer | approve | — | automated checks and the independent judge passed |
