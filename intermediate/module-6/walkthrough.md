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

## Getting Started with GitHub Advanced Security

Welcome to the hands-on walkthrough for GitHub Advanced Security (GHAS)! This section guides you through enabling, configuring, and operationalizing the security features that protect your code and supply chain.

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>
GHAS rollouts are often the starting point for security-focused conversations. Customers frequently ask "where do I start?" The answer: Enable features progressively—start with Dependabot and secret scanning (quick wins), then add CodeQL for deeper analysis. This walkthrough follows that progression.
</div>

**What you'll configure:**
- Repository and organization-level security settings
- CodeQL for code scanning (default and advanced setup)
- Dependabot for dependency management
- Push protection for secret scanning
- Alert triage workflows

**Documentation Reference:**
- [GitHub Advanced Security overview](https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security)
- [Code scanning with CodeQL](https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning-with-codeql)
- [Secret scanning](https://docs.github.com/en/code-security/secret-scanning/about-secret-scanning)

---

## Walkthrough 1: Enabling GHAS Features

Enabling GHAS follows a hierarchy: **Enterprise → Organization → Repository**. Start at the appropriate level for your rollout strategy.

### Understanding the Feature Hierarchy

| Feature | License Required | Enable At |
|---------|------------------|-----------|
| **Dependency graph** | Free (public), Team+ (private) | Repository |
| **Dependabot alerts** | Free (public), Team+ (private) | Organization/Repository |
| **Secret scanning** | Free (public, default ON), GHAS (private) | Organization/Repository |
| **Push protection** | Free (public, default ON), GHAS (private) | Organization/Repository |
| **Code scanning** | Free (public), GHAS (private) | Repository (workflow) |
| **Copilot Autofix** | GHAS license | Repository (automatic with code scanning) |

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

<div class="callout callout-info" markdown="1">
<div class="callout-title">📖 Bulk Enablement</div>
Organization admins can enable GHAS features across all repositories at once. Go to Organization Settings → Code security and analysis, then use the "Enable all" buttons. This is typically used during initial GHAS rollout to establish a baseline.
</div>

**Documentation:** [Configuring global security settings](https://docs.github.com/en/code-security/getting-started/securing-your-organization)

---

## Walkthrough 2: Configuring CodeQL

CodeQL is GitHub's semantic code analysis engine. It queries your code like a database, finding vulnerabilities and anti-patterns that syntax-based tools miss.

### Default vs. Advanced Setup

| Setup Type | Best For | Configuration |
|------------|----------|---------------|
| **Default** | Quick start, standard languages | UI-based, auto-detected languages |
| **Advanced** | Custom queries, monorepos, compiled languages | Workflow file, full control |

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>
Start customers with Default setup—it works for 80% of cases and shows value immediately. Move to Advanced setup when they need custom queries, build-mode control for compiled languages, or integration with their existing CI pipelines.
</div>

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

**Documentation:** [CodeQL CLI](https://docs.github.com/en/code-security/codeql-cli/getting-started-with-the-codeql-cli/about-the-codeql-cli) | [Query suites](https://docs.github.com/en/code-security/code-scanning/managing-your-code-scanning-configuration/codeql-query-suites)

---

## Walkthrough 3: Configuring Dependabot

Dependabot automates dependency updates, creating PRs when new versions are available. Combined with Dependabot alerts, it creates a complete vulnerability response workflow.

### Dependabot Components

| Component | Function |
|-----------|----------|
| **Dependabot alerts** | Notify you of known vulnerabilities |
| **Dependabot security updates** | Auto-create PRs for security fixes |
| **Dependabot version updates** | Keep all dependencies current (configurable) |

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>
Help customers tune Dependabot to avoid PR fatigue. Use grouping to batch minor/patch updates, schedule updates for low-activity times, and set up auto-merge for safe updates. An overwhelmed team ignores Dependabot; a well-configured team adopts it.
</div>

**Documentation:** [Configuring Dependabot version updates](https://docs.github.com/en/code-security/dependabot/dependabot-version-updates/configuring-dependabot-version-updates)

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

<div class="callout callout-warning" markdown="1">
<div class="callout-title">⚠️ Auto-Merge Caution</div>
Auto-merge is powerful but requires good test coverage. Only auto-merge updates that your test suite can validate. Start conservative (patch updates to dev dependencies) and expand as confidence grows.
</div>

---

## Walkthrough 4: Setting Up Push Protection

Push protection prevents secrets from being committed in the first place—a proactive approach compared to alerting after the fact. It's one of the most impactful GHAS features for preventing credential leaks.

### How Push Protection Works

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
    A["Developer commits file with secret"] --> B["git push"]
    B --> C["GitHub detects secret pattern"]
    C --> D["Push is BLOCKED"]
    D --> E["Developer removes secret\nor bypasses with audit trail"]
</div>
</div>

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>
Push protection is often the fastest path to GHAS value. It requires no workflow changes—developers just try to push and get blocked. Use this as the "hook" in demos: commit a fake AWS key and watch it get blocked in real-time.
</div>

**Documentation:** [Push protection for secret scanning](https://docs.github.com/en/code-security/secret-scanning/push-protection-for-repositories-and-organizations)

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

---

## Walkthrough 5: Triaging Security Alerts

Alert triage is where security tooling meets security practice. Having the right workflow turns GHAS from "more alerts" into "actionable security insights."

### Triage Workflow Overview

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
    A["New Alert Created"] --> B["Review in Security Overview\n(org) or Security tab (repo)"]
    B --> C["Assess: Is it real?\nIs it critical?"]
    C --> D["Action: Fix, Dismiss\n(with reason), or Create Issue"]
    D --> E["Track metrics:\nMean time to remediate"]
</div>
</div>

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>
Help customers establish an alert triage process before rollout. Questions to answer: Who reviews alerts? What's the SLA by severity? Who can dismiss? Without answers, alerts pile up and teams lose trust in the tooling.
</div>

**Documentation:** [Managing code scanning alerts](https://docs.github.com/en/code-security/code-scanning/managing-code-scanning-alerts/managing-code-scanning-alerts-for-your-repository)

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

---

## Summary

You've now configured the core GHAS capabilities:

| Feature | Value Delivered |
|---------|-----------------|
| **Dependabot** | Automated dependency updates and vulnerability alerts |
| **Secret scanning** | Detection of exposed credentials |
| **Push protection** | Proactive secret leak prevention |
| **CodeQL** | Deep semantic code analysis |
| **Alert triage** | Operational security workflow |

<div class="callout callout-success" markdown="1">
<div class="callout-title">✅ Ready for Labs</div>
You've seen how to enable and configure GHAS features. In the Hands-On Labs, you'll work with real alerts and practice the triage workflow.
</div>

**Next Steps:**
- Complete the [Hands-On Labs](/intermediate/module-6/labs/) to practice alert triage
- Review [Security Overview documentation](https://docs.github.com/en/code-security/security-overview/about-security-overview)
- Explore [Custom CodeQL queries](https://codeql.github.com/docs/codeql-language-guides/)

{% endraw %}
