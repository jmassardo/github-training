---
layout: training-module
title: "Knowledge Check"
permalink: /advanced/module-12/quiz/
module_number: 12
module_title: "Governance, Compliance & DORA Metrics"
section_number: 5
total_sections: 7
phase: advanced
estimated_time: "15 min"
module_index: /advanced/module-12/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-12/overview/"
    short_title: "Overview"
    icon: "📋"
  - title: "Core Concepts"
    url: "/advanced/module-12/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/advanced/module-12/walkthrough/"
    short_title: "Walkthrough"
    icon: "🔄"
  - title: "Hands-On Labs"
    url: "/advanced/module-12/labs/"
    short_title: "Labs"
    icon: "🧪"
  - title: "Knowledge Check"
    url: "/advanced/module-12/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/advanced/module-12/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/advanced/module-12/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
prev_section:
  url: /advanced/module-12/labs/
  title: "Hands-On Labs"
next_section:
  url: /advanced/module-12/resources/
  title: "Resources"
---

## Knowledge Check

Test your understanding of governance, compliance, and DORA metrics.

---

### Question 1

**What are the four DORA metrics?**

<details>
<summary>Show Answer</summary>

The four DORA (DevOps Research and Assessment) metrics are:

1. **Deployment Frequency** - How often code is deployed to production
2. **Lead Time for Changes** - Time from code commit to production
3. **Mean Time to Recovery (MTTR)** - How quickly service is restored after an incident
4. **Change Failure Rate** - Percentage of deployments causing failures

These metrics predict both organizational performance AND team well-being. Elite performers score well on all four simultaneously.
</details>

---

### Question 2

**A customer says: "We deploy less frequently to reduce failures." What does DORA research say about this approach?**

<details>
<summary>Show Answer</summary>

**DORA research proves this is wrong.** 

The research shows that elite performers have BOTH:
- Higher deployment frequency (multiple deploys per day)
- Lower change failure rates (0-15%)

This works because:
- Small, frequent changes are easier to test and review
- Fast feedback loops catch problems quickly
- Automated pipelines are more reliable than manual processes
- Teams that deploy often build robust recovery capabilities

The key insight: **Speed and stability are not tradeoffs—they reinforce each other.**
</details>

---

### Question 3

**What is "lead time for changes" in DORA metrics?**

A) Time from feature request to deployment  
B) Time from code commit to production deployment  
C) Time from PR creation to merge  
D) Time from deployment to customer feedback

<details>
<summary>Show Answer</summary>

**B) Time from code commit to production deployment**

Lead time measures the efficiency of your delivery pipeline—from when a developer commits code until it's running in production.

Benchmarks:
- **Elite**: Less than one hour
- **High**: Less than one week
- **Medium**: One to six months
- **Low**: More than six months

Note: Some organizations measure from PR creation to merge, which captures part of the lead time but misses the deployment phase.
</details>

---

### Question 4

**What is the difference between Enterprise policies and Organization rulesets?**

<details>
<summary>Show Answer</summary>

| Aspect | Enterprise Policies | Organization Rulesets |
|--------|--------------------|-----------------------|
| **Scope** | All organizations in enterprise | Single organization |
| **Override** | Cannot be overridden | Can be customized within limits |
| **Purpose** | Security baseline, hard requirements | Flexible branch protection |
| **Examples** | 2FA requirement, Actions restrictions | PR review requirements, status checks |

**Enterprise policies** set the floor—minimum requirements across all organizations.

**Organization rulesets** allow customization within those limits, such as requiring more reviewers for specific branches.
</details>

---

### Question 5

**What is "Goodhart's Law" and why is it relevant to engineering metrics?**

<details>
<summary>Show Answer</summary>

**Goodhart's Law**: "When a measure becomes a target, it ceases to be a good measure."

**Relevance to engineering**:

If you measure and reward:
- **Lines of code** → Developers write verbose, redundant code
- **Commits per day** → Small, meaningless commits
- **PR merge time** → Quick approvals without real review
- **Story points** → Point inflation

**Mitigation strategies**:
1. Use multiple metrics in combination
2. Focus on trends, not absolute numbers
3. Measure outcomes, not outputs
4. Involve teams in metric selection
5. Keep some metrics private to teams
6. Review and adjust metrics regularly
</details>

---

### Question 6

**A customer needs to demonstrate compliance for a SOC 2 audit. What GitHub features help?**

<details>
<summary>Show Answer</summary>

**Key GitHub features for SOC 2 compliance:**

| SOC 2 Requirement | GitHub Feature |
|-------------------|----------------|
| Access Controls | 2FA, SSO/SAML, Team-based permissions |
| Change Management | Branch protection, required reviews |
| Audit Trail | Audit log, audit log streaming |
| Segregation of Duties | CODEOWNERS, required reviewers |
| Monitoring | Security alerts, Dependabot |

**Documentation artifacts**:
- Audit log exports showing all administrative actions
- Branch protection configuration evidence
- Security scanning reports
- Access review documentation from SSO integration
</details>

---

### Question 7

**What is "drift" in the context of GitHub governance?**

<details>
<summary>Show Answer</summary>

**Drift** occurs when the actual configuration differs from the intended/documented configuration.

**Examples in GitHub**:
- Branch protection disabled manually via UI
- Repository made public unexpectedly
- Team permissions changed outside normal process
- Security features disabled

**Detecting drift**:
- Scheduled compliance checks comparing API state to baseline
- Audit log monitoring for unexpected changes
- Infrastructure as Code with drift detection (Terraform)

**Preventing drift**:
- Enterprise policies (can't be overridden)
- Automated remediation workflows
- Alert on audit log events
</details>

---

### Question 8

**What's the difference between "vanity metrics" and DORA metrics?**

<details>
<summary>Show Answer</summary>

| Vanity Metrics ❌ | DORA Metrics ✅ |
|------------------|-----------------|
| Lines of code | Deployment Frequency |
| Commits per developer | Lead Time for Changes |
| Story points velocity | Mean Time to Recovery |
| Hours worked | Change Failure Rate |
| PRs created | - |

**Why vanity metrics fail**:
- Easy to game
- Don't correlate with outcomes
- Can encourage harmful behavior
- Measure activity, not value

**Why DORA metrics work**:
- Research-validated correlation with performance
- Measure outcomes that matter
- Balance speed with stability
- Harder to game (improving one helps others)
</details>

---

### Question 9

**An organization wants to implement self-service repository creation while maintaining governance. What's the recommended approach?**

<details>
<summary>Show Answer</summary>

**Recommended self-service pattern:**

```
Developer Request → Automated Validation → Provisioning
                         ↓
              Governance Guardrails Enforced

```

**Implementation**:
1. **Template-based creation**: Repos created from approved templates with settings pre-configured
2. **PR-based requests**: Developers submit YAML files, automation provisions
3. **Rulesets**: Organization rulesets automatically apply to new repos
4. **Naming conventions**: Validation ensures consistent naming
5. **Team assignment**: Required field ensures ownership

**Guardrails**:
- Branch protection applied automatically
- Security scanning enabled by default
- Default branch named consistently
- Topics/metadata required
- Visibility restricted to private/internal
</details>

---

### Question 10

**Match the compliance framework to its primary GitHub relevance:**

| Framework | Primary Concern |
|-----------|-----------------|
| SOC 2 | ? |
| HIPAA | ? |
| PCI-DSS | ? |
| GDPR | ? |

<details>
<summary>Show Answer</summary>

| Framework | Primary Concern |
|-----------|-----------------|
| **SOC 2** | Security controls, audit logging, change management |
| **HIPAA** | Protected Health Information (PHI), access auditing |
| **PCI-DSS** | Cardholder data protection, code review requirements |
| **GDPR** | Personal data handling, right to deletion |

**GitHub relevance by framework**:
- **SOC 2**: Most comprehensive—audit logs, access controls, branch protection
- **HIPAA**: Access logging, encryption, breach notification
- **PCI-DSS**: Required code reviews, access restrictions
- **GDPR**: Data residency (GitHub Server), deletion capabilities
</details>

---

## Score Yourself

| Score | Assessment |
|-------|------------|
| 9-10 | Excellent! Ready for governance and metrics discussions |
| 7-8 | Good foundation, review weak areas |
| 5-6 | Revisit Core Concepts before customer conversations |
| <5 | Complete the module again before proceeding |

---

**Next:** Explore additional Resources for deeper learning.
