---
layout: training-module
title: "Module 10, Section 7: Real-World CSM Scenarios"
description: "Practice handling customer questions about APIs and integrations"
permalink: /advanced/module-10/scenarios/
module_number: 10
section_number: 7
phase: advanced
prev_section:
  url: /advanced/module-10/resources/
  title: "Resources"
next_section:
  url: /advanced/module-11/
  title: "Module 11: IaC & Operations"
toc: true
module_title: "GitHub APIs, Webhooks & Apps"
total_sections: 7
module_index: /advanced/module-10/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-10/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "15 min"
  - title: "Core Concepts"
    url: "/advanced/module-10/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/advanced/module-10/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "45 min"
  - title: "Hands-On Labs"
    url: "/advanced/module-10/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/advanced/module-10/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "20 min"
  - title: "Resources"
    url: "/advanced/module-10/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/advanced/module-10/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "25 min"
---
{% raw %}

## Section 7: Real-World CSM Scenarios

### Scenario 10.1: Custom Dashboard Integration
**Situation:** A customer's engineering leadership wants a dashboard showing:
- Active pull requests by team
- Average time to merge
- Code review velocity
- Deployment frequency
**CSM Approach:**

```markdown
## Dashboard Integration Recommendation
### Data Requirements

| Metric | API Source | Query Type |
|--------|------------|------------|
| Active PRs | GraphQL | Pull request search |
| Time to merge | GraphQL | PR merged_at - created_at |
| Review velocity | GraphQL | Review events timeline |
| Deployments | REST | Deployments API |
### Architecture Options
#### Option 1: Direct API Polling
- Scheduled job queries GitHub APIs
- Store results in analytics database
- Dashboard reads from database
- Pros: Simple, full control
- Cons: Rate limits, data freshness
#### Option 2: Webhook + API Hybrid
- Webhooks capture events in real-time
- API backfills historical data
- Pros: Real-time updates
- Cons: More complex setup
### Recommended Approach
[Based on customer's technical capabilities and requirements]

```

### Scenario 10.2: Compliance Automation
**Situation:** Customer needs to automatically enforce:
- All PRs require linked Jira ticket
- Certain branches require security team approval
- All merges must pass compliance checks
**CSM Approach:**

```markdown
## Compliance GitHub App Design
### Use Case 1: PR Title Validation
- Event: pull_request (opened, edited)
- Action: Create check run validating title format
- Configuration: Repository-level config file
### Use Case 2: Security Team Approval
- Event: pull_request_review
- Action: Check if security team member approved
- Integration: Team membership API
### Use Case 3: Compliance Check
- Event: check_suite
- Action: Aggregate all required checks
- Status: Create summary check run
### Implementation Approach
1. Build single GitHub App for all use cases
2. Use repository config files for customization
3. Leverage branch protection for enforcement
4. Audit log integration for compliance reporting

```

### Scenario 10.3: Migration Automation
**Situation:** Customer migrating from Azure DevOps needs to:
- Import repositories with history
- Map Azure DevOps users to GitHub users
- Recreate work items as GitHub Issues
**CSM Approach:**

```markdown
## Migration Automation Strategy
### Phase 1: Repository Migration
- Tool: GitHub Enterprise Importer (GEI)
- Script generation: `gh gei generate-script`
- Validation: History, branches, tags
### Phase 2: User Mapping
- API: Azure DevOps + GitHub
- Output: User mapping file
- Action: Mannequin linking in GEI
### Phase 3: Work Item Migration
- Source: Azure DevOps Work Item API
- Target: GitHub Issues API
- Script: Custom migration script
### Sample Migration Script

```python

def migrate_work_items(ado_org, ado_project, gh_org, gh_repo):
    # 1. Export work items from Azure DevOps
    work_items = ado_api.get_work_items(ado_org, ado_project)
    
    # 2. Transform to GitHub Issues format
    issues = transform_work_items(work_items)
    
    # 3. Create issues in GitHub
    for issue in issues:
        gh_api.create_issue(gh_org, gh_repo, issue)

```
### Validation Checklist
- [ ] All repositories imported
- [ ] Git history preserved
- [ ] Users mapped correctly
- [ ] Work items migrated
- [ ] Links updated

```

### Scenario 10.4: Event-Driven Architecture
**Situation:** Customer wants to build an internal developer platform that reacts to GitHub events:
- New repository created → Apply standard settings
- Team created → Add to default repositories
- Member added → Trigger onboarding workflow
**CSM Approach:**

```markdown
## Event-Driven Platform Architecture
### GitHub App Configuration
- Install at organization level
- Subscribe to organization events
- Webhook endpoint: Internal platform API
### Event Handlers
#### Repository Created

```yaml

event: repository.created
actions:
  - Apply branch protection to default branch
  - Add standard labels
  - Create standard issue templates
  - Notify platform team

```
#### Team Created

```yaml

event: team.created
actions:
  - Add to common repositories (read access)
  - Create team discussion board
  - Notify team lead

```
#### Member Added

```yaml

event: organization.member_added
actions:
  - Trigger onboarding GitHub Action
  - Add to default teams
  - Send welcome message

```
### Architecture Diagram

```

GitHub Events → Webhook → Event Router → Handler Functions
                                              ↓
                                        GitHub API
                                        (Settings, Teams, Actions)

```

```

---

## Module Summary

### Key Takeaways for CSMs
1. **Choose the Right API** - REST for simple operations, GraphQL for complex queries
2. **GitHub Apps > OAuth Apps** - Better security, permissions, and rate limits
3. **Webhooks Enable Real-Time** - Essential for responsive integrations
4. **Rate Limits Matter** - Design integrations with rate limits in mind
5. **Security First** - Always verify webhook signatures, protect tokens

### Common Customer Questions

| Question | Response |
|----------|----------|
| "Can we pull all our data via API?" | Yes, but consider rate limits and use GraphQL for efficiency |
| "How do webhooks handle failures?" | GitHub retries; implement idempotency on your end |
| "Should we use OAuth or GitHub Apps?" | GitHub Apps for automation; OAuth for user-facing integrations |
| "What's the cost of API usage?" | API is included in GitHub plan; rate limits are the constraint |

### Next Module Preview
**Module 11: Infrastructure as Code & Operations** will cover:
- GitHub's Terraform provider for declarative configuration
- Pulumi and other IaC tools for GitHub management
- GitOps practices for infrastructure
- Operational best practices for GitHub administration
{% endraw %}
