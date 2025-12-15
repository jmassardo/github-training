---
layout: training-module
title: "Module 8, Section 5: Knowledge Check"
description: "Test your understanding of CI/CD pipelines and deployment concepts"
permalink: /intermediate/module-8/quiz/
module_number: 8
section_number: 5
phase: intermediate
prev_section:
  url: /intermediate/module-8/labs/
  title: "Hands-On Labs"
next_section:
  url: /intermediate/module-8/resources/
  title: "Resources"
toc: true
module_title: "CI/CD Pipelines & Deployment"
total_sections: 7
module_index: /intermediate/module-8/
sections:
  - title: "Context & Overview"
    url: "/intermediate/module-8/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "25 min"
  - title: "Core Concepts"
    url: "/intermediate/module-8/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/intermediate/module-8/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "60 min"
  - title: "Hands-On Labs"
    url: "/intermediate/module-8/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/intermediate/module-8/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "30 min"
  - title: "Resources"
    url: "/intermediate/module-8/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/intermediate/module-8/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "35 min"
---
{% raw %}

## Questions
**Question 1:** What's the difference between `workflow_run` and `needs` for workflow/job dependencies?
<details markdown="1">
<summary>View Answer</summary>
**`needs`** - Job dependency within a workflow:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps: [...]
    
  deploy:
    needs: build  # Runs after build job completes
    runs-on: ubuntu-latest
    steps: [...]

```

- Same workflow file
- Shares workflow context (secrets, variables)
- Can access outputs from needed jobs
**`workflow_run`** - Workflow dependency across workflows:

```yaml
on:
  workflow_run:
    workflows: ["CI"]
    branches: [main]
    types: [completed]

```

- Different workflow files
- Separate workflow runs
- Need to explicitly download artifacts
- Can filter by branch and completion status
**Use cases:**

- `needs`: Sequential jobs in same pipeline (build → test → deploy)
- `workflow_run`: Triggered pipelines (CI completes → trigger CD)
</details>
---
**Question 2:** A production deployment failed and you need to rollback immediately. What are your options?
<details markdown="1">
<summary>View Answer</summary>
**Option 1: Kubernetes native rollback**

```bash
kubectl rollout undo deployment/my-app
kubectl rollout status deployment/my-app

```

**Option 2: GitHub Actions rollback workflow**

```yaml
workflow_dispatch:
  inputs:
    version:
      required: true

```

Redeploy a previous known-good version.
**Option 3: Blue-Green swap**

```bash
# If using Azure App Service slots
az webapp deployment slot swap --slot staging --target-slot production
# If using AWS/load balancer
# Switch target group to previous environment

```

**Option 4: GitOps rollback**

```bash
# If using GitOps (Flux, ArgoCD)
git revert <bad-commit>
git push
# GitOps controller auto-deploys previous state

```

**Best practice:**

- Have rollback procedures documented
- Test rollback regularly
- Keep previous 3-5 versions available
- Monitor after rollback to confirm resolution
</details>
**Question 3:** How do you prevent deployments to production on Fridays?
<details markdown="1">
<summary>View Answer</summary>
**Option 1: Workflow condition**

```yaml
deploy-production:
  if: github.event.schedule != 'friday' && 
      (steps.date.outputs.day != 'Friday')
  
  steps:
    - name: Check day
      id: date
      run: echo "day=$(date +%A)" >> $GITHUB_OUTPUT
      
    - name: Block Friday deploys
      if: steps.date.outputs.day == 'Friday'
      run: |
        echo "No production deploys on Fridays!"
        exit 1

```

**Option 2: Custom deployment protection rule**
- Create an external service that returns deploy status
- Configure as custom deployment protection
- Service checks day/time and returns proceed/reject
**Option 3: Branch protection with scheduled windows**
- Require PR approval
- Only approve production PRs Mon-Thu
- Document policy in CODEOWNERS
**Option 4: Environment wait timer**
- Set 72-hour wait timer on Friday evenings
- Effectively blocks until Monday
**Best approach:** Combine technical controls with policy. Document "no Friday deploys" and enforce with workflow checks.
</details>
---
**Question 4:** How do you share secrets between environments without duplicating them?
<details markdown="1">
<summary>View Answer</summary>
**Option 1: Repository secrets (shared)**

```yaml
# Repository secret available to all environments
env:
  COMMON_API_KEY: ${{ secrets.SHARED_API_KEY }}

```

**Option 2: Organization secrets (shared across repos)**
- Organization Settings → Secrets
- Choose repository access policy
- Available in all selected repos/environments
**Option 3: Azure Key Vault / AWS Secrets Manager**

```yaml
- name: Get secrets from Key Vault
  uses: azure/get-keyvault-secrets@v1
  with:
    keyvault: "my-key-vault"
    secrets: 'API-KEY,DB-PASSWORD'
  id: secrets

```

**Option 4: Environment inheritance (not native)**

```yaml
# Custom solution: fetch from common environment
- name: Get shared secrets
  run: |
    # API call to get shared secrets
    # Or use organization variables

```

**Best practice:**

- Use repository secrets for truly shared values
- Use environment secrets for environment-specific
- Use external secret managers for rotation/audit
</details>
**Question 5:** Explain how to implement a deployment approval workflow that requires sign-off from both QA and Security teams.
<details markdown="1">
<summary>View Answer</summary>
**Configuration:**

1. **Create teams:**
   - `@org/qa-team`
   - `@org/security-team`
2. **Configure environment protection:**

```
Environment: production
├── Required reviewers:
│   ├── @org/qa-team (any 1)
│   └── @org/security-team (any 1)
└── Deployment branches: main

```

3. **Workflow configuration:**

```yaml
jobs:
  qa-validation:
    runs-on: ubuntu-latest
    environment: qa-gate  # QA approves here
    steps:
      - run: echo "QA approved"
      
  security-validation:
    runs-on: ubuntu-latest
    environment: security-gate  # Security approves here
    steps:
      - run: echo "Security approved"
      
  deploy:
    needs: [qa-validation, security-validation]
    environment: production
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying..."

```

4. **Alternative: Single environment with multiple reviewers**

```
Environment: production
├── Required reviewers:
│   ├── @org/qa-team (any 1)
│   └── @org/security-team (any 1)
│   Require both teams: Yes

```

**Note:** GitHub natively supports requiring approval from multiple teams when they're all listed as required reviewers.
</details>
{% endraw %}
