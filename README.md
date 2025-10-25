# Autocoder Bot - GitHub Action

This action automates generating code from GitHub issues using OpenAI's API and creates a pull request for review.

## Inputs

| Input | Description | Required | Default |
| --- | --- | --- | --- |
| `github_token` | The `GITHUB_TOKEN` to authenticate against the GitHub API. | `true` | N/A |
| `openai_api_key` | Your OpenAI API key for code generation. | `true` | N/A |
| `issue_label` | The label to add to the created pull request. | `false` | `'autocoder-bot'` |

## Outputs

| Output | Description |
| --- | --- |
| `pull_request_url` | The URL of the newly created pull request. |

## Example Usage

To use this action, create a workflow file (e.g., `.github/workflows/autocoder.yml`) in your repository:

```yaml
name: 'Run Autocoder Bot'

on:
  issues:
    types: [labeled]

jobs:
  autocode:
    # Run only if the label added is 'autocoder-bot'
    if: github.event.label.name == 'autocoder-bot'
    runs-on: ubuntu-latest
    
    # Grant permissions for the PR creation
    permissions:
      contents: write
      pull-requests: write

    steps:
      # You must check out your repository's code first
      - name: Checkout Repository
        uses: actions/checkout@v4

      # Run the Autocoder Bot action
      - name: Run Autocoder Bot
        uses: YOUR-USERNAME/YOUR-REPOSITORY-NAME@v1 # <-- CHANGE THIS
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          openai_api_key: ${{ secrets.OPENAI_API_KEY }}
          issue_label: 'autocoder-bot'
