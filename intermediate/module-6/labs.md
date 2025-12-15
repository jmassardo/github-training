---
layout: training-module
title: "Module 6, Section 4: Hands-On Labs"
description: "Practical exercises for CodeQL, Dependabot, and secret scanning"
permalink: /intermediate/module-6/labs/
module_number: 6
section_number: 4
phase: intermediate
prev_section:
  url: /intermediate/module-6/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /intermediate/module-6/quiz/
  title: "Knowledge Check"
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

## Lab 1: CodeQL Deep Dive
**Objective:** Configure comprehensive code scanning with custom queries

### Setup

```bash
# Clone a vulnerable application for testing
gh repo clone OWASP/WebGoat
cd WebGoat

```

### Tasks
1. **Enable default CodeQL scanning**
2. **Add security-extended queries**
3. **Create custom query configuration:**

```yaml
# .github/codeql/codeql-config.yml
name: "Custom CodeQL Config"
queries:
  - uses: security-extended
  - uses: security-and-quality
  
query-filters:
  - exclude:
      tags contain: /test/
      
paths-ignore:
  - "**/test/**"
  - "**/tests/**"
  - "**/vendor/**"

```

4. **Review and triage at least 5 alerts**
5. **Document false positives with clear reasoning**

### Validation
- [ ] CodeQL running on PRs and main branch
- [ ] Extended query suite enabled
- [ ] Custom configuration applied
- [ ] Alerts triaged with appropriate actions
---

## Lab 2: Comprehensive Dependabot Setup
**Objective:** Configure Dependabot for a multi-language project

### Tasks
1. **Create project structure:**

```bash
mkdir -p frontend backend infrastructure
cd frontend && npm init -y && npm install express lodash
cd ../backend && pip freeze > requirements.txt
cd ../infrastructure && touch Dockerfile

```

2. **Create comprehensive dependabot.yml:**
   - Daily updates for npm with grouping
   - Weekly updates for pip
   - Weekly updates for Docker
   - Weekly updates for GitHub Actions
3. **Configure auto-merge for safe updates:**
   - Patch updates: auto-merge
   - Minor dev dependency updates: auto-merge
   - Everything else: require approval
4. **Add dependency review to PRs:**
   - Block on high severity vulnerabilities
   - Block on AGPL licenses

### Validation
- [ ] Dependabot creates PRs for each ecosystem
- [ ] Auto-merge works for patch updates
- [ ] Dependency review blocks vulnerable PRs

## Lab 3: Secret Scanning and Push Protection
**Objective:** Test and configure secret scanning capabilities

### Tasks
1. **Test secret detection:**

```bash
# Create files with test secrets (use fake/test tokens)
echo "aws_access_key_id = AKIAIOSFODNN7EXAMPLE" > test-secrets.txt
echo "ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx1" >> test-secrets.txt
git add test-secrets.txt
git commit -m "Test secrets"
git push  # Should be blocked

```

2. **Configure custom patterns (if Enterprise):**

```yaml
# Add pattern for internal API keys
pattern: "INTERNAL_[A-Z]{4}_[a-f0-9]{32}"

```

3. **Set up alert routing:**
   - Route AWS alerts to cloud team
   - Route database credentials to DBA team
   - Route all others to security team
4. **Create runbook for secret remediation:**
   - Document rotation procedures
   - List contacts for each secret type
   - Define SLAs for remediation

### Validation
- [ ] Push protection blocks test secrets
- [ ] Historical scan finds existing secrets
- [ ] Alert routing configured correctly
{% endraw %}
---
<div class="section-navigation">
  <a href="{{ page.prev_section.url }}" class="nav-button nav-prev">
    <span class="nav-label">← Previous</span>
    <span class="nav-title">{{ page.prev_section.title }}</span>
  </a>
  <a href="{{ page.next_section.url }}" class="nav-button nav-next">
    <span class="nav-label">Next →</span>
    <span class="nav-title">{{ page.next_section.title }}</span>
  </a>
</div>
