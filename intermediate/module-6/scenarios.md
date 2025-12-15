---
layout: training-module
title: "Module 6, Section 7: Real-World CSM Scenarios"
description: "Practice handling customer questions about GitHub Advanced Security"
permalink: /intermediate/module-6/scenarios/
module_number: 6
section_number: 7
phase: intermediate
prev_section:
  url: /intermediate/module-6/resources/
  title: "Resources"
next_section:
  url: /intermediate/module-7/
  title: "Module 7: GitHub Packages"
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

## Scenario 1: GHAS Enterprise Rollout

### Situation
A large enterprise customer (5000+ developers, 2000+ repositories) wants to roll out GHAS. They're concerned about alert fatigue and developer pushback.

### Customer Question
"How do we roll out GHAS without overwhelming our developers with alerts?"

### Your Response
"Great concern to raise upfront. Here's a battle-tested rollout strategy:
**Phase 1: Establish Baseline (Weeks 1-2)**
- Enable Dependabot alerts and secret scanning in 'audit only' mode
- Don't notify developers yet
- Measure current vulnerability density
**Phase 2: Silent Pilot (Weeks 3-4)**
- Enable CodeQL on 10 representative repos
- Security team triages ALL alerts
- Identify false positive patterns
- Create triage playbook
**Phase 3: Developer Pilot (Weeks 5-8)**
- Expand to 50 repos with engaged teams
- Enable notifications
- Provide training sessions
- Measure MTTR and developer feedback
**Phase 4: Wide Rollout (Weeks 9-16)**
- Roll out to all active repositories
- 100-200 repos per week
- Monitor alert volumes
- Adjust configurations as needed
**Phase 5: Enforcement (Week 17+)**
- Enable branch protection integration
- Require CodeQL checks to pass
- Enable push protection
**Key success factors:**
1. **Training before rollout** - Developers need to understand what alerts mean
2. **Clear ownership** - Define who triages which alert types
3. **SLA agreements** - Critical: 24h, High: 7d, Medium: 30d
4. **Success metrics** - Track MTTR, not just alert count
5. **Executive sponsorship** - Security is a priority, not a blocker
Shall I help create a detailed rollout plan with your specific timeline?"
---

## Scenario 2: High False Positive Rate

### Situation
A customer enabled CodeQL but developers are ignoring alerts because "they're all false positives." The security team is frustrated.

### Customer Question
"Our developers say CodeQL has too many false positives. They've stopped paying attention to alerts. How do we fix this?"

### Your Response
"This is a common challenge. Let's diagnose and fix it:
**Immediate Actions:**
1. **Audit recent dismissals:**

```graphql
# Check dismissed alerts
query {
  repository(owner: \"org\", name: \"repo\") {
    codeScanning {
      alerts(first: 100, states: DISMISSED) {
        nodes {
          dismissedReason
          rule { description }
        }
      }
    }
  }
}

```

2. **Check query suite:**

```yaml
# If using security-and-quality, switch to default
queries: default  # Instead of +security-extended

```

**Configuration Improvements:**
3. **Exclude test code:**

```yaml
paths-ignore:
  - \"**/test/**\"
  - \"**/tests/**\"
  - \"**/__tests__/**\"
  - \"**/mock/**\"

```

4. **Add code models for safe functions:**

```yaml
# Mark sanitization functions as safe
extensions:
  - addsTo:
      pack: codeql/java-queries
    data:
      - [\"com.company.utils\", \"Sanitizer\", \"sanitize\", \"ReturnValue\", \"safe\"]

```

**Process Improvements:**
5. **Require triage notes:**
- Every dismissal needs a reason
- Review dismissals weekly
6. **Report real false positives:**
- Submit to GitHub for query improvement
- Helps the whole community
**Developer Experience:**
7. **Only show high-confidence:**
- Start with `default` suite
- Add `security-extended` only for specific high-risk repos
8. **Integrate into workflow:**
- Show alerts in PR, not just Security tab
- Link to fix guidance
Want me to review your current configuration and suggest specific improvements?"

## Scenario 3: Compliance Evidence

### Situation
A customer is going through SOC 2 audit and needs to demonstrate security scanning controls.

### Customer Question
"Our auditors want evidence that we scan all code for vulnerabilities. How do we prove GHAS is working?"

### Your Response
"GHAS provides excellent compliance evidence. Here's what auditors typically need:
**1. Policy Evidence:**
- Screenshot of organization security settings
- Branch protection rules requiring CodeQL
- SECURITY.md file documenting policies
**2. Coverage Evidence:**

```bash
# Export repo list with security features
gh api /orgs/{org}/repos --paginate | \
  jq '.[] | {name, security: .security_and_analysis}'
# Or use Security Overview
Organization → Security → Security overview → Export CSV

```

**3. Detection Evidence:**
- Security Overview dashboard showing:
  - Total alerts by severity
  - Repositories scanned
  - Mean time to remediate
**4. Remediation Evidence:**
- Closed alerts showing timeline
- PRs fixing vulnerabilities
- Audit log of security events
**5. Continuous Monitoring Evidence:**

```yaml
# Scheduled scanning proves continuous monitoring
on:
  schedule:
    - cron: '0 4 * * *'  # Daily at 4 AM

```

**Report Generation:**

```bash
# Generate compliance report via API
gh api graphql -f query='
query {
  organization(login: \"your-org\") {
    repositories(first: 100) {
      nodes {
        name
        vulnerabilityAlerts { totalCount }
        codeScanning {
          alerts { totalCount }
        }
      }
    }
  }
}'

```

**For your SOC 2 specifically:**
- CC6.1 (Security): CodeQL + Secret Scanning
- CC6.6 (Processing Integrity): Dependency Review
- CC7.1 (Availability): Scanning doesn't block deploys
I can help you create a compliance package with screenshots and API exports if needed."
---

## Scenario 4: Secret Leak Incident

### Situation
A developer accidentally committed an AWS access key. It was detected by secret scanning but the customer doesn't know how to respond.

### Customer Question
"We just got an alert that AWS credentials were exposed in a commit. What do we do?"

### Your Response
"Let's move quickly. Here's your incident response checklist:
**Immediate (Next 15 minutes):**
1. **Rotate the credential NOW:**
   - Go to AWS IAM Console
   - Deactivate the exposed access key
   - Create a new access key
   - Update applications using the key
2. **Check for unauthorized usage:**
   - AWS CloudTrail: Look for unusual API calls
   - Check for new resources created
   - Review billing for anomalies
**Short-term (Next hour):**
3. **Remove from Git history:**

```bash
# Using BFG Repo Cleaner
bfg --replace-text passwords.txt repo.git
# Or git-filter-repo
git filter-repo --replace-text expressions.txt

```

4. **Force push cleaned history:**

```bash
git push origin --force --all
git push origin --force --tags

```

5. **Notify GitHub:**
- If partner secret, GitHub already notified AWS
- AWS may auto-revoke if their policy is configured
**Documentation:**
6. **Document the incident:**
- When was the secret committed?
- When was it detected?
- When was it rotated?
- Was there unauthorized access?
**Prevention:**
7. **Enable push protection:**
- Prevents this from happening again
- Settings → Code security → Push protection
8. **Add pre-commit hooks:**

```bash
# Use detect-secrets or similar
pip install detect-secrets
detect-secrets scan > .secrets.baseline

```

9. **Developer training:**
- Never commit secrets
- Use environment variables
- Use AWS IAM roles instead of access keys
Do you need help with any of these steps?"

## Module Summary

### Key Takeaways
1. **Shift Security Left**
   - Integrate security into development workflow
   - Catch issues before they reach production
   - Make security feedback fast and actionable
2. **CodeQL Provides Deep Analysis**
   - Semantic analysis finds complex vulnerabilities
   - Configure for your language and risk tolerance
   - Use query suites appropriate to your needs
3. **Secret Scanning is Essential**
   - Detects 200+ secret types automatically
   - Push protection prevents accidents
   - Partner program enables auto-revocation
4. **Dependabot Automates Updates**
   - Alerts notify about vulnerabilities
   - Security updates create fix PRs
   - Version updates keep dependencies current
5. **Phased Rollout Ensures Success**
   - Don't enable everything at once
   - Train developers first
   - Establish baselines before enforcement
6. **Metrics Drive Improvement**
   - Track MTTR, not just alert counts
   - Monitor false positive rates
   - Celebrate security wins
---

## What's Next
In **Module 7: Dependency & Package Management**, you'll learn about:
- GitHub Packages registry
- Container registry (GHCR)
- Package publishing workflows
- Dependency supply chain security
The security scanning you've learned here integrates directly with package management to create a secure software supply chain.

## Checklist: Module 6 Complete
- [ ] Enabled GHAS features at repository level
- [ ] Configured CodeQL with appropriate query suite
- [ ] Set up Dependabot with custom configuration
- [ ] Enabled secret scanning and push protection
- [ ] Triaged at least 5 security alerts
- [ ] Completed all three labs
- [ ] Reviewed real-world scenarios
{% endraw %}
