---
layout: training-module
title: "Module 6, Section 3: Guided Walkthrough"
description: "Step-by-step guides for enabling and configuring GHAS features"
permalink: /intermediate/module-6/walkthrough/
module_number: 6
section_number: 3
phase: intermediate
prev_section:
  url: /intermediate/module-6/concepts/
  title: "Core Concepts"
next_section:
  url: /intermediate/module-6/labs/
  title: "Hands-On Labs"
toc: true
module_title: "Code Security with GHAS"
total_sections: 7
module_index: /intermediate/module-6/
sections:
  - title: "Context & Overview"
    url: "/intermediate/module-6/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "20 min"
  - title: "Core Concepts"
    url: "/intermediate/module-6/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/intermediate/module-6/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "50 min"
  - title: "Hands-On Labs"
    url: "/intermediate/module-6/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/intermediate/module-6/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "25 min"
  - title: "Resources"
    url: "/intermediate/module-6/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/intermediate/module-6/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "30 min"
---
{% raw %}

## Walkthrough 1: Enabling GHAS Features

### Step 1: Enable at Repository Level
1. Go to repository **Settings** → **Code security and analysis**
2. Enable features:
   ```
   Dependency graph: ☑ Enabled
   Dependabot alerts: ☑ Enabled
   Dependabot security updates: ☑ Enabled
   Secret scanning: ☑ Enabled
   Push protection: ☑ Enabled
   Code scanning: Click "Set up" → Default setup
   ```

### Step 2: Enable at Organization Level
1. Go to Organization **Settings** → **Code security and analysis**
2. Configure default settings for new repositories
3. Enable for all existing repositories (bulk enable)

### Step 3: Configure Branch Protection Integration
1. Go to repository **Settings** → **Branches**
2. Edit main branch protection
3. Add required status checks:
   - `CodeQL`
   - `Dependency Review`
---

## Walkthrough 2: Configuring CodeQL

### Step 1: Default Setup (Quick)
1. Repository → **Security** tab → **Code scanning**
2. Click **Configure CodeQL alerts**
3. Select **Default** setup
4. Select languages to analyze
5. Click **Enable CodeQL**

### Step 2: Advanced Setup (Custom)
Create `.github/workflows/codeql.yml`:

```yaml
name: CodeQL
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 4 * * 0'  # Sunday 4 AM
jobs:
  analyze:
    name: Analyze (${{ matrix.language }})
    runs-on: ${{ (matrix.language == 'swift' && 'macos-latest') || 'ubuntu-latest' }}
    timeout-minutes: 360
    
    permissions:
      security-events: write
      packages: read
      actions: read
      contents: read
    strategy:
      fail-fast: false
      matrix:
        include:
          - language: javascript-typescript
            build-mode: none
          - language: python
            build-mode: none
          - language: java-kotlin
            build-mode: autobuild
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      - name: Initialize CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: ${{ matrix.language }}
          build-mode: ${{ matrix.build-mode }}
          queries: +security-extended,+security-and-quality
          
      - if: matrix.build-mode == 'manual'
        name: Manual Build
        shell: bash
        run: |
          ./build.sh
      - name: Perform CodeQL Analysis
        uses: github/codeql-action/analyze@v3
        with:
          category: "/language:${{ matrix.language }}"
          
      - name: Upload SARIF
        uses: github/codeql-action/upload-sarif@v3
        if: always()
        with:
          sarif_file: ../results

```

### Step 3: Add Custom Queries
Create `.github/codeql/custom-queries/security.qls`:

```yaml
- description: "Custom security queries"
- queries: .
- apply: security-extended
- apply: security-and-quality

```

## Walkthrough 3: Configuring Dependabot

### Step 1: Create Dependabot Configuration
`.github/dependabot.yml`:

```yaml
version: 2
registries:
  npm-artifactory:
    type: npm-registry
    url: https://artifactory.company.com/npm/
    token: ${{ secrets.ARTIFACTORY_TOKEN }}
updates:
  # JavaScript/npm
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
      time: "06:00"
      timezone: "America/New_York"
    open-pull-requests-limit: 10
    registries:
      - npm-artifactory
    reviewers:
      - "frontend-team"
    labels:
      - "dependencies"
      - "javascript"
    commit-message:
      prefix: "chore(deps)"
    groups:
      # Group all minor/patch updates together
      minor-and-patch:
        update-types:
          - "minor"
          - "patch"
          
  # Python
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
    reviewers:
      - "backend-team"
    labels:
      - "dependencies"
      - "python"
    ignore:
      # Ignore Django major updates (handle manually)
      - dependency-name: "django"
        update-types: ["version-update:semver-major"]
        
  # Docker
  - package-ecosystem: "docker"
    directory: "/"
    schedule:
      interval: "weekly"
    reviewers:
      - "platform-team"
      
  # GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"
    groups:
      actions:
        patterns:
          - "*"

```

### Step 2: Configure Auto-Merge for Safe Updates
Create `.github/workflows/dependabot-auto-merge.yml`:

```yaml
name: Dependabot Auto-merge
on: pull_request
permissions:
  contents: write
  pull-requests: write
jobs:
  auto-merge:
    runs-on: ubuntu-latest
    if: github.actor == 'dependabot[bot]'
    
    steps:
      - name: Fetch Dependabot metadata
        id: metadata
        uses: dependabot/fetch-metadata@v2
        with:
          github-token: "${{ secrets.GITHUB_TOKEN }}"
          
      - name: Auto-merge safe updates
        if: |
          steps.metadata.outputs.update-type == 'version-update:semver-patch' ||
          (steps.metadata.outputs.update-type == 'version-update:semver-minor' && 
           steps.metadata.outputs.dependency-type == 'direct:development')
        run: gh pr merge --auto --squash "$PR_URL"
        env:
          PR_URL: ${{ github.event.pull_request.html_url }}
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```

---

## Walkthrough 4: Setting Up Push Protection

### Step 1: Enable Push Protection
1. Repository **Settings** → **Code security and analysis**
2. Under "Secret scanning":
   - ☑ Push protection

### Step 2: Test Push Protection

```bash
# Create a file with a test secret
echo "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" > config.js
git add config.js
git commit -m "Add config"
git push origin main
# Push will be rejected with instructions

```

### Step 3: Handle Push Protection Bypasses
When bypass is needed:

1. Visit the URL provided in the rejection message
2. Select a bypass reason:
   - "It's a false positive"
   - "Used in tests"
   - "I'll fix it later"
3. Complete the bypass
4. Push succeeds (logged for audit)

### Step 4: Configure Bypass Alerts
At organization level, configure who receives notifications when push protection is bypassed:

- Security team
- Repository administrators

## Walkthrough 5: Triaging Security Alerts

### Step 1: Access Security Overview
For organization-wide view:

1. Go to Organization → **Security** tab
2. View **Security overview**
For repository:

1. Go to Repository → **Security** tab

### Step 2: Triage Code Scanning Alerts
1. Click on **Code scanning alerts**
2. Filter by severity, state, or rule
3. Click on an alert to see:
   - Affected code
   - Data flow path
   - Remediation guidance

### Step 3: Alert Actions

| Action | When to Use |
|--------|-------------|
| **Dismiss: False positive** | Detection error, not vulnerable |
| **Dismiss: Won't fix** | Accepted risk, documented |
| **Dismiss: Used in tests** | Test code, not production |
| **Create issue** | Track for future fix |
| **Fix** | Remediate the vulnerability |

### Step 4: Configure Alert Notifications
Set up Slack/email notifications:

1. Repository **Settings** → **Notifications**
2. Configure security alert notifications
3. Or use GitHub Actions:

```yaml
on:
  code_scanning_alert:
    types: [created]
  secret_scanning_alert:
    types: [created]
    
jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Notify security team
        uses: slackapi/slack-github-action@v1
        with:
          payload: |
            {
              "text": "Security alert in ${{ github.repository }}: ${{ github.event.alert.rule.description }}"
            }
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK }}

```

{% endraw %}
