# DE-05 · Data Model Changes

**Agent:** Data Engineering · **Item:** FEAT-ZD-48600 · **Status:** ✅ done
**Approved by:** Data engineer: approve by sim.data_engineer

**What this step does:** Update the data model based on the source systems and ingested data

**Expected outcome:** Updated data model

**Started by:** Previous outcome

## Input: what the agent received

| # | Input | What it is | Produced by | Open |
|---|---|---|---|---|
| 1 | `task.components` | Components, layers, sources or pipelines to change | Data Engineering agent · DE-04 | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-05/task.components.md) · [original](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/outputs/FEAT-ZD-48600/task.components.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-engineering-agent/main/docs/FEAT-ZD-48600/DE-04_task-components.html) · [PDF](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/docs/FEAT-ZD-48600/DE-04_task-components.pdf) |
| 2 | `org.data_model` | Data Model and Knowledge Graph | organisation source | [input copy](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/inputs/FEAT-ZD-48600/DE-05/org.data_model.md) · [original](https://github.com/arghya05/hobasa-agentic-sdlc/blob/main/features/FEAT-ZD-48600/org-overrides/org.data_model.md) |

The full brief the agent was given, attempt by attempt: `brief_attempt<N>.md` in this folder.

## Output: what the agent produced

| # | Output | What it is | Open |
|---|---|---|---|
| 1 | `feat.data_model_change` | Data Model change for the feature | [Markdown](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/outputs/FEAT-ZD-48600/feat.data_model_change.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-engineering-agent/main/docs/FEAT-ZD-48600/DE-05_feat-data_model_change.html) · [PDF](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/docs/FEAT-ZD-48600/DE-05_feat-data_model_change.pdf) |

**Code:** repository [ap-pipeline-sample](https://github.com/arghya05/ap-pipeline-sample) · branch [feature/story](https://github.com/arghya05/ap-pipeline-sample/tree/feature/story) · commit [3fcbe1b](https://github.com/arghya05/ap-pipeline-sample/commit/3fcbe1b)

```
no file changes in this step
```

## Checks and approval

| Attempt | Session | Problems found by the checks | Unsupported statements (judge) | Judge scores |
|---|---|---|---|---|
| 1 | new | 1 | — | — |
| 2 | continued | 4 | 4 | {'correct': 4, 'complete': 5, 'grounded': 3, 'useful': 5} |
| 3 | continued | 0 | 0 | {'correct': 5, 'complete': 4, 'grounded': 4, 'useful': 5} |

Improvements the independent judge suggests (non-blocking):

- Row 'paid_date value in the feed' (section 2) and the 'Format in the feed' row (3.2) cite `record:memory (JOURNEY requirement: ...)`. The quoted text 'optional paid_date column (ISO date, empty when unpaid)' is in features/FEAT-ZD-48600/JOURNEY.md line 3, not in memory/MEMORY.md. Cite it as file:hobasa-agentic-sdlc/features/FEAT-ZD-48600/JOURNEY.md. The fact holds, and in 3.2 feat.design B1.2 also supports it.
- Citations use `journey:feat.design`, which is not one of the forms the brief allows (input item, tool:repo, api:, mcp:, file:<repo>/<path>, record:*). The content checks out against hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md, so cite it as file:hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md to keep provenance clear.
- DMQ-1 records a real contradiction: org/org.data_model.md says `vendor (str, optional)` and the feature override says `vendor (str)`. It is marked non-blocking, with an interim choice to follow the override. The SDLC CLAUDE.md says contradictions become BLOCKING questions. Either raise it as BLOCKING to the data modeler, or state plainly that the brief makes the override the contracted org.data_model input for this feature, so no choice is being assumed.
- DMQ-2 (who updates org/org.data_model.md after release) is a governance gap that affects the 'Updated data model' outcome. Name a concrete owner from the org files, if one is recorded, instead of a proposed role.

## How the agent worked

- **Approach:** I followed DE-05 in the data-engineering-steps skill. First I read both contracted inputs: task.components at 7cb32bd and the feature override of org.data_model at e8afce8. I also read the organisation's baseline org/org.data_model.md so I could compare the model before and after the feature. I used context_get to read the approved feat.design as background, which gives a receipt for its record contracts (DC-1, DC-2, B1.2 to B1.4, DD-6, DD-7, NF-4). Every statement about the ap_pipeline code rests on the step's tool view (tool:repo) alone, and the output makes no claim about a CI run. The output covers: the change to the source system, the Invoice entity before and after, the paid_date attribute specification, the new reject reason, the derived delay-history view, a knowledge-graph diagram, compatibility, and a column-to-attribute handover for DE-06. I changed no code, because task.components assigns the implementation to TSK-01 and TSK-02 in DE-08. Where the baseline model and the override differ, I raised the difference as an open question rather than resolving it myself.
- **Tools used:** Read, mcp__hobasa-context__context_get (feat.design), Write
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
