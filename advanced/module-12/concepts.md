---
layout: training-module
title: "Core Concepts"
permalink: /advanced/module-12/concepts/
module_number: 12
module_title: "Governance, Compliance & DORA Metrics"
section_number: 2
total_sections: 7
phase: advanced
estimated_time: "30 min"
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
  url: /advanced/module-12/overview/
  title: "Context & Overview"
next_section:
  url: /advanced/module-12/walkthrough/
  title: "Guided Walkthrough"
---

## Enterprise Governance Framework

### The Governance Pyramid

Effective GitHub governance operates at three levels:

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Pyramid["Governance Hierarchy"]
    E["🏛️ ENTERPRISE<br/>Policies<br/>(Enforced)"]
    
    E --> O1 & O2
    
    O1["🏢 ORGANIZATION<br/>Policies<br/>(Configurable)"]
    O2["🏢 ORGANIZATION<br/>Policies<br/>(Configurable)"]
    
    O1 --> R1 & R2 & R3
    O2 --> R4
    
    R1["📁 Repo<br/>Rules"]
    R2["📁 Repo<br/>Rules"]
    R3["📁 Repo<br/>Rules"]
    R4["📁 Repo<br/>Rules"]
  end
</div>
</div>

### Enterprise-Level Policies

Set at the enterprise level, these policies cannot be overridden:

| Policy | Options | Recommendation |
|--------|---------|----------------|
| Base repository permissions | None, Read, Write, Admin | **Read** (least privilege) |
| Repository creation | All members, Admins only | **Admins** for enterprise |
| Repository forking | Enabled, Disabled | Context-dependent |
| Actions permissions | All, Local only, Disabled | **Local + approved** |
| Default branch name | Any string | **main** |

### Organization-Level Governance

Organizations can configure (within enterprise limits):

```yaml
# Recommended organization settings
Member privileges:
  - Repository creation: Disabled (or template-based)
  - Team creation: Enabled (with naming convention)
  - Outside collaborators: Require 2FA

Security:
  - 2FA requirement: Enforced
  - SSH certificate authorities: Configured
  - IP allow lists: Enabled for sensitive orgs

Actions:
  - Allowed actions: Actions in this enterprise
  - Fork PR workflows: Require approval
  - Default permissions: Read

```

### Repository Rulesets

Rulesets provide consistent protection across repositories:

```yaml
# Example: Production branch ruleset
name: Production Branches
target: branch
conditions:
  ref_name:
    include: ["refs/heads/main", "refs/heads/release/*"]
rules:
  - type: required_pull_request
    parameters:
      required_approving_review_count: 2
      dismiss_stale_reviews_on_push: true
      require_code_owner_review: true
      require_last_push_approval: true
  - type: required_status_checks
    parameters:
      strict_required_status_checks_policy: true
      required_status_checks:
        - context: "ci/build"
        - context: "security/scan"
  - type: required_signatures
  - type: non_fast_forward
enforcement: active

```

## Compliance Frameworks

### Common Compliance Requirements

| Framework | GitHub Relevance |
|-----------|------------------|
| **SOC 2** | Access controls, audit logging, change management |
| **HIPAA** | PHI protection, access auditing, encryption |
| **PCI-DSS** | Code review requirements, access controls |
| **FedRAMP** | Government cloud requirements |
| **GDPR** | Data handling, right to deletion |

### GitHub Audit Log

The audit log captures all administrative and security-relevant events:

```json
{
  "@timestamp": "2024-01-15T10:30:00Z",
  "action": "repo.update",
  "actor": "admin-user",
  "actor_location": {
    "country_code": "US"
  },
  "org": "my-organization",
  "repo": "my-organization/sensitive-repo",
  "data": {
    "visibility": "private"
  }
}

```

**Key audit log categories:**
- `repo.*` - Repository operations
- `org.*` - Organization settings
- `team.*` - Team management
- `protected_branch.*` - Branch protection changes
- `integration.*` - App and OAuth events

### Compliance Reporting Pattern

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph Pipeline["Compliance Reporting Pipeline"]
    GH["📋 GitHub<br/>Audit Log"] --> Stream["📤 Stream<br/>Export"]
    Stream --> Storage["💾 Storage<br/>(S3/Azure)"]
    Storage --> Query["🔍 Query<br/>& Analyze"]
    Query --> Process["⚙️ Process<br/>& Alert"]
    Process --> SIEM["📊 SIEM<br/>(Splunk/etc)"]
  end
</div>
</div>

## DORA Metrics Deep Dive

### Metric 1: Deployment Frequency

**Definition**: How often code is successfully deployed to production

**Measurement in GitHub**:
- Count of deployments via GitHub Actions
- Releases created per time period
- Merged PRs to main/production branch

```yaml
# Example: Track deployments in GitHub Actions
- name: Record Deployment
  uses: actions/github-script@v7
  with:
    script: |
      await github.rest.repos.createDeployment({
        owner: context.repo.owner,
        repo: context.repo.repo,
        ref: context.sha,
        environment: 'production',
        auto_merge: false
      });

```

**Benchmarks**:
- Elite: Multiple deploys per day
- High: Between once per week and once per month
- Medium: Between once per month and once every 6 months
- Low: Fewer than once every 6 months

### Metric 2: Lead Time for Changes

**Definition**: Time from code committed to code running in production

**Measurement in GitHub**:
- Time from first commit in PR to merge
- Time from merge to deployment
- Total time from commit to production

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph LeadTime["Lead Time for Changes"]
    C["📝 Commit<br/>Created"] --> PR["🔀 PR<br/>Merged"] --> D["🚀 Production<br/>Deploy"]
  end
</div>
</div>

**Benchmarks**:
- Elite: Less than one hour
- High: Between one day and one week
- Medium: Between one month and six months
- Low: More than six months

### Metric 3: Mean Time to Recovery (MTTR)

**Definition**: How quickly you can restore service after an incident

**Measurement in GitHub**:
- Time from incident issue creation to resolution
- Time from bug report to hotfix deployed
- Rollback deployment time

```yaml
# Track incidents with labels
Incident Opened: 2024-01-15 10:00:00
Incident Closed: 2024-01-15 10:45:00
MTTR: 45 minutes

```

**Benchmarks**:
- Elite: Less than one hour
- High: Less than one day
- Medium: Less than one week
- Low: More than six months

### Metric 4: Change Failure Rate

**Definition**: Percentage of deployments causing a failure in production

**Measurement in GitHub**:
- Rollback deployments / Total deployments
- Hotfix PRs / Total PRs
- Issues labeled "production-incident" / Deployments

```
Change Failure Rate = (Failed Deployments / Total Deployments) × 100

Example:
  100 deployments this month
  8 required rollback or hotfix
  CFR = 8%  (Elite performance)

```

**Benchmarks**:
- Elite: 0-15%
- High: 16-30%
- Medium: 16-30%
- Low: 46-60%

## Value Stream Mapping

### What is Value Stream Mapping?

Value stream mapping visualizes the flow of work from idea to production, identifying:
- Wait times (waste)
- Process bottlenecks
- Automation opportunities

### Software Development Value Stream

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph ValueStream["Software Development Value Stream"]
    I["💡 Idea<br/>────<br/>Process: 2d<br/>Wait: 5d"] --> D["📐 Design<br/>────<br/>Process: 3d<br/>Wait: 2d"]
    D --> C["💻 Code<br/>────<br/>Process: 5d<br/>Wait: 0d"]
    C --> R["👀 Review<br/>────<br/>Process: 2d<br/>Wait: 3d"]
    R --> T["🧪 Test<br/>────<br/>Process: 3d<br/>Wait: 2d"]
    T --> DP["🚀 Deploy<br/>────<br/>Process: 0.5d<br/>Wait: 1d"]
  end
  
  subgraph Stats["Summary"]
    S1["📊 Total Process: 15.5 days"]
    S2["⏳ Total Wait: 13 days"]
    S3["📈 Efficiency: 54%"]
    S4["🎯 Opportunity: Reduce wait in Review & Test"]
  end
</div>
</div>

### GitHub Metrics for Value Stream

| Phase | GitHub Data Sources |
|-------|-------------------|
| Idea → Design | Issue creation to assignment time |
| Design → Code | Issue assignment to first commit |
| Code → Review | PR creation to first review |
| Review → Merge | First review to merge |
| Merge → Deploy | Merge to deployment event |

## Innersource Metrics

### Measuring Cross-Team Collaboration

Innersource success indicators:

| Metric | Definition | Target |
|--------|-----------|--------|
| **External Contributors** | PRs from outside the owning team | Increasing trend |
| **Documentation Quality** | README completeness, contribution guides | 100% repos have CONTRIBUTING.md |
| **Response Time** | Time to first response on external PRs | < 2 business days |
| **Merge Rate** | % of external PRs merged | > 70% |
| **Discoverability** | Repos with topics, descriptions | 100% |

### Cross-Pollination Tracking

```
External Contributions by Team:

Team Alpha ──────────────▶ 45 PRs to other teams
Team Beta ───────────────▶ 32 PRs to other teams
Team Gamma ──────────────▶ 67 PRs to other teams
Platform ────────────────▶ 12 PRs to other teams (support focus)

Receiving Teams:
Shared Libraries ◀──────── 89 external PRs
Platform Tools ◀─────────── 34 external PRs
Documentation ◀──────────── 23 external PRs

```

## Building Metrics Programs

### Anti-Patterns to Avoid

| Anti-Pattern | Why It's Harmful | Better Approach |
|--------------|------------------|-----------------|
| Measuring individuals | Creates competition, gaming | Measure teams |
| Lines of code | Rewards verbosity, punishes refactoring | Outcome metrics |
| Commits per day | Encourages small, meaningless commits | Lead time, deployment frequency |
| PR merge time only | Teams game by approving quickly | Include quality metrics |
| Public rankings | Shame-based culture | Private team feedback |

### Goodhart's Law

> "When a measure becomes a target, it ceases to be a good measure."

**Mitigation strategies:**
- Use multiple metrics in combination
- Focus on trends, not absolute numbers
- Measure outcomes, not outputs
- Involve teams in metric selection
- Review and adjust metrics regularly

### Metrics Maturity Model

| Level | Characteristics |
|-------|-----------------|
| **1 - None** | No systematic measurement |
| **2 - Ad Hoc** | Manual reporting, inconsistent |
| **3 - Defined** | Consistent metrics, regular reporting |
| **4 - Managed** | Automated collection, dashboards |
| **5 - Optimizing** | Predictive analytics, continuous improvement |

---

**Next:** Learn how to implement these concepts in the Guided Walkthrough.
