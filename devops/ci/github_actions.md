# Github Actions

Summary from [here](https://learn.microsoft.com/en-us/training/modules/github-actions-automate-tasks/2-github-actions-automate-development-tasks)

Github Actions helps automake deployments and workflows.

## Best Practices
- Review an `action.yml` before running it, it is possible for an open source contributor to provide malicious code
- Include a version of the action your using via git ref, sha, or tag.

## Sample Github Actions
[Link](https://github.com/actions)

## Types of Github Actions

Three types

1. Container Actions: Run in a Linux environment

2. JavaScript Actions: Require an environment specification for the code, run in a VM either on-premise or with a cloud provider. Supports Linux, Mac, and Windows.

3. Composite Actions: Combines multiple workflow steps within one action.

## Runners

A runner is a server that has the Github Actions runner application installed. There are two types of runners: those that are hosted by Github, and those that are self-hosted. Github hosted runners are triggered by using `runs-on: {os-version}` (e.g. ubuntu-latest). Self-hosted runners need to apply a self-hosted label, along with os and system architecture. Github runners are easier to get going, but less configurable, so may not be as desirable for resource intensive jobs. Self-hosted runners also have the benefit of you providing the OS architecture you want to work with instead of the limited options provided by Github. Note though that not all OSes are supported, you can find a list [here](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners#usage-limits).

## Components

Workflow -> Job -> Step -> Action

### Workflows

A workflow is an automation process in your repo. It must have at least one job. The entire workflow is defined by the yaml config.

### Jobs

A job is a component of a workflow that is associated with a runner, and is run on either the machine or in a container. As mentioned in the Runners section, the runner is specified using the `runs-on` attribute.

### Steps

Steps are individual tasks that run commands in a job. A generic action might checkout an action from the Github Actions repo like `actions/checkout@v2` using the `uses` attribute.

### Actions

Actions are standalone commands that are executed. They might run containers or commands on a runner, e.g. `npm i -g express`

## Configuration

Workflows can be configured to run based on various types of Github events (pull requests, push to branch, issue created, etc.), events outside of Github, and on a scheduled time managed via cron. When using cron triggers the shortest schedule available is every 5 minutes.

### Manual Events

Workflows can be manually triggered using the `workflow_dispatch` event attribute. Another way to manually trigger events is using the Github API webhook event called `repository_dispatch`.

### Webhook Events

Webhooks allow for events outside of Github. When webhooks have multiple activities, you can specify an array of valid activity types to trigger the event.

### Conditional Triggers

Your workflow file can access context information that you can use with the `if` conditional attribute to check if a particular step should be triggered, e.g. `if: ${{github.ref == 'refs/heads/main'}}`. Although the `${{ <expression> }}` can be omitted in some cases, it is typically used with many conditional expressions.
