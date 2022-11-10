# Workflow to automatically add issues to a GitHub project

A shared workflow `.yaml` file contains the GitHub GraphQL API queries and mutations to automatically add any issues that are opened or reopened to a GitHub project board.

See the [Reusable workflows documentation](https://docs.github.com/en/actions/using-workflows/reusing-workflows) from GitHub for more details on how this is configured in practice.

## Security

Call to the GitHub APIs requires the use of a GitHub [personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token). 

Unfortunately, only the classic tokens seem to give the correct org/repository settings at the moment. The scopes required are `[project, read:org, repo]`.

## Using the workflow

1. In the repository where the automation is needed, set the _repository_ secret `AUTOMATION_GH_PAT` with the current GitHub PAT that has the needed permissions to run the workflow.
1. Create a workflow similar to the following (e.g. `.github/workflows/add-issue-to-project.yaml`):
```
name: Add new/reopened issues to project

on:
  issues:
    types:
      - opened
      - reopened

jobs:
  add-issue-to-project:
    uses: futurice/reusable-github-workflows/add-issue-to-project/workflow.yaml@main
    with:
      issueNodeId: ${{ github.event.issue.node_id }}
      projectNumber: 7
      projectOrg: 'futurice'
    secrets:
      githubToken: ${{secrets.AUTOMATION_GH_PAT}}
```
1. Open/Reopen an issue & it should be added to the target project.
