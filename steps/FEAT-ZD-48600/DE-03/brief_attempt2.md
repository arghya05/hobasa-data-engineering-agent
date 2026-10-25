Your previous result for this step was refused. Fix every point, rewrite the output files in full, and return the structured result again.

## Refusal reasons
G2 claim cites file:ap_pipeline/src/ap_pipeline/ but the agent never read that file in this session
G2 claim cites file:ap_pipeline/data/sample_feed.csv but the agent never read that file in this session

## Step brief (unchanged except the attempt number)
# Step DE-03: Database changes (attempt 2) for FEAT-ZD-48600

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
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:pypi (Python package metadata: latest version, licence, release dates (GET /pypi/<package>/json)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

## Tool view (cite as tool:repo-access)
ap_pipeline has no database; no migrations exist or are required.

Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (BLOCKING: only when you cannot finish), summary.