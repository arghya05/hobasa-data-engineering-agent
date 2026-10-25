# Evaluate step DE-03: Database changes for FEAT-ZD-48600

- Workbook action: Check the required database changes are completed
- Workbook outcome: Required Database changes are completed

## Outputs to evaluate
- `hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.db_changes.md`

## Inputs the agent was given
- feat.tasks: `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-03/feat.tasks.md`
- feat.design: `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-03/feat.design.md`

## Tool results the spine recorded from the real tools in this step (a valid source; the section 'Tool results' at the end of an output file is written by the spine, not the agent)


[Tool result] [ap_pipeline] No database in this repository's architecture; no migrations required

## Other GitHub files the agent read (valid sources when cited as file:<repository>/<path>)
- `hobasa-data-engineering-agent/.claude/skills/data-engineering-steps/SKILL.md`
- `hobasa-data-engineering-agent/CLAUDE.md`
- `ap-aging-sample/CLAUDE.md`
- `ap-forecast-sample/CLAUDE.md`
- `ap-pipeline-sample/CLAUDE.md`
- `ap-pipeline-sample/README.md`
- `ap-pipeline-sample/src/ap_pipeline/pipeline.py`
- `ap-pipeline-sample/src/ap_pipeline/transform.py`
- `ap-portal-sample/CLAUDE.md`
- `hobasa-agentic-sdlc/CLAUDE.md`
- `hobasa-agentic-sdlc/features/FEAT-ZD-48600/JOURNEY.md`
- `hobasa-agentic-sdlc/memory/MEMORY.md`
- `final_document/hobasa-claude-code-plugin/skills/data-engineering-steps/SKILL.md`
- `final_document/hobasa-claude-code-plugin/skills/grounded-output/SKILL.md`

Shared memory and conventions: `memory/MEMORY.md` and `CLAUDE.md` in the SDLC repository.

## The step brief the agent was given (a valid source, cited as record:brief)

# Step DE-03: Database changes (attempt 1) for FEAT-ZD-48600

- Workbook trigger: Task creation, or any re-entry trigger ¹
- Workbook action: Check the required database changes are completed
- Workbook outcome: Required Database changes are completed
- Owner of this step: Data engineer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- feat.tasks (Engineering tasks with goal, AC, dependencies and size budget): `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-03/feat.tasks.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md at commit b0fd4d2)
- feat.design (HLD, LLD and contracts): `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-03/feat.design.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.design.md at commit 722a98f)

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.db_changes (Completed database changes): write `hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.db_changes.md`


## How to work
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:pypi (Python package metadata: latest version, licence, release dates (GET /pypi/<package>/json)), mcp:codegraph (Code intelligence over the repository: a symbol's source, its callers and callees, the blast radius of a change and the tests it affects (codegraph_explore, codegraph_node and related tools; pass projectPath = the repository checkout)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

## Tool view (cite as tool:repo-access)
ap_pipeline has no database; no migrations exist or are required.

Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (anything missing or unclear: BLOCKING: <what you need and who should answer>; never assume), summary.