---
layout: training-module
title: "Module 5, Section 2: Core Concepts"
description: "Master GitHub Actions architecture, syntax, and key components"
permalink: /intermediate/module-5/concepts/
module_number: 5
section_number: 2
phase: intermediate
prev_section:
  url: /intermediate/module-5/overview/
  title: "Context & Overview"
next_section:
  url: /intermediate/module-5/walkthrough/
  title: "Guided Walkthrough"
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

## 2.1 GitHub Actions Architecture

<div class="callout callout-info" markdown="1">
<div class="callout-title">📖 Official Documentation</div>
For comprehensive reference, see the <a href="https://docs.github.com/en/actions">GitHub Actions Documentation</a>.
</div>

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph REPO["📦 GitHub Repository"]
        WF[".github/workflows/*.yml"]
    end
    
    REPO -->|"Trigger<br/>(push, PR, schedule)"| SERVICE
    
    subgraph SERVICE["⚡ GitHub Actions Service"]
        ORCH["Workflow Orchestrator"]
    end
    
    SERVICE --> RUNNERS
    
    subgraph RUNNERS["🖥️ Runners"]
        J1["🔧 Job 1"]
        J2["🔧 Job 2"]
        J3["🔧 Job 3"]
    end
    
    J1 --> S1["Step → Step → Step"]
    J2 --> S2["Step → Step → Step"]
    J3 --> S3["Step → Step → Step"]
</div>
</div>

---

## 2.2 Core Components

### Workflows
The top-level automation definition:

```yaml
# .github/workflows/ci.yml
name: CI Pipeline           # Display name
on:                         # Triggers
  push:
    branches: [main]
  pull_request:
    branches: [main]
jobs:                       # Jobs to execute
  build:
    # ... job definition

```

### Events (Triggers)

| Event | Description | Common Use |
|-------|-------------|------------|
| `push` | Code pushed to branch | CI on main |
| `pull_request` | PR opened/updated | PR validation |
| `schedule` | Cron schedule | Nightly builds |
| `workflow_dispatch` | Manual trigger | On-demand runs |
| `release` | Release created | Publish artifacts |
| `issues` | Issue activity | Triage automation |
| `workflow_call` | Called by another workflow | Reusable workflows |

```yaml
on:
  push:
    branches: [main, develop]
    paths:
      - 'src/**'
      - '!src/**/*.md'      # Exclude markdown
    tags:
      - 'v*.*.*'
      
  pull_request:
    types: [opened, synchronize, reopened]
    branches: [main]
    
  schedule:
    - cron: '0 2 * * *'     # 2 AM UTC daily
    
  workflow_dispatch:
    inputs:
      environment:
        description: 'Target environment'
        required: true
        default: 'staging'
        type: choice
        options:
          - staging
          - production

```

### Jobs
Jobs run in parallel by default, on separate runners:

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run lint
      
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm test
      
  build:
    needs: [lint, test]     # Wait for lint and test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run build

```

Job dependency visualization:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    lint["lint"] --> build["build"]
    test["test"] --> build
</div>
</div>

### Steps
Steps run sequentially within a job:

```yaml
steps:
  # Action step (uses)
  - name: Checkout code
    uses: actions/checkout@v4
    with:
      fetch-depth: 0
      
  # Shell command step (run)
  - name: Install dependencies
    run: npm ci
    
  # Multi-line command
  - name: Build and test
    run: |
      npm run build
      npm test
      
  # Conditional step
  - name: Deploy
    if: github.ref == 'refs/heads/main'
    run: ./deploy.sh

```

### Runners
Where workflows execute:

| Runner | OS | Specs | Use Case |
|--------|-------|-------|----------|
| `ubuntu-latest` | Ubuntu 22.04 | 2 CPU, 7GB RAM | Most workflows |
| `ubuntu-24.04` | Ubuntu 24.04 | 2 CPU, 7GB RAM | Specific version |
| `windows-latest` | Windows Server 2022 | 2 CPU, 7GB RAM | Windows builds |
| `macos-latest` | macOS 14 (Sonoma) | 3 CPU, 14GB RAM | iOS/macOS builds |
| `self-hosted` | Custom | Custom | Private networks, special hardware |

## 2.3 Workflow Syntax Deep Dive

### Environment Variables
{% raw %}

```yaml
env:                              # Workflow-level
  NODE_ENV: production
  
jobs:
  build:
    env:                          # Job-level
      CI: true
    steps:
      - name: Build
        env:                      # Step-level
          API_KEY: ${{ secrets.API_KEY }}
        run: npm run build
        
      # Accessing variables
      - run: echo "Node env is $NODE_ENV"
      - run: echo "Event is ${{ github.event_name }}"

```

{% endraw %}

### Contexts
Access information about the workflow run:
{% raw %}

```yaml
# github context
${{ github.repository }}          # owner/repo
${{ github.ref }}                 # refs/heads/main
${{ github.sha }}                 # commit SHA
${{ github.actor }}               # user who triggered
${{ github.event_name }}          # push, pull_request, etc.
${{ github.run_number }}          # incrementing run number
${{ github.workflow }}            # workflow name
# secrets context
${{ secrets.GITHUB_TOKEN }}       # auto-generated token
${{ secrets.MY_SECRET }}          # custom secret
# env context
${{ env.MY_VAR }}
# job context
${{ job.status }}                 # success, failure, cancelled
# steps context
${{ steps.step_id.outputs.name }} # step output
${{ steps.step_id.outcome }}      # success, failure, skipped
# runner context
${{ runner.os }}                  # Linux, Windows, macOS
${{ runner.arch }}                # X64, ARM64
${{ runner.temp }}                # temp directory path

```

{% endraw %}

### Expressions and Functions
{% raw %}

```yaml
# Conditionals
if: ${{ github.event_name == 'push' }}
if: github.ref == 'refs/heads/main'
if: contains(github.event.head_commit.message, '[skip ci]')
if: startsWith(github.ref, 'refs/tags/')
if: always()                      # Run even if previous failed
if: failure()                     # Run only if previous failed
if: cancelled()                   # Run only if cancelled
# Boolean operators
if: github.event_name == 'push' && github.ref == 'refs/heads/main'
if: github.event_name == 'pull_request' || github.event_name == 'push'
if: "!startsWith(github.ref, 'refs/tags/')"
# Functions
${{ toJSON(github.event) }}       # Convert to JSON
${{ fromJSON(steps.data.outputs.json) }}
${{ format('Hello {0}', github.actor) }}
${{ join(matrix.os, ', ') }}

```

{% endraw %}

### Matrix Strategies
Test across multiple configurations:
{% raw %}

```yaml
jobs:
  test:
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node: [18, 20, 22]
        exclude:
          - os: macos-latest
            node: 18
        include:
          - os: ubuntu-latest
            node: 22
            experimental: true
      fail-fast: false            # Continue other jobs if one fails
      max-parallel: 4             # Limit concurrent jobs
      
    runs-on: ${{ matrix.os }}
    continue-on-error: ${{ matrix.experimental || false }}
    
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
      - run: npm test

```

{% endraw %}
This creates 8 job combinations (3×3 - 1 exclusion):

```
ubuntu-latest + node 18
ubuntu-latest + node 20
ubuntu-latest + node 22 (experimental)
windows-latest + node 18
windows-latest + node 20
windows-latest + node 22
macos-latest + node 20
macos-latest + node 22

```

---

## 2.4 Actions Marketplace

### Using Actions

```yaml
steps:
  # Specific version (recommended)
  - uses: actions/checkout@v4
  
  # Specific commit SHA (most secure)
  - uses: actions/checkout@8ade135a41bc03ea155e62e844d188df1ea18608
  
  # Branch (not recommended - can change)
  - uses: actions/checkout@main
  
  # With inputs
  - uses: actions/setup-node@v4
    with:
      node-version: '20'
      cache: 'npm'
      
  # Action from another repository
  - uses: owner/repo/path@v1
  
  # Local action
  - uses: ./.github/actions/my-action
  
  # Docker action
  - uses: docker://alpine:3.18

```

### Popular Actions

| Action | Purpose |
|--------|---------|
| `actions/checkout` | Clone repository |
| `actions/setup-node` | Set up Node.js |
| `actions/setup-python` | Set up Python |
| `actions/setup-java` | Set up Java |
| `actions/cache` | Cache dependencies |
| `actions/upload-artifact` | Upload build artifacts |
| `actions/download-artifact` | Download artifacts |
| `actions/github-script` | Run JavaScript with Octokit |
| `softprops/action-gh-release` | Create GitHub releases |
| `docker/build-push-action` | Build and push Docker images |

## 2.5 Secrets and Security

### Secret Types
{% raw %}

```yaml
# Repository secrets
${{ secrets.API_KEY }}
# Environment secrets (scoped to environment)
environment: production
${{ secrets.PROD_API_KEY }}
# Organization secrets (shared across repos)
${{ secrets.ORG_NPM_TOKEN }}
# Automatic token
${{ secrets.GITHUB_TOKEN }}        # Scoped to repository

```

{% endraw %}

### GITHUB_TOKEN Permissions

```yaml
permissions:
  contents: read                   # Read repo contents
  pull-requests: write             # Comment on PRs
  issues: write                    # Create/update issues
  packages: write                  # Push to GHCR
  id-token: write                  # OIDC token (for cloud auth)
  
# Or set at job level
jobs:
  deploy:
    permissions:
      contents: read
      deployments: write

```

### Security Best Practices
{% raw %}

```yaml
# Pin actions to SHA
- uses: actions/checkout@8ade135a41bc03ea155e62e844d188df1ea18608
# Limit GITHUB_TOKEN permissions
permissions:
  contents: read
# Use environments for sensitive deployments
jobs:
  deploy:
    environment: production
    
# Don't echo secrets
- run: |
    # Bad - secret might appear in logs
    echo ${{ secrets.API_KEY }}
    
    # Good - mask automatically
    echo "::add-mask::${{ secrets.API_KEY }}"
    
# Use OIDC for cloud providers (no long-lived secrets)
- uses: aws-actions/configure-aws-credentials@v4
  with:
    role-to-assume: arn:aws:iam::123456789:role/github-actions
    aws-region: us-east-1

```

{% endraw %}
---

## 2.6 Artifacts and Caching

### Artifacts
Share data between jobs or store build outputs:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm run build
      
      - uses: actions/upload-artifact@v4
        with:
          name: build-output
          path: dist/
          retention-days: 5
          
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/download-artifact@v4
        with:
          name: build-output
          path: dist/
          
      - run: ./deploy.sh dist/

```

### Caching
Speed up workflows by caching dependencies:
{% raw %}

```yaml
# Automatic caching with setup actions
- uses: actions/setup-node@v4
  with:
    node-version: '20'
    cache: 'npm'                   # Automatic npm cache
# Manual caching
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: npm-${{ runner.os }}-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      npm-${{ runner.os }}-
      
# Gradle example
- uses: actions/cache@v4
  with:
    path: |
      ~/.gradle/caches
      ~/.gradle/wrapper
    key: gradle-${{ runner.os }}-${{ hashFiles('**/*.gradle*', '**/gradle-wrapper.properties') }}

```

{% endraw %}
