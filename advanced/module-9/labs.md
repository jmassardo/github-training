---
layout: training-module
title: "Hands-On Labs"
permalink: /advanced/module-9/labs/
module_number: 9
module_title: "Enterprise Administration"
section_number: 4
total_sections: 7
phase: advanced
estimated_time: "45 min"
module_index: /advanced/module-9/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-9/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/advanced/module-9/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/advanced/module-9/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/advanced/module-9/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/advanced/module-9/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/advanced/module-9/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/advanced/module-9/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
prev_section:
  url: /advanced/module-9/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /advanced/module-9/quiz/
  title: "Knowledge Check"
---

# Hands-On Labs

## Lab 9.1: Design an Enterprise Organizational Structure

**Objective:** Create an organizational design for a fictional enterprise.

**Scenario:** TechCorp is a technology company with 500 developers across:
- Platform team (50 people) - builds shared infrastructure
- Product teams (300 people) - 4 product lines
- Data Science (100 people) - ML and analytics
- IT/Security (50 people) - internal tools and security

### Tasks

**1. Design Organization Structure:**

```markdown
## TechCorp GitHub Enterprise Design

### Organizations
1. techcorp-platform
   - Purpose: 
   - Visibility: 
   - Key repositories:
   
2. techcorp-products
   - Purpose:
   - Visibility:
   - Key repositories:

[Continue for all orgs...]

### Team Structure
[Design team hierarchy within each org]

### Cross-Org Collaboration
[How will teams collaborate across orgs?]

```

**2. Define Enterprise Policies:**

```markdown
## Enterprise Policy Recommendations

### Repository Policies
- Default visibility:
- Fork policy:
- Base permissions:

### Security Policies
- Required 2FA:
- SAML enforcement:
- Outside collaborators:

### Actions Policies
- Allowed actions:
- Self-hosted runners:

```

**3. Create RACI Matrix:**

| Activity | Enterprise Owner | Org Owner | Team Maintainer | Developer |
|----------|-----------------|-----------|-----------------|-----------|
| Create organization | | | | |
| Add org member | | | | |
| Create repository | | | | |
| Configure branch protection | | | | |
| Approve Actions workflow | | | | |

---

## Lab 9.2: Configure SSO Test Environment

**Objective:** Document SSO configuration for a customer.

### Tasks

**1. Create Configuration Documentation:**

```markdown
## SSO Configuration Guide for [Customer]

### Prerequisites
- [ ] Enterprise admin access
- [ ] IdP admin access
- [ ] Recovery codes stored securely

### IdP Configuration

#### Application Settings
- Name: 
- Entity ID: 
- ACS URL:
- Attributes to map:

#### Group Mappings

| IdP Group | GitHub Team |
|-----------|-------------|
| | |

### GitHub Configuration
[Detailed steps]

### Testing Checklist
- [ ] Test user can authenticate
- [ ] Attributes pass correctly
- [ ] Team membership syncs
- [ ] Deprovisioning works

### Rollback Plan
[Document how to disable SSO if needed]

```

**2. Create Troubleshooting Guide:**

```markdown
## SSO Troubleshooting Guide

### Common Issues

#### Issue: User cannot authenticate
- Symptoms:
- Possible causes:
- Resolution steps:

#### Issue: Team membership not syncing
- Symptoms:
- Possible causes:
- Resolution steps:

[Continue for common scenarios...]

```

---

## Lab 9.3: Audit Log Analysis

**Objective:** Analyze audit logs to answer compliance questions.

**Scenario:** A security team needs to answer:
1. Who accessed repository X in the last 30 days?
2. What repositories were created last month?
3. Were there any failed authentication attempts?
4. What changes were made to branch protection rules?

### Tasks

**1. Write GraphQL Queries:**

```graphql
# Query 1: Repository access
query RepoAccess {
  # Your query here
}

# Query 2: Repository creation
# Query 3: Failed authentications
# Query 4: Branch protection changes

```

**2. Create Audit Report Template:**

```markdown
## Monthly Security Audit Report

### Report Period: [Date Range]

### Authentication Summary
- Total logins:
- Failed attempts:
- Accounts locked:

### Repository Activity
- Repositories created:
- Repositories deleted:
- Visibility changes:

### Access Changes
- Users added:
- Users removed:
- Permission escalations:

### Security Events
- Branch protection changes:
- Secret exposures:
- Policy violations:

### Recommendations
[Based on findings]

```

---

## Lab 9.4: Migration Planning

**Objective:** Create a migration plan for a GitLab to GitHub migration.

**Scenario:** A customer is migrating from GitLab to GitHub Enterprise Cloud:
- 200 repositories
- 5 GitLab groups → GitHub organizations
- Existing CI/CD pipelines in GitLab CI
- Need to maintain git history

### Tasks

**1. Migration Assessment:**

```markdown
## Migration Assessment: GitLab → GitHub

### Source Inventory
- Total repositories:
- Total users:
- CI/CD pipelines to convert:
- Package registries in use:

### Migration Complexity Matrix

| Component | Complexity | Notes |
|-----------|------------|-------|
| Git history | | |
| Branches | | |
| Issues/MRs | | |
| CI/CD | | |
| Wiki | | |
| Packages | | |

### Risk Assessment
[Identify risks and mitigations]

```

**2. Migration Timeline:**

```markdown
## Migration Timeline

### Phase 1: Preparation (Week 1-2)
- [ ] Complete inventory
- [ ] Set up GitHub Enterprise
- [ ] Configure SSO
- [ ] Create organizations and teams

### Phase 2: Pilot Migration (Week 3-4)
- [ ] Select 5-10 pilot repos
- [ ] Execute migration
- [ ] Validate content
- [ ] Convert CI/CD
- [ ] Get team feedback

### Phase 3: Bulk Migration (Week 5-8)
- [ ] Execute in batches
- [ ] Daily validation
- [ ] Track issues

### Phase 4: Cutover (Week 9-10)
- [ ] Final sync
- [ ] DNS/redirect updates
- [ ] Decommission source

```

---

## Lab Completion Checklist

- [ ] Lab 9.1: Enterprise organizational design documented
- [ ] Lab 9.2: SSO configuration guide created
- [ ] Lab 9.3: Audit log queries and report template created
- [ ] Lab 9.4: Migration assessment and timeline documented
