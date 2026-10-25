# Step DE-01: Checking the coding prerequisites ⁹ (attempt 1) for FEAT-ZD-48600

- Workbook trigger: Task creation, or any re-entry trigger ¹
- Workbook action: Check for access to Databricks, repo availability, repo creation and write access for the same
- Workbook outcome: Databricks and Repo is available with write access
- Owner of this step: Data engineer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- feat.tasks (Engineering tasks with goal, AC, dependencies and size budget): `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-01/feat.tasks.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md at commit b0fd4d2)
- org.engineering_stack (Repositories, languages, versions and exact commands): `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-01/org.engineering_stack.md` (from hobasa-agentic-sdlc/org/org.engineering_stack.md at commit 3b03f15)

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.repo_access (Repository (and Databricks) availability and write access): write `hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.repo_access.md`


## How to work
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:pypi (Python package metadata: latest version, licence, release dates (GET /pypi/<package>/json)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

## Tool view (cite as tool:repo-access)
Repository ap_pipeline (Python 3.9 (standard library)) is available locally; branch feature/story created from dev at 3fcbe1b; the agent identity has write access (git status clean). No Databricks or other workspace is used by this repository.

Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (BLOCKING: only when you cannot finish), summary.