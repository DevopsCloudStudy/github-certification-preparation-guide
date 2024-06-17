# GitHub Actions Topics and Key Points for Certification Preparation

## Runner
- **GitHub hosted, self-hosted**

## Actions and Types
- **Custom**
- **Docker**
- **JavaScript**

## Workflow Maintenance

## Variables and Environments

## Artifact and Cache

## Release Management
- **Custom actions**

## Action Runtime Controllers

---

## Artifact
- Artifacts enable you to share data between jobs in a workflow and store data once that workflow has been completed.
- Artifact is used by downstream jobs.
- You can use the REST API to download, delete, and retrieve information about workflow artifacts in GitHub Actions.
- We can perform API operations with artifacts using `curl`, JavaScript, and GitHub CLI. Users need to authorize with a GitHub token.
- Artifacts can be downloaded from the Workflow GUI.
- Once an artifact is deleted, it cannot be recovered.
- GitHub will keep artifacts in public repositories for 90 days by default.
- Artifacts and caching are similar because they provide the ability to store files on GitHub, but each feature offers different use cases and cannot be used interchangeably.
- Use caching when you want to reuse files that don't change often between jobs or workflow runs, such as build dependencies from a package management system.
- When uploading an artifact, you can specify a single file or directory, or multiple files or directories. You can also exclude certain files or directories and use wildcard patterns (`actions/upload-artifact` action).
- You cannot access artifacts that were created in a different workflow run.
- Use artifacts when you want to save files produced by a job to view after a workflow run has ended, such as built binaries or build logs.
- If a workflow run is triggered for a pull request, it can also restore caches created in the base branch, including base branches of forked repositories.
- Operations like GET, LIST, DELETE, and DOWNLOAD can be performed on artifacts using the REST API with the following authentication token methods:
  - GitHub App user access tokens
  - GitHub App installation access tokens
  - Fine-grained personal access tokens

## Workflow
- Workflows triggered using `on: push` or `on: pull_request` won't be triggered if you add any of the following strings to the commit message in a push, or the HEAD commit of a pull request:
  - `[skip ci]`
  - `[ci skip]`
  - `[no ci]`
  - `[skip actions]`
  - `[actions skip]`
- `workflow_dispatch` event: This event allows you to run the workflow by using the GitHub REST API or by selecting the "Run workflow" button in the Actions tab within your repository on GitHub.
- In addition to `workflow_dispatch`, you can use the GitHub API to trigger a webhook event called `repository_dispatch`. This event allows you to trigger a workflow for activity that occurs outside of GitHub and essentially serves as an HTTP request to your repository asking GitHub to trigger a workflow off an action or webhook.
- You can delete a workflow run that has been completed or is more than two weeks old.
- You can disable and re-enable a workflow using the GitHub UI, the REST API, or GitHub CLI. Disabling a workflow allows you to stop a workflow from being triggered without having to delete the file from the repo. You can easily re-enable the workflow again on GitHub.
- Workflow templates are defined in a `.github` repository, enabling you to leverage all the power of GitHub’s collaborative capabilities and providing full auditability. Workflow files need to reside in the `workflow-templates` directory.
- The "contents" permission is responsible for operations with the contents of the repository. For example, `contents: read` permits an action to list the commits, and `contents: write` allows the action to create a release.
- The "deployments" permission is for controlling deployments. For example, `deployments: write` permits an action to create a new deployment. This token is usually only needed when you interact with GitHub functionality and services.
- The "pull-requests" permission is for controlling pull requests. For example, `pull-requests: write` permits an action to add a label to a pull request.
- The "security-events" permission is for controlling GitHub code scanning and Dependabot alerts. For example, `security-events: read` permits an action to list the Dependabot alerts for the repository, and `security-events: write` allows an action to update the status of a code scanning alert.
- The `check_run` event runs your workflow when activity related to a check run occurs. A check run is an individual test that is part of a check suite.
- At the start of each workflow job, GitHub automatically creates a unique `GITHUB_TOKEN` secret to use in your workflow. You can use the `GITHUB_TOKEN` to authenticate in the workflow job.
- Alternatively, you can add a `skip-checks` trailer to your commit message.
- Skip instructions only apply to the workflow run(s) that would be triggered by the commit that contains the skip instructions.
- From within a reusable workflow, you can call another reusable workflow.
- A workflow should have an `on:` keyword with a trigger event and at least one job defined. The workflow name and specific trigger branches are optional.
- **Checkout**: This action checks out your repository under `$GITHUB_WORKSPACE`, so your workflow can access it.

## Actions
- All actions require a metadata file. The metadata filename must be either `action.yml` or `action.yaml`.
- Input, outputs, and author are optional parameters.
- `runs` parameter is used as:
  ```yaml
  runs:
    using: 'node20'
    main: 'main.js'


- There are only exit (0) or exit (non-zero) codes for success and failure for GitHub actions.
- If you are creating a JavaScript action, you can use the actions toolkit @actions/core package to log a message and set a failure exit code. For example:
javascript
      ``Copy code
      try {
        // something
      } catch (error) {
        core.setFailed(error.message);
      }``
- Secrets cannot be directly referenced in if: conditionals. Instead, consider setting secrets as environment variables, then referencing the environment variables to conditionally run steps in the job.
- There is no property such as steps..inputs with step context.

## Runner ##
- Runner groups are used to collect sets of runners and create a security boundary around them. You can then decide which organizations or repositories are permitted to run jobs on those sets of machines. Organization owners can configure access policies that control which repositories in an organization have access to the runner group.
- To enable runner diagnostic logging, set the ACTIONS_RUNNER_DEBUG secret in the repository that contains the workflow to true.
- The runner diagnostic logs are contained in the runner-diagnostic-logs folder. While you can download them, you will need to proactively enable debug logging.
- Not all steps run actions, but all actions run as a step.
- The runner need not be stopped/restarted to assign/remove labels.
- CodeQL: A query language specifically designed for analyzing code. Integrating CodeQL as a workflow step allows you to automate static code analysis, scanning your codebase for potential security vulnerabilities and bugs before pushing code or merging pull requests.
- The name in the action's metadata file must be unique:
- The name cannot match an existing action name published on GitHub Marketplace.
- The name cannot match a user or organization on GitHub unless the user or organization owner is publishing the action. For example, only the GitHub organization can publish an action named github.
- The name cannot match an existing GitHub Marketplace category.
- GitHub reserves the names of GitHub features.
- GitHub-hosted runners are automatically managed by GitHub, including scaling up or down resources based on usage.
- Self-hosted runners require you to provision additional runner machines or adjust resource allocation manually to handle increased workflow execution.
css
