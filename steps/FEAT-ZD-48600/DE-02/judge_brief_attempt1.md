# Evaluate step DE-02: Access to secrets for FEAT-ZD-48600

- Workbook action: Check the access for required secrets and ask for access as needed
- Workbook outcome: Secrets are available with access permissions

## Outputs to evaluate
- `hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.secrets_scope.md`

## Inputs the agent was given
- feat.tasks: `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-02/feat.tasks.md`

## Other GitHub files the agent read (valid sources when cited as file:<repository>/<path>)
- `~/.claude/projects/-Users-arghyamukherjee-Documents-automaiton-final-document-e2e-run-runs-run-20261025-123936-feat-zd-48600-workspace-ap-pipeline/0796d890-3980-470b-a3b7-ed23c05133a9/tool-results/hook-12c148e0-05f1-4871-972a-9cc8942c56a6-1-additionalContext.txt`

Shared memory and conventions: `memory/MEMORY.md` and `CLAUDE.md` in the SDLC repository.

## The step brief the agent was given (a valid source, cited as record:brief)

# Step DE-02: Access to secrets (attempt 1) for FEAT-ZD-48600

- Workbook trigger: Task creation, or any re-entry trigger ¹
- Workbook action: Check the access for required secrets and ask for access as needed
- Workbook outcome: Secrets are available with access permissions
- Owner of this step: Data engineer
- Gate (who approves this output): no gate: automated checks, the independent judge and sampled human evaluation

## Inputs (GitHub checkout; read them with Read, or context_get for a receipt)
- feat.tasks (Engineering tasks with goal, AC, dependencies and size budget): `hobasa-data-engineering-agent/inputs/FEAT-ZD-48600/DE-02/feat.tasks.md` (from hobasa-design-agent/outputs/FEAT-ZD-48600/feat.tasks.md at commit b0fd4d2)
- org.permission_table (Allowed tools, secrets and data per agent): restricted, passed by reference only

## Outputs (write each file in full; the spine commits and pushes after evaluation and approval)
- task.secrets_scope (Secrets granted for the task's environments (names only)): write `hobasa-data-engineering-agent/outputs/FEAT-ZD-48600/task.secrets_scope.md`


## How to work
Decide the best way to do this step from the inputs and the workbook action, plan it (TodoWrite for multi-part work), then do it. Helper subagents you can delegate to: explorer, verifier (use the verifier before you finish when the output makes many factual statements). Use the shared memory (memory_search) for decisions and lessons. Connectors approved for you: api:osv (Known vulnerabilities for a dependency version (POST /v1/query with {"package":{"name":...,"ecosystem":"PyPI|npm"},"version":...})), api:pypi (Python package metadata: latest version, licence, release dates (GET /pypi/<package>/json)). Use one only when your inputs do not hold what you need; request a missing one with capability_request. You may also read any other file in the GitHub checkouts your step needs (for example other requirements when grouping, or code); cite it as file:<repository>/<path>.

## Tool view (cite as tool:repo-access)
Tool registry for ap_pipeline: no secrets are required or granted for this repository (permission table).

Finish with the structured result: approach, tools_used, claims (each citing an input item name, a tool view, api:<name>, mcp:<server>, file:<repository>/<path> for any other GitHub file you read, record:intake, record:owner_answer, record:memory or record:brief for this brief; a claim drawing on several sources lists them separated by ';'; every number must appear in what you cite), open questions (BLOCKING: only when you cannot finish), summary.