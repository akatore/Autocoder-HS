Use GitHub Actions to interact with the GitHub API to read issues and comments. Accessing issues is made even simpler with the github.event variable in GitHub Actions. This environment variable provides payload information about the event that triggered the workflow. Use this variable to access issue details such as title, body, assignees, and labels and echo them back in the workflow logs. These environment variables include:

Issue Number: ${{ github.event.issue.number }}
Issue Body: ${{ github.event.issue.body }}
Issue Assignees: ${{ join(github.event.issue.assignees.*.login, ', ') }}
Issue Labels: ${{ join(github.event.issue.labels.*.name, ', ') }}

These variables can also be used when creating the URLs needed to interact with the GitHub API. For example, to retrieve the issues, you can use the following commands in your workflow:

ISSUE_NUMBER=${{ github.event.issue.number }}
ISSUE_RESPONSE=$(curl -s https://api.github.com/repos/${{ github.repository }}/issues/$ISSUE_NUMBER)
echo "Issue Information: $ISSUE_RESPONSE"

Objectives
To pass this stage, perform the following:

Modify the workflow to trigger when a new issue opened and when a closed issue is reopened;

Checkout the repository using the appropriate action;

Set and use environment variables to read the content of the issue that triggered the workflow taking advantage of the github.event variable to access issue details such as number, title, body, assignees, and labels as shown in the example above;

Echo the content back in the workflow logs - ensure to use distinct echo commands for each issue attribute;

Examples
Example 1:
```yaml
name: Access Issue Information
jobs:
  access_issue_info:
    runs-on: ubuntu-latest

    steps:
      - name: Print Issue Information
        env:
          ISSUE_NUMBER: ${{ github.event.issue.number }}
          # other environment variables
```
Example 2:
```yaml
name: Access Issue Information
jobs:
  access_issue_info:
    runs-on: ubuntu-latest

    steps:
      - name: Print Issue Information
        run: |
            echo "Issue Number: ${ISSUE_NUMBER}"
```
---

## 🧭 What Is a GitHub “Issue”?

A **GitHub Issue** is like a discussion thread or task card in your repository.
You use issues to:

* Report bugs 🐛
* Request new features ✨
* Track tasks or documentation updates 📋

Each issue has:

* A **title**
* A **body** (description)
* Optional **labels** (like `bug`, `enhancement`)
* Optional **assignees** (people responsible)
* A **status**: Open 🔓 or Closed 🔒

When someone **opens** or **reopens** an issue, that triggers a GitHub **event** — and your **GitHub Actions workflow** can listen for that event and run automatically.

---

## ⚙️ Why You Need to Open an Issue for This Test

Your workflow runs only when an issue event happens — specifically:

```yaml
on:
  issues:
    types: [opened, reopened]
```

So to test it, you must actually **open** or **reopen** an issue in your repository.
That’s what triggers the `github.event.issue` variables (like `${{ github.event.issue.title }}`) to contain real data.

If you don’t open an issue, the workflow won’t trigger → and you’ll see the error like:

> “No open issues found in the repository.”

---

## 🪜 Step-by-Step: How to Open an Issue Manually

1. Go to your **GitHub repository page** (e.g. `https://github.com/akatore/your-repo-name`).

2. Click on the **“Issues”** tab near the top:

   ```
   Code | Issues | Pull requests | Actions | ...
   ```

3. Click the green **“New issue”** button.

4. Fill in:

   * **Title:** something like `Test issue for GitHub Actions`
   * **Description (body):** anything you want, e.g. `This issue is just for testing the workflow.`

5. (Optional) Add **labels** (like `bug`) or **assign** yourself.

6. Click **“Submit new issue”** ✅

7. After you create it, your workflow will **automatically start running** under the **Actions** tab.

---

## 🧩 Bonus: To Test “Reopened” Event

If you want to test the second trigger (`reopened`):

1. Open your issue → click **“Close issue”**.
2. Then click **“Reopen issue”**.

That will trigger the workflow again with the same event payload — now under the `reopened` type.

---

## 🕵️ Check If It Worked

1. Go to your repository’s **Actions** tab.
2. You’ll see a workflow named **“Access Issue Information”** running.
3. Click it → click the latest job → expand the logs.
4. You should see something like:

```
=== Issue Information ===
Issue Number: 1
Issue Title: Test issue for GitHub Actions
Issue Body: This issue is just for testing the workflow.
Issue Assignees:
Issue Labels:
Repository: akatore/your-repo-name
```

That means everything worked perfectly ✅

---

Would you like me to show you how to **auto-comment back on the issue** (e.g., “Thanks for opening this issue!”) once your workflow runs?
