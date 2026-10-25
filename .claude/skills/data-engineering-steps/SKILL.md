---
name: data-engineering-steps
description: Procedures, inputs, outputs and gates for every Data Engineering agent step in the Hobasa agentic SDLC. Use when running any DE-01, DE-02, DE-03, DE-04, DE-05, DE-06… step.
---

# Data Engineering agent steps

## DE-01 Checking the coding prerequisites ⁹

- Trigger: Task creation, or any re-entry trigger ¹
- Action: Check for access to Databricks, repo availability, repo creation and write access for the same
- Outcome: Databricks and Repo is available with write access
- Reads: feat.tasks, org.engineering_stack
- Writes: task.repo_access
- Gate: automated checks; sampled human evaluation

## DE-02 Access to secrets

- Trigger: Task creation, or any re-entry trigger ¹
- Action: Check the access for required secrets and ask for access as needed
- Outcome: Secrets are available with access permissions
- Reads: feat.tasks, org.permission_table
- Writes: task.secrets_scope
- Gate: automated checks; sampled human evaluation

## DE-03 Database changes

- Trigger: Task creation, or any re-entry trigger ¹
- Action: Check the required database changes are completed
- Outcome: Required Database changes are completed
- Reads: feat.tasks, feat.design
- Writes: task.db_changes
- Gate: automated checks; sampled human evaluation

## DE-04 Identify the sources/pipelines to be changed (Ingestion) ⁵

- Trigger: Previous outcome
- Action: Read the existing sources, pipelines and understand it.
Identify the sources or pipelines that needs to be created/updated with minimum lines of code for the ingestion layer
Access for the secrets in all the environments ⁸
- Outcome: The list of sources, pipelines needs to be created/updated with minimum lines of code for the ingestion layer
- Reads: feat.tasks, feat.design, org.data_model, task.repo_access
- Writes: task.components
- Gate: automated checks; sampled human evaluation

## DE-05 Data Model Changes

- Trigger: Previous outcome
- Action: Update the data model based on the source systems and ingested data
- Outcome: Updated data model
- Reads: task.components, org.data_model
- Writes: feat.data_model_change
- Gate: automated checks; sampled human evaluation

## DE-06 Data Mapping

- Trigger: Previous outcome
- Action: Update the data mapping based on the data model and fetch the latest data mappings based on the customer input
- Outcome: Updated Data mapping and transformation steps
- Reads: feat.data_model_change, org.customer_mappings
- Writes: feat.mappings
- Gate: automated checks; sampled human evaluation

## DE-07 Identify the sources/pipelines to be changed (Transformation) ⁵

- Trigger: Previous outcome
- Action: Read the existing sources, pipelines and understand it.
Identify the sources or pipelines that needs to be created/updated with minimum lines of code for the Transformation pipeline
Access for the secrets in all the environments
- Outcome: The list of sources, pipelines needs to be created/updated with minimum lines of code for the transformation pipeline
- Reads: feat.mappings, task.components
- Writes: task.components
- Gate: automated checks; sampled human evaluation

## DE-08 Coding

- Trigger: Previous outcome
- Action: Coding for the implementation required for changes in the sources or pipelines with proper comments and required documentation
Access for the secrets in all the environments
- Outcome: Updated repo with the new implemented code changes
- Reads: task.components, feat.mappings, feat.design, org.engineering_stack, org.lessons, task.secrets_scope
- Writes: task.code
- Gate: automated checks; sampled human evaluation

## DE-09 Unit Test Cases

- Trigger: Code completion
- Action: Write the unit test cases to cover the updated or newly written code/transformation logic as per the requirement documents with at least 90 percent of the code coverage.
Schema validation ²
- Outcome: Unit testing completed
- Reads: task.code, feat.stories, feat.data_model_change
- Writes: task.unit_tests
- Gate: automated checks; sampled human evaluation

## DE-10 Commit & Push

- Trigger: Unit Testing Completion
- Action: Commit the code to the local repository with proper comment for back tracing into the user story.

If any merge conflicts at times and needs to be manually reviewed and resolve

Push the code to central/main repository
Access for the secrets in all the environments
- Outcome: Code Push completed and PR is being raised
- Reads: task.code, task.unit_tests, feat.stories
- Writes: task.commit
- Gate: automated checks; sampled human evaluation

## DE-11 Code Quality Failure ³ ⁹

- Trigger: CI Pipeline Failure
- Action: Identify the cause for failure & implement the necessary code changes to fix it
Access for the secrets in all the environments
- Outcome: CI Pipeline failure fixed and pushed to the repo
- Reads: task.commit
- Writes: task.code, task.commit
- Gate: automated checks; sampled human evaluation; runs only when only when CI fails

## DE-12 Share the PR for review ⁹

- Trigger: Completion of CI Pipeline
- Action: Identify the code push and raise a PR on the commit
Access for the secrets in all the environments
- Outcome: Pull request URL
- Reads: task.commit
- Writes: task.pr
- Gate: PR reviewer

## DE-13 Scheduling the pipelines

- Trigger: Pull request URL
- Action: Based on customer frequency, the pipelines would be scheduled
- Outcome: Audit log for the scheduled triggered time
- Reads: task.pr, org.customer_mappings
- Writes: task.schedule
- Gate: automated checks; sampled human evaluation

## DE-14 Pipeline Status

- Trigger: Data Pipelines
- Action: Status of the pipeline load and data quality report
- Outcome: Notifications in email and teams
- Reads: task.schedule
- Writes: task.pipeline_status
- Gate: automated checks; sampled human evaluation

## DE-15 Pipeline Re run

- Trigger: Data Pipeline notifications
- Action: Re-run the required data pipelines based on their status.
Iterate it for three times, if failure continuous then get the help from human ⁷
- Outcome: Notifications in email and teams
- Reads: task.pipeline_status
- Writes: task.pipeline_status
- Gate: automated checks; sampled human evaluation

## DE-16 Update the defect ⁴

- Trigger: PR Approval
- Action: Add the appropriate comment to the defect and assign it back to QA team for re-testing
The status should be updated as ready to test
- Outcome: Defect Updated
- Reads: feat.defects, task.pr
- Writes: feat.defects
- Gate: automated checks; sampled human evaluation; runs only when only when the task came from a defect

