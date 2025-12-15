---
layout: training-module
title: "CSM Scenarios"
permalink: /advanced/module-11/scenarios/
module_number: 11
module_title: "Infrastructure as Code & Operations"
section_number: 7
total_sections: 7
phase: advanced
estimated_time: "20 min"
module_index: /advanced/module-11/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-11/overview/"
    short_title: "Overview"
    icon: "📋"
  - title: "Core Concepts"
    url: "/advanced/module-11/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/advanced/module-11/walkthrough/"
    short_title: "Walkthrough"
    icon: "🔄"
  - title: "Hands-On Labs"
    url: "/advanced/module-11/labs/"
    short_title: "Labs"
    icon: "🧪"
  - title: "Knowledge Check"
    url: "/advanced/module-11/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/advanced/module-11/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/advanced/module-11/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
prev_section:
  url: /advanced/module-11/resources/
  title: "Resources"
---

## Customer Success Scenarios

Practice handling real-world customer situations related to Infrastructure as Code for GitHub administration.

---

## Scenario 1: The Overwhelmed Platform Team

### Situation

**Customer:** FinanceFirst Corp (Enterprise, 2,000 developers, 800 repositories)

**Contact:** Sarah, Platform Engineering Lead

**Problem:** "Our team of 4 spends half their time just creating and configuring repositories. Developers wait 2-3 days for new repos. We can't scale."

### Discovery Questions

Before proposing solutions, gather more context:

<details>
<summary>Show Discovery Questions</summary>

1. "What does your current repository creation process look like?"
2. "What configuration do you apply to each repository? Is it consistent?"
3. "Does your team have experience with Infrastructure as Code tools like Terraform?"
4. "How do you currently handle branch protection and security settings?"
5. "What's your team's appetite for automation vs. manual control?"
</details>

### Recommended Response

<details>
<summary>Show Response Strategy</summary>

**Phase 1: Acknowledge & Empathize**
"That's a common challenge at your scale. Manual repository management doesn't scale beyond a few hundred repos, and your developers shouldn't wait days for something that could take minutes."

**Phase 2: Present the Solution**
"Infrastructure as Code, specifically Terraform with the GitHub provider, can transform this. Here's what it looks like:

1. **Standardized Templates**: Define your repository configuration once in code
2. **Self-Service Portal**: Developers submit a PR to request new repos
3. **Automated Provisioning**: CI/CD creates repos within minutes of approval
4. **Guaranteed Consistency**: Every repo has required security settings"

**Phase 3: Quantify the Impact**
"Organizations like yours typically see:
- Repository provisioning time: 2-3 days → 15 minutes
- Platform team time savings: 50%+ reduction in repo admin work
- Security compliance: 100% of repos meeting policy (vs. ~70% with manual)"

**Phase 4: Propose Next Steps**
"I'd recommend we:
1. Schedule a technical workshop to walk through the architecture
2. Identify 2-3 pilot projects to start with
3. Build a proof of concept with your standards baked in"
</details>

---

## Scenario 2: The Security Audit Failure

### Situation

**Customer:** HealthTech Solutions (Enterprise, HIPAA regulated)

**Contact:** Marcus, CISO

**Problem:** "We just failed a security audit. The auditor found 200 repositories without branch protection, and we have no documentation of when security settings were changed. We're facing compliance issues."

### Discovery Questions

<details>
<summary>Show Discovery Questions</summary>

1. "How are branch protection rules currently managed?"
2. "Do you have a documented security baseline for repositories?"
3. "How many repositories does the audit cover?"
4. "What's your timeline for remediation?"
5. "Do you have any existing automation or IaC in place?"
</details>

### Recommended Response

<details>
<summary>Show Response Strategy</summary>

**Phase 1: Acknowledge Urgency**
"Compliance issues are serious, especially in healthcare. Let's address both the immediate remediation and the long-term solution."

**Phase 2: Immediate Actions**
"For quick wins:
1. GitHub's API can audit all repos for branch protection status immediately
2. We can bulk-apply branch protection rules using scripts or Terraform
3. Enable required security features across all repos"

**Phase 3: Sustainable Solution**
"For ongoing compliance, Infrastructure as Code provides:
- **Audit Trail**: Every configuration change is a git commit with author, timestamp, and PR approval
- **Drift Detection**: Automated alerts when someone modifies settings outside the approved process
- **Continuous Compliance**: Scheduled checks ensure policy is always enforced
- **Documentation**: The code IS your documentation - always current"

**Phase 4: Audit Response**
"For your auditor, you'll be able to show:
- Git history of all security configuration changes
- Automated compliance checks running continuously
- Alerts and remediation when drift occurs
- Clear ownership and approval process for all changes"
</details>

---

## Scenario 3: The Skeptical Architect

### Situation

**Customer:** TechStartup Inc (500 developers)

**Contact:** James, Principal Architect

**Objection:** "We already have scripts that manage our GitHub config. Why would we add Terraform complexity? Seems like overkill."

### Handling the Objection

<details>
<summary>Show Response Strategy</summary>

**Acknowledge Their Investment**
"Scripts absolutely work, and it sounds like your team has built something useful. Let me understand - are these bash scripts calling the API, or something more structured?"

**Identify Pain Points (if they exist)**
Ask about:
- How do you handle errors mid-execution?
- What happens if someone runs a script twice?
- How do you know what's deployed vs. what's defined?
- How do new team members learn what's configured?

**Present Terraform's Differentiators**
"Where Terraform adds value over scripts:

| Scripts | Terraform |
|---------|-----------|
| Imperative (do steps) | Declarative (desired state) |
| Run twice = problems | Idempotent - safe to re-run |
| No state tracking | State tracks what exists |
| Manual drift detection | Built-in plan/diff |
| Tribal knowledge | Self-documenting |

The key difference: scripts tell GitHub *what to do*, Terraform tells GitHub *what should exist*. Terraform figures out the steps."

**Meet Them Where They Are**
"If your scripts work well, that's great. Consider Terraform when:
- You need to know if actual config matches intended config
- Team members beyond the script author need to make changes
- Audit requirements demand change documentation
- You want to prevent accidental destructive changes"
</details>

---

## Scenario 4: The Multi-Org Challenge

### Situation

**Customer:** GlobalManufacturing (Enterprise, 5 GitHub orgs, 3,000+ repos)

**Contact:** Lin, DevOps Director

**Problem:** "We have 5 GitHub organizations due to acquisitions. Each has different standards. We need to consolidate and standardize, but don't know where to start."

### Recommended Approach

<details>
<summary>Show Response Strategy</summary>

**Phase 1: Discovery & Assessment**
"Before consolidating, let's understand what you have:
1. Inventory all organizations and their configurations
2. Map teams to repositories across orgs
3. Identify common patterns and unique requirements
4. Document current security baselines per org"

**Phase 2: Define Target State**
"Create a unified standard:
- Repository naming conventions
- Team structure and access model
- Branch protection baseline
- Security settings requirements
- Allowed/required features per repo type"

**Phase 3: Terraform Strategy**
"With Terraform, you can:
1. **Multi-Provider Configuration**: Single codebase managing all 5 orgs
2. **Consistent Modules**: Same standards applied everywhere
3. **Incremental Migration**: Import existing repos org by org
4. **Parallel Management**: Run old and new in parallel during transition"

```hcl
# Example: Multi-org Terraform structure
provider "github" {
  alias = "org_alpha"
  owner = "company-alpha"
}

provider "github" {
  alias = "org_beta"
  owner = "company-beta"
}

module "standard_repo_alpha" {
  source    = "./modules/standard-repo"
  providers = { github = github.org_alpha }
  # ...
}

```

**Phase 4: Migration Path**
1. Start with one org as pilot
2. Import existing resources
3. Standardize configuration
4. Replicate to remaining orgs
5. (Optional) Consolidate orgs if desired
</details>

---

## Scenario 5: The Self-Service Request

### Situation

**Customer:** CloudNative Corp (Enterprise, fast-growing)

**Contact:** Alex, VP Engineering

**Request:** "We're growing fast - 50 new developers per quarter. I want developers to be able to spin up their own repos without waiting for platform team approval, but still maintain standards."

### Solution Design

<details>
<summary>Show Response Strategy</summary>

**Validate the Goal**
"Self-service with guardrails - that's exactly what modern platform teams aim for. Developers get speed, you get consistency."

**Propose Architecture**

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph SelfService["Self-Service Repository Creation"]
    subgraph Dev["Developer"]
      YAML["📝 Creates<br/>YAML Request"]
    end
    
    YAML -->|"PR"| Review
    
    subgraph Platform["Platform Team"]
      Review["👀 Reviews<br/>(optional)"]
    end
    
    Review --> TF["⚙️ Terraform<br/>Apply"]
    TF --> Repo["✅ Repository<br/>Created<br/>(standard config)"]
  end
</div>
</div>

**Guardrails to Implement**
1. **YAML Validation**: CI checks request format before allowing merge
2. **Naming Convention**: Regex validation on repository names
3. **Team Assignment**: Must specify valid owning team
4. **Visibility Limits**: Only private/internal allowed without approval
5. **Template Requirement**: Must use approved template
6. **Auto-Approval for Standard**: Fast path for compliant requests
7. **Manual Review for Exceptions**: Public repos, special permissions

**Workflow Example**

```yaml
# Developer creates: requests/my-new-service.yaml
name: my-new-service
description: New backend service for payments
team: payments-team
template: backend-service-template
visibility: private

```

"Merge → Terraform applies → Repo exists in minutes"

**Metrics to Track**
- Time to repository creation (target: <10 minutes)
- Percentage self-service vs. manual
- Policy compliance rate
- Developer satisfaction score
</details>

---

## Role Play Exercise

Practice these scenarios with a colleague:

1. **You are the CSM**, partner plays the customer
2. Work through discovery questions
3. Present your recommendation
4. Handle follow-up questions and objections

### Evaluation Criteria

| Criterion | What to Look For |
|-----------|------------------|
| Discovery | Asked questions before proposing solutions |
| Technical Accuracy | Correct understanding of IaC concepts |
| Business Value | Connected features to business outcomes |
| Actionable | Provided clear next steps |
| Objection Handling | Addressed concerns without being dismissive |

---

## Module Complete! 🎉

You've completed Module 11: Infrastructure as Code & Operations.

**Key Takeaways:**
- IaC transforms GitHub administration from manual to automated
- Terraform provides declarative, version-controlled configuration
- GitOps practices enable self-service with guardrails
- Drift detection ensures compliance is maintained

**Next Module:** [Module 12: Governance, Metrics & DORA](/advanced/module-12/)
