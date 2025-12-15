---
layout: training-module
title: "Module 6, Section 5: Knowledge Check"
description: "Test your understanding of GHAS concepts"
permalink: /intermediate/module-6/quiz/
module_number: 6
section_number: 5
phase: intermediate
prev_section:
  url: /intermediate/module-6/labs/
  title: "Hands-On Labs"
next_section:
  url: /intermediate/module-6/resources/
  title: "Resources"
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
Test your understanding of GitHub Advanced Security concepts.
---

## Questions

### Question 1
What's the difference between Dependabot alerts and Dependabot security updates?
<details markdown="1">
<summary>View Answer</summary>
**Dependabot Alerts:**
- Notifications about known vulnerabilities in dependencies
- Does not make any changes
- Just informs you about the problem
- Shows affected dependency and severity
- Links to advisory information
**Dependabot Security Updates:**
- Automatically creates PRs to fix vulnerable dependencies
- Only triggered by security advisories
- Tries to update to the minimum version that fixes the vulnerability
- Requires alerts to be enabled first
Additionally, there are **Dependabot Version Updates**:
- Creates PRs to keep ALL dependencies up to date
- Not just for security, also for features
- Configured via dependabot.yml
- Can be grouped, scheduled, and filtered
</details>

### Question 2
A developer bypassed push protection with reason "I'll fix it later." What happens and what should the security team do?
<details markdown="1">
<summary>View Answer</summary>
**What happens:**
1. Push is allowed (secret enters repository)
2. A secret scanning alert is created
3. Bypass is logged in audit log
4. Alert shows "Bypassed: I'll fix it later"
5. If partner pattern, partner may still be notified
**Security team should:**
1. Review the bypass in audit log
2. Contact the developer to understand context
3. Ensure the secret is:
   - Rotated immediately if production credential
   - Removed from git history using BFG or git-filter-repo
   - Added to `.gitignore` if configuration file
4. Document the incident
5. If chronic bypasser, consider revoking bypass privilege
**Best practice:** 
Configure bypass notifications to alert security team immediately when any bypass occurs, regardless of reason.
</details>
---

### Question 3
How do you reduce false positives in CodeQL scanning?
<details markdown="1">
<summary>View Answer</summary>
**Configuration approaches:**
1. **Use appropriate query suite:**

```yaml
# Default suite has fewer false positives
queries: default
# vs extended which is more thorough but noisier
queries: +security-extended

```

2. **Exclude test files:**

```yaml
# codeql-config.yml
paths-ignore:
  - "**/test/**"
  - "**/tests/**"
  - "**/*_test.go"

```

3. **Use query filters:**

```yaml
query-filters:
  - exclude:
      tags contain: /experimental/

```

4. **Dismiss with reason:**
- Mark false positives individually
- Use "Dismiss: False positive" with clear notes
**Remediation approaches:**
5. **Add code annotations:**

```java
// codeql-disable-next-line
String query = "SELECT * FROM " + table;

```

6. **Customize queries:**
- Create `.github/codeql/` directory
- Add model files to mark safe functions
- Exclude specific patterns
7. **Report to GitHub:**
- True false positives should be reported
- Helps improve CodeQL for everyone
</details>

### Question 4
What's the recommended order for rolling out GHAS to a large organization?
<details markdown="1">
<summary>View Answer</summary>
**Phased rollout approach:**
**Phase 1: Pilot (2-4 weeks)**
- Select 5-10 representative repositories
- Include: high-risk, active development, various languages
- Enable all GHAS features
- Measure: alert volume, false positive rate, fix time
**Phase 2: High-Risk Repositories (4-6 weeks)**
- Identify repositories with:
  - Customer data
  - Payment processing
  - Authentication systems
- Enable GHAS with configured policies
- Train developers on triage
**Phase 3: All Active Repositories (ongoing)**
- Roll out to remaining active repos
- Use organization settings for defaults
- Prioritize based on commit activity
**Phase 4: Enforcement (after baseline)**
- Enable branch protection integration
- Require CodeQL checks to pass
- Enable push protection
**Key metrics to track:**
- Mean time to remediation (MTTR)
- False positive rate
- Developer feedback/adoption
- Alert backlog size
**Anti-patterns to avoid:**
- Enabling for all repos simultaneously
- Requiring checks before baseline established
- Not training developers first
</details>
---

### Question 5
How do you configure CodeQL for a compiled language like Java that requires a build step?
<details markdown="1">
<summary>View Answer</summary>
**Option 1: Autobuild (recommended for simple projects)**

```yaml
- name: Initialize CodeQL
  uses: github/codeql-action/init@v3
  with:
    languages: java
    
- name: Autobuild
  uses: github/codeql-action/autobuild@v3

```

**Option 2: Manual build (complex projects)**

```yaml
- name: Initialize CodeQL
  uses: github/codeql-action/init@v3
  with:
    languages: java
    
- name: Setup Java
  uses: actions/setup-java@v4
  with:
    distribution: 'temurin'
    java-version: '17'
    
- name: Build with Maven
  run: mvn clean compile -DskipTests
  
- name: Perform CodeQL Analysis
  uses: github/codeql-action/analyze@v3

```

**Option 3: Build mode none (Java/Kotlin 17+)**

```yaml
- name: Initialize CodeQL
  uses: github/codeql-action/init@v3
  with:
    languages: java-kotlin
    build-mode: none  # Extracts without compilation

```

**Key considerations:**
- Build must succeed for extraction to work
- Skip tests to speed up analysis
- Use same JDK version as production
- Multi-module projects may need special handling
</details>

### Question 6
What license types should you typically block with dependency review?
<details markdown="1">
<summary>View Answer</summary>
**Commonly blocked licenses:**
**Copyleft (viral) licenses:**
- GPL-3.0 - Requires derivative works to be GPL
- AGPL-3.0 - Even network use triggers copyleft
- GPL-2.0 - Similar to GPL-3.0
**Typically safe to allow:**
- MIT - Very permissive
- Apache-2.0 - Permissive with patent grant
- BSD-2-Clause - Permissive
- BSD-3-Clause - Permissive
- ISC - Similar to MIT
**Configuration example:**

```yaml
- uses: actions/dependency-review-action@v4
  with:
    fail-on-severity: high
    deny-licenses: >-
      GPL-3.0,
      AGPL-3.0,
      GPL-2.0,
      LGPL-3.0
    allow-licenses: >-
      MIT,
      Apache-2.0,
      BSD-2-Clause,
      BSD-3-Clause,
      ISC,
      MPL-2.0

```

**Note:** Always consult with legal team for organization-specific requirements. Some companies have different risk tolerances.
</details>
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
