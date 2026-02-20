# claude-issue

Composite action that runs Claude Code against a GitHub issue and opens (or updates) a pull request with the result.

> **Requires a devcontainer.** Claude runs inside the project's devcontainer via [`devcontainers/ci`](https://github.com/devcontainers/ci). The repository must have a valid `devcontainer.json` and `claude` must be available inside the container.

## What it does

Given a GitHub issue, runs Claude Code inside the devcontainer and opens (or updates) a pull request with the resulting changes. Subsequent triggers on the same issue will build on the existing branch rather than starting fresh.

## Usage

The repository must already be checked out before calling this action.

```yaml
- uses: actions/checkout@v6

- uses: ./.github/actions/claude-issue
  with:
    issue_number: ${{ github.event.issue.number }}
    claude_token: ${{ secrets.CLAUDE_CODE_OAUTH_TOKEN }}
    github_token: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Input          | Required | Default                           | Description                                                                      |
| -------------- | -------- | --------------------------------- | -------------------------------------------------------------------------------- |
| `issue_number` | yes      |                                   | GitHub issue number                                                              |
| `base_branch`  | no       | `main`                            | Branch to open the PR against                                                    |
| `claude_token` | yes      |                                   | Claude Code OAuth token (`CLAUDE_CODE_OAUTH_TOKEN`)                              |
| `github_token` | yes      |                                   | GitHub token with `contents: write`, `pull-requests: write`, and `issues: write` |
| `devcontainer` | no       | `.devcontainer/devcontainer.json` | Path to the devcontainer config                                                  |

## Required permissions

The calling job must grant:

```yaml
permissions:
  contents: write       # push the feature branch
  pull-requests: write  # create / update the PR
  issues: write         # read issue data and post reactions
```

## Retry behaviour

If a `claude/issue-<N>` branch already exists on the remote, the action checks it out and includes a retry notice in the prompt, instructing Claude to review the previous attempt before iterating.
