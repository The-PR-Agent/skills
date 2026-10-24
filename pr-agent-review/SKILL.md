---
name: pr-agent-review
description: "Use when reviewing a GitHub pull request with PR-Agent. Choose the appropriate PR-Agent command, preserve repository configuration, and report results without claiming checks that were not run."
version: 1.0.0
author: The-PR-Agent community
license: MIT
---

# PR-Agent review

Use this skill when a user asks an AI agent to review, describe, improve, or suggest changes for a GitHub pull request with [PR-Agent](https://github.com/The-PR-Agent/pr-agent).

## Before running a command

1. Confirm the repository and pull-request number or URL.
2. Confirm the user wants an analysis only or wants comments posted to GitHub.
3. Check that the repository's PR-Agent configuration is present and does not expose secrets.
4. Check whether the relevant PR-Agent GitHub Action is installed. If it is not, explain the setup path instead of inventing a result.
5. Never print or commit API keys, tokens, passwords, or webhook secrets.

## Choose a command

- `/review` — review the pull request and identify bugs, risks, and maintainability concerns.
- `/describe` — generate or update the pull-request description.
- `/improve` — propose code improvements; use only when the user requests improvement suggestions.
- `/ask <question>` — ask a focused question about the pull request.
- `/similar_issue` — look for related issues when repository search is configured.

Prefer `/review` for a general review. Use `/ask` when the user has a specific concern. Do not run multiple commands by default because each command may create a GitHub comment and consume model/API budget.

## GitHub Action setup

A minimal workflow uses the PR-Agent action and passes the command through the pull-request comment trigger:

```yaml
name: PR-Agent

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  pull_request:
    types: [opened, synchronize]

jobs:
  pr-agent:
    if: ${{ github.event.sender.type != 'Bot' }}
    runs-on: ubuntu-latest
    name: Run PR-Agent
    steps:
      - uses: Codium-ai/pr-agent@main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          OPENAI_KEY: ${{ secrets.OPENAI_KEY }}
```

Use the action and provider configuration documented by the current PR-Agent release. Pin the action to a reviewed tag or commit when the repository's supply-chain policy requires it; do not blindly copy `main` into a production workflow.

## Configuration guidance

Keep provider credentials in GitHub Actions secrets or environment variables. Keep non-secret behavior in `.pr_agent.toml` or the repository's documented configuration file. Start with the smallest configuration that meets the review need, then add settings deliberately.

If the repository uses a self-hosted PR-Agent service, follow that deployment's authentication and webhook documentation instead of combining it with the GitHub Action workflow.

## Reporting results

- Distinguish findings from questions and suggestions.
- Include file paths and line references when PR-Agent provides them.
- Say explicitly when a command was not run, failed, or returned no findings.
- Do not claim that a PR is safe, CI is green, or a comment was published without verifying the corresponding GitHub state.
- Ask before publishing comments when the user requested a local or draft review.
