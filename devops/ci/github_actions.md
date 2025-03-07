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

## Workflows

A workflow is an automation process in your repo. This is done by adding an `action.yml`

## Runners

A runner is a server that has the Github Actions runner application installed. There are two types of runners: those that are hosted by Github, and those that are self-hosted. Github hosted runners are triggered by using `runs-on: {os-version}` (e.g. ubuntu-latest). Self-hosted runners need to apply a self-hosted label, along with os and system architecture. Github runners are easier to get going, but less configurable, so may not be as desirable for resource intensive jobs. Self-hosted runners also have the benefit of you providing the OS architecture you want to work with instead of the limited options provided by Github. Note though that not all OSes are supported, you can find a list [here](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners#usage-limits).
