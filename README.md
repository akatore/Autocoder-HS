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
name: 'AutoCoder'

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
        uses: akatore/Autocoder-HS@v1 # <-- CHANGE THIS
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          openai_api_key: ${{ secrets.OPENAI_API_KEY }}
          issue_label: 'autocoder-bot'

```
---

### **3. How to Test Your Action**

Before you publish, you should test it.

1.  **Delete Your Old Workflow:**
    Delete your old workflow file (e.g., `.github/workflows/autocoder.yml` or `.github/workflows/main.yml`). Its logic is now inside `action.yml`.

2.  **Create a New Test Workflow:**
    Create a new file named `.github/workflows/test-action.yml`. This file will *use* your new action, just like a customer would.

    **`.github/workflows/test-action.yml`**
    ```yaml
    name: 'Test Local Action'

    on:
      issues:
        types: [labeled]
    
    jobs:
      test-autocoder:
        # Run only if the label added is 'autocoder-bot'
        if: github.event.label.name == 'autocoder-bot'
        runs-on: ubuntu-latest
        
        # Grant permissions
        permissions:
          contents: write
          pull-requests: write
    
        steps:
          # Step 1: Check out the code
          - name: Checkout Repository
            uses: actions/checkout@v4
    
          # Step 2: Run the local action
          # 'uses: ./' tells GitHub to use the action.yml
          # in the root of THIS repository.
          - name: Run Local Autocoder Action
            uses: ./ 
            with:
              github_token: ${{ secrets.GITHUB_TOKEN }}
              openai_api_key: ${{ secrets.OPENAI_API_KEY }}
              issue_label: 'autocoder-bot'
    ```

3.  **Commit and Push:**
    Commit your new `action.yml`, your updated `README.md`, and your new `.github/workflows/test-action.yml`. (Make sure `gpt_interaction_script.sh` is still in the root).

4.  **Trigger the Test:**
    Go to "Issues" and **create a new issue with the `autocoder-bot` label**. This will trigger your `test-action.yml` workflow, which in turn will run your `action.yml` and create the pull request.

5.  **Publish (Final Step):**
    Once you see it working, you can **create a release** on GitHub (e.g., `v1.0.0`). This "publishes" your action so others can use it by referencing the tag (like `uses: Your-Username/Your-Repo-Name@v1.0.0`).


