---
layout: training-module
title: "Module 5, Section 3: Guided Walkthrough"
description: "Step-by-step walkthroughs for building GitHub Actions workflows"
permalink: /intermediate/module-5/walkthrough/
module_number: 5
section_number: 3
phase: intermediate
prev_section:
  url: /intermediate/module-5/concepts/
  title: "Core Concepts"
next_section:
  url: /intermediate/module-5/labs/
  title: "Hands-On Labs"
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

## Getting Started with GitHub Actions Workflows

Welcome to the hands-on walkthrough for GitHub Actions! This section takes you through building real CI/CD workflows step-by-step, explaining the reasoning behind each configuration choice.

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>
GitHub Actions is often the "aha moment" for customers. Walking through these examples during enablement sessions helps teams see how Actions fits into their existing development workflow. Focus on the progressive complexity—start simple, then show the power of matrices and reusable workflows.
</div>

**What you'll build:**
- A complete CI pipeline with linting, testing, and building
- Matrix testing across multiple platforms and versions  
- Reusable workflows for organizational standardization
- Composite actions for custom automation
- Conditional workflows and advanced patterns

**Documentation Reference:**
- [Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions)
- [Using workflows](https://docs.github.com/en/actions/using-workflows)
- [Actions marketplace](https://github.com/marketplace?type=actions)

---

## Walkthrough 1: Your First CI Pipeline

A CI (Continuous Integration) pipeline automatically validates code changes through linting, testing, and building. This ensures every commit meets quality standards before merging.

### Why This Pipeline Structure?

This example demonstrates a **parallel job architecture** where lint and test run simultaneously, with the build job waiting for both to succeed. This pattern:
- Provides fast feedback on failures
- Uses runner resources efficiently
- Creates clear visual progress in the Actions UI

Let's create a complete CI pipeline for a Node.js project.

### Step 1: Create the Workflow File
Create `.github/workflows/ci.yml`:

```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  lint:
    name: Lint Code
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
        
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Run linter
        run: npm run lint
        
  test:
    name: Run Tests
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
          
      - run: npm ci
      
      - name: Run tests with coverage
        run: npm test -- --coverage
        
      - name: Upload coverage report
        uses: actions/upload-artifact@v4
        with:
          name: coverage-report
          path: coverage/
          
  build:
    name: Build Application
    needs: [lint, test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
          
      - run: npm ci
      
      - name: Build
        run: npm run build
        
      - name: Upload build artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/

```

### Step 2: Commit and Observe

```bash
git add .github/workflows/ci.yml
git commit -m "Add CI workflow"
git push origin main

```

### Step 3: View Results
1. Go to repository → **Actions** tab
2. Click on the workflow run
3. Observe jobs running in parallel
4. Click into each job to see step logs

<div class="callout callout-info" markdown="1">
<div class="callout-title">📖 Understanding the Workflow</div>
Notice how the <code>build</code> job uses <code>needs: [lint, test]</code>. This creates a dependency graph where build only runs after both lint and test succeed. The Actions UI visualizes this as a workflow diagram, making it easy to see where failures occur.
</div>

---

## Walkthrough 2: Matrix Testing

Matrix builds let you test your code across multiple combinations of operating systems, language versions, or other variables—all from a single workflow definition. This is essential for libraries and tools that need to work across diverse environments.

### When to Use Matrix Testing

| Scenario | Matrix Dimensions |
|----------|-------------------|
| **Public libraries** | OS + language versions customers might use |
| **Internal tools** | OS versions matching your fleet |
| **Web applications** | Browser versions + Node versions |
| **Microservices** | Runtime versions + database versions |

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>
Matrix testing is a powerful selling point for GitHub Actions. Show customers how a single workflow can replace complex Jenkins pipeline matrices or manually maintained test configurations. The <code>fail-fast: false</code> option lets all combinations complete even if one fails—great for understanding the full compatibility picture.
</div>

**Documentation:** [Using a matrix for your jobs](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs)

Create a cross-platform, multi-version test workflow.

```yaml
name: Cross-Platform Tests
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  test:
    name: Test on ${{ matrix.os }} with Node ${{ matrix.node }}
    runs-on: ${{ matrix.os }}
    
    strategy:
      fail-fast: false
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node: [18, 20, 22]
        
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js ${{ matrix.node }}
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
          cache: 'npm'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Run tests
        run: npm test
        
      - name: Upload test results
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: test-results-${{ matrix.os }}-node${{ matrix.node }}
          path: test-results/

```

---

## Walkthrough 3: Reusable Workflows

Reusable workflows let you define a workflow once and call it from multiple other workflows. This is the **key to organizational standardization**—define your CI/CD patterns centrally, then have all teams use them consistently.

### Why Reusable Workflows Matter

Organizations often struggle with:
- Inconsistent CI/CD patterns across teams
- Duplicated workflow code that drifts over time
- No central place to enforce best practices

Reusable workflows solve this by creating a **"workflow library"** that teams can call with different parameters.

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>
Reusable workflows are a major governance win for enterprise customers. Help them identify 3-5 common patterns (build, deploy, security scan) that can be standardized. A central DevOps or Platform Engineering team can own these workflows, while product teams simply call them.
</div>

**Documentation:** [Reusing workflows](https://docs.github.com/en/actions/using-workflows/reusing-workflows)

### Step 1: Create the Reusable Workflow
`.github/workflows/reusable-build.yml`:

```yaml
name: Reusable Build
on:
  workflow_call:
    inputs:
      node-version:
        description: 'Node.js version'
        required: false
        default: '20'
        type: string
      artifact-name:
        description: 'Name for build artifact'
        required: false
        default: 'build'
        type: string
    secrets:
      NPM_TOKEN:
        description: 'NPM authentication token'
        required: false
    outputs:
      artifact-name:
        description: 'Name of uploaded artifact'
        value: ${{ jobs.build.outputs.artifact-name }}
jobs:
  build:
    runs-on: ubuntu-latest
    outputs:
      artifact-name: ${{ inputs.artifact-name }}
      
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node-version }}
          cache: 'npm'
          registry-url: 'https://registry.npmjs.org'
          
      - name: Install dependencies
        run: npm ci
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
          
      - name: Build
        run: npm run build
        
      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: ${{ inputs.artifact-name }}
          path: dist/

```

### Step 2: Call the Reusable Workflow
`.github/workflows/ci.yml`:

```yaml
name: CI
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
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
    needs: test
    uses: ./.github/workflows/reusable-build.yml
    with:
      node-version: '20'
      artifact-name: 'production-build'
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
      
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: production-build
      - run: ./deploy.sh

```

<div class="callout callout-info" markdown="1">
<div class="callout-title">📖 Inputs, Secrets, and Outputs</div>
Reusable workflows support typed inputs (<code>string</code>, <code>boolean</code>, <code>number</code>), inherited secrets, and outputs that can be consumed by downstream jobs. This makes them behave like functions in your CI/CD system.
</div>

---

## Walkthrough 4: Creating a Custom Action

While reusable workflows work at the **workflow level**, composite actions work at the **step level**. They bundle multiple steps into a single reusable action that can be called within any job.

### When to Use Each Approach

| Pattern | Use Case | Scope |
|---------|----------|-------|
| **Composite Action** | Bundle related setup steps | Single step in a job |
| **Reusable Workflow** | Standardize entire workflows | Complete job(s) |
| **JavaScript Action** | Complex logic, API calls | Single step |
| **Docker Action** | Language-specific tools | Single step |

**Documentation:** [Creating a composite action](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action)

### Step 1: Create Action Structure
`.github/actions/setup-project/action.yml`:

```yaml
name: 'Setup Project'
description: 'Setup Node.js project with caching and dependencies'
inputs:
  node-version:
    description: 'Node.js version'
    required: false
    default: '20'
  working-directory:
    description: 'Working directory'
    required: false
    default: '.'
  install-command:
    description: 'Install command'
    required: false
    default: 'npm ci'
outputs:
  cache-hit:
    description: 'Whether cache was hit'
    value: ${{ steps.cache.outputs.cache-hit }}
runs:
  using: 'composite'
  steps:
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ inputs.node-version }}
        
    - name: Get npm cache directory
      id: npm-cache-dir
      shell: bash
      run: echo "dir=$(npm config get cache)" >> $GITHUB_OUTPUT
      
    - name: Cache npm dependencies
      id: cache
      uses: actions/cache@v4
      with:
        path: ${{ steps.npm-cache-dir.outputs.dir }}
        key: npm-${{ runner.os }}-${{ hashFiles('**/package-lock.json') }}
        restore-keys: |
          npm-${{ runner.os }}-
          
    - name: Install dependencies
      shell: bash
      working-directory: ${{ inputs.working-directory }}
      run: ${{ inputs.install-command }}

```

### Step 2: Use the Custom Action

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup project
        uses: ./.github/actions/setup-project
        with:
          node-version: '20'
          
      - run: npm run build

```

---

## Walkthrough 5: Environment Deployments

GitHub Environments add **deployment gates and protection rules** to your workflows. They integrate approvals, wait timers, and branch restrictions directly into Actions—no external tools needed.

### Why Use Environments?

Environments solve common deployment governance needs:
- **Required approvals** before deploying to production
- **Wait timers** for staged rollouts
- **Branch restrictions** ensuring only `main` can deploy
- **Environment-specific secrets** for credentials

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>
Environments are a key differentiator from basic CI tools. Show customers the deployment history view—it provides audit trails showing who approved what and when. This is often a compliance requirement that previously required external tools like ServiceNow or Jira.
</div>

**Documentation:** [Using environments for deployment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)

### Step 1: Create Environments
1. Go to repository **Settings** → **Environments**
2. Click **New environment**
3. Create `staging` and `production` environments

### Step 2: Configure Production Protection
1. Click on `production` environment
2. Add protection rules:
   - ☑ Required reviewers: Select approvers
   - ☑ Wait timer: 10 minutes
   - ☑ Restrict deployment branches: `main` only

### Step 3: Create Deployment Workflow

```yaml
name: Deploy
on:
  push:
    branches: [main]
  workflow_dispatch:
    inputs:
      environment:
        description: 'Environment to deploy to'
        required: true
        type: environment
jobs:
  build:
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
  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    environment: staging
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build
          
      - name: Deploy to Staging
        run: |
          echo "Deploying to staging..."
          # Add actual deployment commands
          
  deploy-production:
    needs: deploy-staging
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build
          
      - name: Deploy to Production
        run: |
          echo "Deploying to production..."
          # Add actual deployment commands

```

---

## Summary

You've now walked through the core GitHub Actions patterns that form the foundation of modern CI/CD:

| Pattern | Key Takeaway |
|---------|--------------|
| **CI Pipeline** | Parallel jobs with dependencies create fast feedback loops |
| **Matrix Testing** | Test across combinations without duplicating workflows |
| **Reusable Workflows** | Standardize patterns across the organization |
| **Composite Actions** | Bundle related steps for cleaner workflow files |
| **Environments** | Add governance and approval gates to deployments |

<div class="callout callout-success" markdown="1">
<div class="callout-title">✅ Ready for Labs</div>
You've seen how these concepts work in practice. In the Hands-On Labs, you'll build these workflows yourself and see them execute in real time.
</div>

**Next Steps:**
- Try the [Hands-On Labs](/intermediate/module-5/labs/) to implement these patterns
- Explore the [GitHub Actions documentation](https://docs.github.com/en/actions)
- Check the [Actions Marketplace](https://github.com/marketplace?type=actions) for pre-built actions

{% endraw %}
