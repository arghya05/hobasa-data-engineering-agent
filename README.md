# Hobasa Data Engineering agent

Data Engineering - Coding Agent in the Hobasa agentic SDLC. Owner: Data engineering lead; data modelers.. It runs on Claude Code: every step is one Claude Code session started by the Hobasa spine, with this agent's prompt and step skill (`.claude/`), helper subagents, guardrail hooks and the shared context server.

**Reads from GitHub:** each step's inputs are prepared in `inputs/<trace>/<STEP>/` with the repository, path and commit they came from (other agents' `outputs/`, the requirements and organisation sources in `hobasa-agentic-sdlc`, code in the `*-sample` repositories).

**Writes back to GitHub:** approved outputs in `outputs/<trace>/`; every step run in `steps/<trace>/<STEP>/` (brief, attempts, claims, checks, independent judge, human decision). Code changes go to the `*-sample` repositories. Journey, evaluations and shared memory: `hobasa-agentic-sdlc`.

## Steps

| Step | Name | Reads | Writes | Gate |
|---|---|---|---|---|
| DE-01 | Checking the coding prerequisites ⁹ | feat.tasks, org.engineering_stack | task.repo_access | checks + sampled evaluation |
| DE-02 | Access to secrets | feat.tasks, org.permission_table | task.secrets_scope | checks + sampled evaluation |
| DE-03 | Database changes | feat.tasks, feat.design | task.db_changes | checks + sampled evaluation |
| DE-04 | Identify the sources/pipelines to be changed (Ingestion) ⁵ | feat.tasks, feat.design, org.data_model, task.repo_access | task.components | checks + sampled evaluation |
| DE-05 | Data Model Changes | task.components, org.data_model | feat.data_model_change | checks + sampled evaluation |
| DE-06 | Data Mapping | feat.data_model_change, org.customer_mappings | feat.mappings | checks + sampled evaluation |
| DE-07 | Identify the sources/pipelines to be changed (Transformation) ⁵ | feat.mappings, task.components | task.components | checks + sampled evaluation |
| DE-08 | Coding | task.components, feat.mappings, feat.design, org.engineering_stack, org.lessons, task.secrets_scope | task.code | checks + sampled evaluation |
| DE-09 | Unit Test Cases | task.code, feat.stories, feat.data_model_change | task.unit_tests | checks + sampled evaluation |
| DE-10 | Commit & Push | task.code, task.unit_tests, feat.stories | task.commit | checks + sampled evaluation |
| DE-11 | Code Quality Failure ³ ⁹ | task.commit | task.code, task.commit | checks + sampled evaluation |
| DE-12 | Share the PR for review ⁹ | task.commit | task.pr | PR reviewer |
| DE-13 | Scheduling the pipelines | task.pr, org.customer_mappings | task.schedule | checks + sampled evaluation |
| DE-14 | Pipeline Status | task.schedule | task.pipeline_status | checks + sampled evaluation |
| DE-15 | Pipeline Re run | task.pipeline_status | task.pipeline_status | checks + sampled evaluation |
| DE-16 | Update the defect ⁴ | feat.defects, task.pr | feat.defects | checks + sampled evaluation |

<!-- work:FEAT-ZD-48600:start -->
## Work done for FEAT-ZD-48600

| Step | Name | Status | Input | Output | Details |
|---|---|---|---|---|---|
| DE-01 | Checking the coding prerequisites ⁹ | done | `feat.tasks`, `org.engineering_stack` | `task.repo_access`: [Markdown](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/outputs/FEAT-ZD-48600/task.repo_access.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-engineering-agent/main/docs/FEAT-ZD-48600/DE-01_task-repo_access.html) · [PDF](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/docs/FEAT-ZD-48600/DE-01_task-repo_access.pdf) | [step record](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/steps/FEAT-ZD-48600/DE-01/README.md) |
| DE-02 | Access to secrets | done | `feat.tasks`, `org.permission_table` | `task.secrets_scope`: [Markdown](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/outputs/FEAT-ZD-48600/task.secrets_scope.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-engineering-agent/main/docs/FEAT-ZD-48600/DE-02_task-secrets_scope.html) · [PDF](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/docs/FEAT-ZD-48600/DE-02_task-secrets_scope.pdf) | [step record](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/steps/FEAT-ZD-48600/DE-02/README.md) |
| DE-03 | Database changes | done | `feat.tasks`, `feat.design` | `task.db_changes`: [Markdown](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/outputs/FEAT-ZD-48600/task.db_changes.md) · [HTML](https://htmlpreview.github.io/?https://raw.githubusercontent.com/arghya05/hobasa-data-engineering-agent/main/docs/FEAT-ZD-48600/DE-03_task-db_changes.html) · [PDF](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/docs/FEAT-ZD-48600/DE-03_task-db_changes.pdf) | [step record](https://github.com/arghya05/hobasa-data-engineering-agent/blob/main/steps/FEAT-ZD-48600/DE-03/README.md) |
<!-- work:FEAT-ZD-48600:end -->
