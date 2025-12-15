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

## Walkthrough 1: Your First CI Pipeline
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
---

## Walkthrough 2: Matrix Testing
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

## Walkthrough 3: Reusable Workflows
Create a reusable workflow that can be called from other workflows.

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

---

## Walkthrough 4: Creating a Custom Action
Create a composite action for common setup steps.

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

## Walkthrough 5: Environment Deployments
Set up deployment with environment protection.

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

{% endraw %}
