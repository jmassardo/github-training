---
layout: training-module
title: "CSM Scenarios"
permalink: /beginner/module-4/scenarios/
module_number: 4
module_title: "SCM Process"
section_number: 7
total_sections: 7
phase: beginner
estimated_time: "25 min"
module_index: /beginner/module-4/
sections:
  - title: "Context & Overview"
    url: "/beginner/module-4/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/beginner/module-4/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/beginner/module-4/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/beginner/module-4/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/beginner/module-4/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/beginner/module-4/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/beginner/module-4/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
prev_section:
  url: /beginner/module-4/resources/
  title: "Resources"
---

# Real-World CSM Scenarios

## Scenario 1: The Compliance Audit

**Situation:**
A financial services customer is preparing for SOC 2 audit. The auditor requires evidence that:
- All production code is reviewed before deployment
- Developers cannot merge their own code
- All commits are attributable to verified identities
- There's an audit trail of all changes

**Customer Question:**
"How do we configure GitHub to meet these compliance requirements?"

**Your Response:**
"Let me walk you through a compliance-ready SCM configuration:

1. **Code Review Requirement:**
   - Branch protection requiring 2 approvals on main
   - Enable 'Dismiss stale reviews when new commits are pushed'
   - Enable 'Require review from CODEOWNERS'
   - Implement 'Require approval of most recent reviewable push' (prevents self-approval of follow-up commits)

2. **Separation of Duties:**
   - 'Require approval of most recent reviewable push' prevents merging your own code
   - CODEOWNERS ensures appropriate team reviews each area
   - Consider enabling 'Restrict who can push to matching branches'

3. **Identity Verification:**
   - Enable 'Require signed commits'
   - Configure SSO with your identity provider
   - Use vigilant mode for key personnel
   - Audit log captures all authentication events

4. **Audit Trail:**
   - GitHub's audit log (Enterprise) captures all administrative actions
   - Git history preserves complete change history
   - Pull request conversations document review decisions
   - Export audit logs to SIEM for long-term retention

For your SOC 2 evidence, I can help you generate reports showing these controls are in place across all production repositories using our API."

## Scenario 2: The Monorepo Migration

**Situation:**
A customer is consolidating 50 microservice repositories into a single monorepo. Different teams own different services, and they need to maintain existing ownership and review requirements.

**Customer Question:**
"How do we manage code ownership when everything is in one repository?"

**Your Response:**
"CODEOWNERS is designed exactly for this use case. Here's how to structure it:

```
# .github/CODEOWNERS for monorepo

# Global ownership (platform/infrastructure team)
*                               @platform-team

# Service-specific ownership
/services/user-auth/            @auth-team
/services/payments/             @payments-team @security-team
/services/inventory/            @inventory-team
/services/shipping/             @logistics-team

# Shared libraries require multiple approvals
/libs/common/                   @platform-team @tech-leads
/libs/security/                 @security-team

# Cross-cutting concerns
*.proto                         @api-standards-team
*.sql                           @database-team
/infrastructure/                @platform-team @security-team
/.github/                       @platform-team

```

Additionally:
- Create repository rulesets for different path patterns if needed
- Use GitHub Teams that mirror your service teams
- Consider CODEOWNERS validation in CI to catch syntax errors
- Document ownership in each service's README

One thing to watch: PR reviewers can get overwhelming in monorepos. Consider using review assignment rules to rotate reviewers within teams rather than requesting the entire team every time."

## Scenario 3: The Open Source Governance

**Situation:**
A customer is open-sourcing an internal project. They want to accept community contributions while maintaining quality and security standards.

**Customer Question:**
"How do we protect our main branch from potentially malicious or low-quality community contributions?"

**Your Response:**
"Here's an open source governance model I recommend:

**Branch Protection Configuration:**

```yaml
Main branch:
  - Require PR from fork (community can't push branches)
  - Require 1 maintainer approval
  - Require status checks:
    - CI/build
    - CI/test
    - License check
    - DCO (Developer Certificate of Origin)
  - Require signed commits (for maintainers)
  - Block force pushes

```

**CODEOWNERS for Maintainer Control:**

```
# All changes need maintainer review
*                    @org/project-maintainers

# Security-sensitive areas need security review
/auth/               @org/security-maintainers
/.github/workflows/  @org/security-maintainers

```

**Additional Protections:**

1. **Workflow Permissions:**
   - Require approval for first-time contributors' workflows
   - Limit GITHUB_TOKEN permissions in workflows
   
2. **Issue and PR Templates:**
   - Require bug reports to use template
   - PR template includes DCO acknowledgment

3. **Automated Checks:**
   - License compliance checking
   - Dependency vulnerability scanning
   - Code quality gates

4. **Community Guidelines:**
   - CONTRIBUTING.md with clear expectations
   - CODE_OF_CONDUCT.md
   - SECURITY.md for vulnerability reporting

This gives you maintainer control while being welcoming to contributors."

## Scenario 4: The Enterprise Standardization

**Situation:**
A large enterprise customer with 2,000 repositories wants to enforce consistent security policies across all projects, but different business units have different requirements.

**Customer Question:**
"How do we balance enterprise-wide standards with business unit flexibility?"

**Your Response:**
"This is where the layered ruleset model shines. Here's a tiered approach:

**Tier 1: Enterprise Non-Negotiables (Org Rulesets)**

```yaml
Ruleset: Enterprise Security Baseline
Target: All repositories
Enforcement: Active (no bypass)

Rules:
  - Block force pushes to default branch
  - Require signed commits
  - Require pull requests
  - Require at least 1 approval

```

**Tier 2: Compliance-Specific (Org Rulesets by Team)**

```yaml
Ruleset: PCI Compliance
Target: Repositories with topic 'pci-scope'
Enforcement: Active

Rules:
  - Require 2 approvals
  - Require CODEOWNERS review
  - Require security/scan status check

```

**Tier 3: Team Standards (Repository Rulesets)**

```yaml
# Each team can add rules on top
Ruleset: Frontend Team Standards
Rules:
  - Require lint/eslint status check
  - Require test/coverage status check

```

**Implementation Approach:**

1. **Tag repositories by classification:**
   - Use topics: `pci-scope`, `hipaa-scope`, `internal`, `public`
   - Rulesets target by topic pattern

2. **Create a compliance framework:**
   - Document which rulesets apply to which classifications
   - Automate topic assignment based on repo metadata

3. **Enable monitoring:**
   - Use audit log to detect non-compliant repos
   - Create dashboards showing compliance status
   - Alert when new repos are created without proper classification

4. **Provide self-service:**
   - Teams can add stricter rules but can't weaken enterprise baseline
   - Clear documentation on how to request exceptions

Would you like me to help you map your current security requirements to this tiered model?"

---

## Module Summary

### Key Takeaways

1. **Branch Protection is Your First Line of Defense**
   - Require PRs, reviews, and status checks
   - Consider signed commits for identity verification
   - Use linear history for cleaner audit trails

2. **CODEOWNERS Automates Review Assignment**
   - Last matching pattern wins
   - Use teams, not individuals
   - Combine with required CODEOWNER approval in branch protection

3. **Rulesets Scale Better Than Branch Protection**
   - Organization rulesets for enterprise-wide policies
   - Granular bypass permissions
   - Can target multiple branches and repositories

4. **Commit Signing Builds Trust**
   - SSH signing is easier to set up
   - GPG signing is more widely supported
   - Vigilant mode adds personal accountability

5. **Release Management Provides Structure**
   - Semantic versioning communicates change impact
   - Auto-generated notes save time
   - Release workflows ensure consistency

### What's Next

In **Module 5: GitHub Actions & Automation**, you'll learn how to:
- Create CI/CD workflows
- Automate code quality checks
- Build custom automation
- Implement the status checks your protection rules require

The SCM processes you've learned here work best when combined with automation. Status checks become meaningful when backed by actual CI pipelines, and release workflows are most powerful when automated.

---

**Checklist: Module 4 Complete**
- [ ] Configured branch protection rules
- [ ] Created a CODEOWNERS file
- [ ] Set up repository or organization rulesets
- [ ] Enabled commit signing
- [ ] Created a GitHub release
- [ ] Completed all three labs
- [ ] Reviewed real-world scenarios
