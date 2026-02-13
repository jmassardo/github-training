---
layout: training-module
title: "Module 6, Section 2: Core Concepts"
description: "Deep dive into CodeQL, secret scanning, Dependabot, and security policies"
permalink: /intermediate/module-6/concepts/
module_number: 6
section_number: 2
phase: intermediate
prev_section:
  url: /intermediate/module-6/overview/
  title: "Context & Overview"
next_section:
  url: /intermediate/module-6/walkthrough/
  title: "Guided Walkthrough"
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

<div class="callout callout-info" markdown="1">
<div class="callout-title">📖 Official Documentation</div>
For comprehensive reference, see <a href="https://docs.github.com/en/code-security">GitHub Code Security Documentation</a> and the <a href="https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security">GHAS Overview</a>.
</div>

## 2.1 Code Scanning with CodeQL
CodeQL is GitHub's semantic code analysis engine:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    Code["Your Code\n(Parse source)"] --> DB["CodeQL Database\n(Extract facts about code)"] --> Query["Query Execution\n(Run security queries)"] --> Alerts["Security Alerts\n(Report vulnerabilities)"]
</div>
</div>

### Supported Languages

| Language | Support Level | Features |
|----------|--------------|----------|
| C/C++ | Full | Memory safety, injection |
| C# | Full | Injection, crypto issues |
| Go | Full | Injection, path traversal |
| Java/Kotlin | Full | Full OWASP coverage |
| JavaScript/TypeScript | Full | XSS, injection, prototype pollution |
| Python | Full | Injection, SSRF, path traversal |
| Ruby | Full | Injection, command execution |
| Swift | Beta | Common vulnerabilities |

### CodeQL Workflow

```yaml
name: CodeQL Analysis
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 2 * * 1'  # Weekly Monday 2 AM
jobs:
  analyze:
    name: Analyze
    runs-on: ubuntu-latest
    permissions:
      security-events: write
      actions: read
      contents: read
      
    strategy:
      fail-fast: false
      matrix:
        language: ['javascript', 'python']
        
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        
      - name: Initialize CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: ${{ matrix.language }}
          queries: +security-extended
          
      - name: Autobuild
        uses: github/codeql-action/autobuild@v3
        
      - name: Perform CodeQL Analysis
        uses: github/codeql-action/analyze@v3
        with:
          category: "/language:${{ matrix.language }}"

```

### Query Suites

| Suite | Coverage | Use Case |
|-------|----------|----------|
| `default` | High-confidence findings | Production-ready alerts |
| `security-extended` | More rules, some false positives | Thorough security review |
| `security-and-quality` | Security + code quality | Comprehensive analysis |
---

## 2.2 Secret Scanning

### How It Works
Secret scanning detects 200+ token patterns from 100+ service providers:

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    PUSH["📤 Git Push"] --> SCAN
    
    subgraph SCAN["🔍 Secret Scanning"]
        ENGINE["Pattern Matching Engine<br/>• AWS Keys: AKIA...<br/>• GitHub PAT: ghp_...<br/>• Stripe: sk_live_...<br/>• 200+ more patterns"]
    end
    
    SCAN --> PP["🛡️ Push Protection<br/>(BLOCK)"]
    SCAN --> ALERT["⚠️ Alert Created<br/>(post-commit)<br/>Notify partner"]
</div>
</div>

### Secret Types
**Partner Patterns (auto-revoked):**

- AWS Access Keys → AWS notified, key revoked
- GitHub Personal Access Tokens → Automatically revoked
- Stripe API Keys → Stripe notified
- Azure credentials → Azure AD notified
**User Patterns (detected only):**

- Generic API keys
- Private keys
- Connection strings
- Custom patterns (Enterprise)

### Custom Patterns (Enterprise)

```yaml
# Custom pattern definition
patterns:
  - name: "Internal API Key"
    pattern: "MYCOMPANY_API_[A-Za-z0-9]{32}"
    type: "api_key"
    
  - name: "Database Connection String"
    pattern: "mongodb\\+srv://[^:]+:[^@]+@[^/]+/[^\\s]+"
    type: "connection_string"

```

## 2.3 Push Protection
Push protection blocks secrets BEFORE they enter the repository:

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph BLOCKED["🛑 Push Rejected"]
        CMD["$ git push origin main"]
        MSG["remote: Push rejected<br/><br/>Secret scanning found secrets:<br/>— GitHub Personal Access Token<br/>   Location: src/config.js:15<br/>   Secret: ghp_xxxx...xxxx<br/><br/>Remove the secret to push."]
    end
    CMD --> MSG
</div>
</div>

### Bypass Options
When a legitimate secret is detected:

1. **Remove the secret** (recommended)
2. **Mark as false positive** (appears safe but isn't)
3. **It's used in tests** (sandbox/test environment)
4. **I'll fix it later** (creates alert, allows push)
All bypasses are logged for audit.
---

## 2.4 Dependabot

### Dependabot Alerts
Automatically notifies when dependencies have known vulnerabilities:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    Graph["Dependency Graph\n(package.json,\nrequirements.txt,\npom.xml)"] --> Advisory["Advisory Database\n(GitHub Advisory\nDatabase, NVD, vendor)"] --> Match["Match\n(Found CVE)"] --> Alert["Alert\n(Create alert with\nseverity and fix info)"]
</div>
</div>

### Dependabot Security Updates
Automatically creates PRs to fix vulnerable dependencies:

```yaml
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    open-pull-requests-limit: 10
    
  - package-ecosystem: "pip"
    directory: "/"
    schedule:
      interval: "weekly"
      day: "monday"
      
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "weekly"

```

### Dependabot Version Updates
Keep all dependencies up-to-date (not just security):

```yaml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    versioning-strategy: "increase"
    groups:
      # Group minor/patch updates
      development-dependencies:
        dependency-type: "development"
        update-types:
          - "minor"
          - "patch"
      production-dependencies:
        dependency-type: "production"
        update-types:
          - "patch"
    ignore:
      # Ignore specific packages
      - dependency-name: "aws-sdk"
        update-types: ["version-update:semver-major"]

```

## 2.5 Dependency Review
Catches vulnerabilities introduced in pull requests:

```yaml
name: Dependency Review
on: [pull_request]
permissions:
  contents: read
  pull-requests: write
jobs:
  dependency-review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Dependency Review
        uses: actions/dependency-review-action@v4
        with:
          fail-on-severity: moderate
          deny-licenses: GPL-3.0, AGPL-3.0
          allow-licenses: MIT, Apache-2.0, BSD-3-Clause

```

---

## 2.6 Security Policies

### SECURITY.md

```markdown
# Security Policy
## Supported Versions

| Version | Supported          |
| ------- | ------------------ |
| 5.1.x   | :white_check_mark: |
| 5.0.x   | :x:                |
| 4.0.x   | :white_check_mark: |
| < 4.0   | :x:                |
## Reporting a Vulnerability
Please report security vulnerabilities through GitHub's 
private vulnerability reporting feature:

1. Go to the Security tab
2. Click "Report a vulnerability"
3. Fill out the form with details
**Please do not report security vulnerabilities through 
public GitHub issues.**
## Response Timeline
- Initial response: 48 hours
- Triage and assessment: 7 days
- Fix timeline: Depends on severity
  - Critical: 24-48 hours
  - High: 7 days
  - Medium: 30 days
  - Low: 90 days
## Bug Bounty
We offer bounties for qualifying vulnerabilities:

- Critical: $5,000
- High: $2,500
- Medium: $500
- Low: $100

```

### Private Vulnerability Reporting
Enable private vulnerability reporting to receive security reports directly on GitHub without public disclosure.
{% endraw %}
