---
layout: training-module
title: "Module 5, Section 5: Knowledge Check"
description: "Test your understanding of GitHub Actions concepts"
permalink: /intermediate/module-5/quiz/
module_number: 5
section_number: 5
phase: intermediate
prev_section:
  url: /intermediate/module-5/labs/
  title: "Hands-On Labs"
next_section:
  url: /intermediate/module-5/resources/
  title: "Resources"
toc: true
module_title: "GitHub Actions & Automation"
total_sections: 7
module_index: /intermediate/module-5/
sections:
  - title: "Context & Overview"
    url: "/intermediate/module-5/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "20 min"
  - title: "Core Concepts"
    url: "/intermediate/module-5/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/intermediate/module-5/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "50 min"
  - title: "Hands-On Labs"
    url: "/intermediate/module-5/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/intermediate/module-5/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "25 min"
  - title: "Resources"
    url: "/intermediate/module-5/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/intermediate/module-5/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "30 min"
---
{% raw %}
Test your understanding of GitHub Actions concepts.
---

## Questions

### Question 1
What's the difference between `uses` and `run` in a workflow step?
<details markdown="1">
<summary>View Answer</summary>
**`uses`** - Executes a reusable action:

```yaml
- uses: actions/checkout@v4        # Run the checkout action
- uses: ./.github/actions/custom   # Run local action
- uses: docker://alpine:3.18       # Run Docker container

```

**`run`** - Executes shell commands:

```yaml
- run: npm install                 # Single command
- run: |                           # Multi-line script
    npm install
    npm test

```

Key differences:
- `uses` is for pre-built, reusable functionality
- `run` is for custom shell commands
- `uses` requires a reference (action path, Docker image)
- `run` executes in the runner's shell (bash, PowerShell)
</details>

### Question 2
A workflow is running on every push, but you only want it to run when files in the `src/` directory change. How do you configure this?
<details markdown="1">
<summary>View Answer</summary>
Use path filters in the trigger:

```yaml
on:
  push:
    branches: [main]
    paths:
      - 'src/**'              # Run when any file in src/ changes
      - '!src/**/*.md'        # Except markdown files
      
  pull_request:
    branches: [main]
    paths:
      - 'src/**'

```

You can also use `paths-ignore` for inverse logic:

```yaml
on:
  push:
    paths-ignore:
      - 'docs/**'
      - '*.md'
      - '.github/**'

```

</details>
---

### Question 3
How do you pass data between jobs in a workflow?
<details markdown="1">
<summary>View Answer</summary>
Two main approaches:
**1. Job outputs (for small data):**

```yaml
jobs:
  job1:
    runs-on: ubuntu-latest
    outputs:
      version: ${{ steps.get_version.outputs.version }}
    steps:
      - id: get_version
        run: echo "version=1.2.3" >> $GITHUB_OUTPUT
        
  job2:
    needs: job1
    runs-on: ubuntu-latest
    steps:
      - run: echo "Version is ${{ needs.job1.outputs.version }}"

```

**2. Artifacts (for files/large data):**

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/
          
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build
          path: dist/

```

Key differences:
- Outputs: Small strings, immediate access
- Artifacts: Files, stored 90 days by default, downloadable from UI
</details>

### Question 4
What is the purpose of `workflow_call` trigger and how does it differ from `workflow_dispatch`?
<details markdown="1">
<summary>View Answer</summary>
**`workflow_dispatch`** - Manual trigger from UI/API:

```yaml
on:
  workflow_dispatch:
    inputs:
      environment:
        type: choice
        options: [dev, prod]

```

- User clicks "Run workflow" button
- Can provide inputs via UI
- Runs independently
**`workflow_call`** - Called by another workflow:

```yaml
on:
  workflow_call:
    inputs:
      environment:
        type: string
        required: true
    secrets:
      API_KEY:
        required: true

```

- Cannot be triggered manually
- Must be called using `uses`:

```yaml
jobs:
  deploy:
    uses: ./.github/workflows/reusable-deploy.yml
    with:
      environment: production
    secrets:
      API_KEY: ${{ secrets.API_KEY }}

```

- Enables DRY workflow design
- Supports inputs, outputs, and secrets
</details>
---

### Question 5
How do you ensure a workflow step runs even if previous steps failed?
<details markdown="1">
<summary>View Answer</summary>
Use conditional expressions:

```yaml
steps:
  - run: npm test
  
  - name: Upload test results
    if: always()                    # Always run
    uses: actions/upload-artifact@v4
    with:
      name: test-results
      path: test-results/
      
  - name: Notify on failure
    if: failure()                   # Only if previous failed
    run: ./notify-team.sh
    
  - name: Cleanup
    if: cancelled()                 # Only if workflow cancelled
    run: ./cleanup.sh

```

Available status functions:
- `always()` - Always run regardless of status
- `success()` - Run only if all previous steps succeeded (default)
- `failure()` - Run only if any previous step failed
- `cancelled()` - Run only if workflow was cancelled
Can combine with other conditions:

```yaml
if: always() && github.ref == 'refs/heads/main'

```

</details>

### Question 6
What's wrong with this workflow? How would you fix it?

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - run: npm test
      
  build:
    runs-on: ubuntu-latest
    steps:
      - run: npm run build
      
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: ./deploy.sh

```

<details markdown="1">
<summary>View Answer</summary>
**Issues:**
1. **No checkout step** - Jobs don't have repository code
2. **No dependency installation** - npm commands will fail
3. **Test and build run in parallel** - Build might fail before tests complete
4. **Deploy doesn't wait for tests** - Could deploy untested code
**Fixed version:**

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm test
      
  build:
    needs: test                     # Wait for tests
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/
      
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build
      - run: ./deploy.sh

```

</details>
---

### Question 7
How do you securely authenticate to AWS from GitHub Actions without storing long-lived credentials?
<details markdown="1">
<summary>View Answer</summary>
Use OIDC (OpenID Connect) authentication:
**1. Configure AWS IAM:**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Federated": "arn:aws:iam::123456789:oidc-provider/token.actions.githubusercontent.com"
      },
      "Action": "sts:AssumeRoleWithWebIdentity",
      "Condition": {
        "StringEquals": {
          "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
        },
        "StringLike": {
          "token.actions.githubusercontent.com:sub": "repo:owner/repo:*"
        }
      }
    }
  ]
}

```

**2. Configure workflow:**

```yaml
permissions:
  id-token: write                  # Required for OIDC
  contents: read
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789:role/github-actions
          aws-region: us-east-1
          
      - run: aws s3 ls              # Authenticated!

```

Benefits:
- No long-lived secrets stored in GitHub
- Token is short-lived and scoped
- Can be scoped to specific repos/branches
- Works with AWS, Azure, GCP, HashiCorp Vault
</details>
{% endraw %}
