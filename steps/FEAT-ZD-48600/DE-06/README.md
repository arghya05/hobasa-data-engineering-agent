# DE-06 · Data Mapping

**Agent:** Data Engineering · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** Data engineer: approve by sim.data_engineer

**What this step does:** Update the data mapping based on the data model and fetch the latest data mappings based on the customer input

**Expected outcome:** Updated Data mapping and transformation steps

**Started by:** Previous outcome

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `feat.data_model_change` | Data Model change for the feature | Data Engineering agent · DE-05 | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-06/feat.data_model_change.md) · [original](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/outputs/FEAT-ZD-48600/feat.data_model_change.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-engineering-agent/main/docs/FEAT-ZD-48600/DE-05_feat-data_model_change.html) · [PDF](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/docs/FEAT-ZD-48600/DE-05_feat-data_model_change.pdf) |
| 2 | `org.customer_mappings` | Versioned customer data-mapping contracts and schedule frequency | organisation source | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-06/org.customer_mappings.md) · [original](https://github.com/arghya05/hobasa-agentic-sdlc/blob/main/features/FEAT-ZD-48600/org-overrides/org.customer_mappings.md) |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `feat.mappings` | Updated data mappings and transformation steps | [Markdown](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/outputs/FEAT-ZD-48600/feat.mappings.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-engineering-agent/main/docs/FEAT-ZD-48600/DE-06_feat-mappings.html) · [PDF](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/docs/FEAT-ZD-48600/DE-06_feat-mappings.pdf) |

**Code:** repository [ap-pipeline-sample](https://github.com/arghya05/ap-pipeline-sample) · branch [feature/story](https://github.com/arghya05/ap-pipeline-sample/tree/feature/story) · commit [3fcbe1b](https://github.com/arghya05/ap-pipeline-sample/commit/3fcbe1b)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | — | — |
| 2 | continued | 1 | 1 | {'correct': 5, 'complete': 4, 'grounded': 4, 'useful': 5} |
| 3 | continued | 1 | 1 | {'correct': 4, 'complete': 5, 'grounded': 4, 'useful': 5} |
| 4 | continued | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 5, 'useful': 5} |

Improvements the independent judge suggests (non-blocking):

- Citation format: many rows cite `journey:feat.design`, but neither the brief nor the grounded-output skill lists `journey:` as a valid citation. feat.design was read from disk, so the documented form is `file:hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md`. Every cited section (B1.1, B1.2, B1.4, B1.5, HC-3, DC-1, DC-2, DD-6, DD-7, NF-10, SE-4) does support its statement.
- MQ-4 (whether a `paid_date` cell holding only whitespace counts as blank or invalid) is marked non-blocking, yet the output itself says DE-08 must not pick a behaviour silently. The SDLC CLAUDE.md says anything unclear becomes a BLOCKING question to its owner. The gate should decide whether MQ-4 should be BLOCKING to the Data modeler instead of sitting under 'No blocking questions'.
- MQ-2 sets the customer contract ('empty when unpaid') against DD-7 (an unpaid row with a valid paid_date is accepted). Calling it a 'wording difference' that the mapping overrides in favour of the Data Model change is a judgement call. The owner of the customer contract should confirm it, or it should be raised as BLOCKING, because the contract is the customer input this step is meant to apply.
- The 'feed owner' named as an owner in MQ-2 is not a role in any source. Name an owner from the sources, or mark this one as still to be identified.
- The section 2 mapping table leaves out some reject paths the code has today: a missing `due_date` column raises KeyError, an invalid `amount` raises InvalidOperation, and a missing `amount` raises KeyError. Listing them would make the mapping a complete reference for DE-08 and DE-09.
- Section 1 says the column lists agree on 'same names and order'. The Data Model change only says 'v2 columns plus paid_date', and csv.DictReader keys rows by header name, so column order does not matter. Either drop the order claim or say that order does not matter.

## How the agent worked

- **Approach:** I followed DE-06 in the data-engineering-steps skill. I read both contracted inputs with context_get to get receipts: feat.data_model_change at 3943254 and org.customer_mappings at e8afce8. As background I read the approved feat.design (722a98f) for B1.1, B1.2, B1.4, B1.5, HC-3, DC-1, DC-2, DD-6, DD-7, NF-10 and SE-4. The ap_pipeline code and tests came from the step's tool view, and I read data/sample_feed.csv from the checkout. Where the file names a later step, I cite the skill file that defines it. I compared the latest customer contract (feed v3 columns, paid_date -> Invoice.paid_date, schedule) with the Data Model change field by field. From that I wrote feat.mappings in full: the source-to-target mapping, the ordered transformation steps, the derived Delay-history mapping, the schedule mapping, synthetic validation cases, and open questions, each with a source. Before finishing I had the verifier subagent check every statement in the file against the source it cites. I fixed everything it flagged: the result wording, the paid None-cell behaviour, the citation for no change to pipeline.py, the purity citation, the open-question sources, and the whitespace-only case, which is now an open question with no assumed answer. Mapping is a documentation step and coding is DE-08, so I changed no code. CI passes on the baseline and git status shows a clean working tree.
- **Tools used:** Skill (data-engineering-steps), mcp__hobasa-context__context_get (feat.data_model_change, org.customer_mappings, feat.design), Read, Glob, Write, Agent (verifier), Bash (/usr/bin/python3 ci.py, git status)
- **Helper subagents:** Agent
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
