---
layout: training-module
title: "CSM Scenarios"
permalink: /advanced/module-9/scenarios/
module_number: 9
module_title: "Enterprise Administration"
section_number: 7
total_sections: 7
phase: advanced
estimated_time: "30 min"
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
  url: /advanced/module-9/resources/
  title: "Resources"
---

# Real-World CSM Scenarios

## Scenario 9.1: SSO Implementation Project

**Situation:** A new enterprise customer wants to implement SSO but is concerned about:
- Developer disruption during rollout
- Handling service accounts
- Recovery procedures if SSO fails

### CSM Approach

**1. Address Developer Concerns:**

```markdown
## Developer Communication Plan

### Before Rollout
- Announce SSO implementation 2 weeks in advance
- Share what will change (login flow)
- Share what WON'T change (repos, permissions, workflow)
- Offer testing opportunity

### During Rollout
- Start with pilot group (10% of users)
- Provide dedicated support channel
- Monitor for authentication issues

### After Rollout
- Survey user experience
- Document any issues encountered
- Update runbooks

```

**2. Handle Service Accounts:**

```markdown
## Service Account Strategy

### Option 1: PAT-based Service Accounts
- Create dedicated GitHub accounts
- Use fine-grained PATs
- Exempt from SSO requirement
- Document ownership and rotation

### Option 2: GitHub Apps
- Create GitHub App for automation
- Use installation tokens
- Better audit trail
- Scoped permissions

```

**3. Document Recovery Procedures:**

```markdown
## SSO Bypass Procedures

### Emergency Access
1. Enterprise owners have bypass rights
2. Recovery codes stored in password manager
3. Contact GitHub Support for lockout

### SSO Disable Procedure
1. Enterprise owner logs in with bypass
2. Navigate to Authentication settings
3. Disable SAML requirement
4. Communicate to users
5. Investigate root cause

```

---

## Scenario 9.2: Complex Migration

**Situation:** Customer is migrating from Bitbucket Server with:
- 500 repositories
- Complex branch permissions
- Build pipelines in Jenkins
- Multiple teams with different timelines

### CSM Approach

```markdown
## Migration Strategy

### Phase 1: Discovery & Planning (2 weeks)
- Inventory all repositories and classify by criticality
- Map Bitbucket permissions to GitHub model
- Identify pipeline conversion requirements
- Create team-specific migration schedules

### Phase 2: Foundation (2 weeks)
- Set up GitHub Enterprise organization structure
- Configure SSO and SCIM
- Establish naming conventions
- Create migration runbook

### Phase 3: Pilot (2 weeks)
- Select 20 low-risk repositories
- Execute migration with GEI
- Convert 5 Jenkins pipelines to Actions
- Gather feedback and refine process

### Phase 4: Wave Migrations (6 weeks)
- Wave 1: Team A (100 repos)
- Wave 2: Team B (150 repos)
- Wave 3: Team C (150 repos)
- Wave 4: Final cleanup (100 repos)

### Success Criteria
- [ ] All git history preserved
- [ ] No code freeze longer than 4 hours
- [ ] CI/CD downtime under 1 day per team
- [ ] User satisfaction > 4/5

```

---

## Scenario 9.3: Compliance Audit Support

**Situation:** Customer's security team needs to demonstrate:
- User access reviews are performed
- All access changes are logged
- Sensitive repositories have appropriate controls

### CSM Approach

**1. Set Up Access Review Process:**

```markdown
## Quarterly Access Review

### Automated Reports
1. Export org membership via API
2. Generate repository access matrix
3. Flag stale accounts (no activity 90+ days)
4. List outside collaborators

### Review Process
1. Distribute reports to team leads
2. Team leads verify member necessity
3. Remove/update unnecessary access
4. Document decisions in ticket

### Evidence Collection
- Screenshot of review completion
- Audit log export for changes made
- Sign-off from security team

```

**2. Configure Audit Log Streaming:**

```markdown
## Audit Log Streaming Setup

### Destination: Customer's Splunk Instance

### Events to Monitor:
- Authentication events
- Repository access changes
- Permission modifications
- Branch protection changes
- Secret scanning alerts

### Alert Conditions:
- Failed authentication > 5 in 5 minutes
- Repository visibility changed to public
- Branch protection disabled
- Enterprise owner added

```

**3. Document Controls:**

```markdown
## GitHub Security Controls Matrix

| Control | Implementation | Evidence |
|---------|----------------|----------|
| Access Management | SCIM provisioning | SCIM logs, audit log |
| Authentication | SAML SSO + MFA | IdP logs, GitHub audit |
| Authorization | Team-based RBAC | Team membership exports |
| Logging | Audit log streaming | Splunk retention |
| Change Management | Branch protection | Repository settings |

```

---

## Scenario 9.4: Hybrid Deployment Design

**Situation:** Customer wants to keep sensitive IP on GitHub Enterprise Server while using GitHub.com for open source contributions.

### CSM Approach

```markdown
## Hybrid GitHub Deployment

### Architecture
- GHES: Internal/sensitive code (on-premises)
- GHEC: Open source and external collaboration

### GitHub Connect Configuration
- [x] Unified search (internal users only)
- [x] Unified contributions
- [x] Dependabot updates
- [ ] Actions sync (evaluate security)

### User Experience
- Developers have accounts on both platforms
- Internal work on GHES
- OSS contributions on GHEC
- Single sign-on for both (same IdP)

### Code Flow Policies
- GHES → GHEC: Requires security review
- GHEC → GHES: Requires legal approval
- Inner source: GHES internal visibility

### Sync Strategy
- No automatic repository sync
- Manual process for code sharing
- Clear ownership documentation

```

---

## Module Summary

### Key Takeaways for CSMs

1. **Identity Management is Foundation** - SSO and SCIM are prerequisites for enterprise success
2. **EMU vs. Standard is Critical Decision** - Help customers understand tradeoffs early
3. **Migration Planning Prevents Pain** - Thorough planning makes migrations smooth
4. **Audit Logs Enable Compliance** - Configure streaming for security and compliance teams
5. **Hybrid Deployments Add Complexity** - GitHub Connect helps bridge GHES and GHEC

### Common Customer Questions

| Question | Response |
|----------|----------|
| "Can we use multiple IdPs?" | One IdP per enterprise, but can federate IdPs upstream |
| "What happens if SSO goes down?" | Enterprise owners can bypass; have recovery plan |
| "How long do migrations take?" | Varies; 100 repos typically 2-4 weeks with validation |
| "Can we audit who viewed code?" | Repository access logged; code views require GHES |

### Next Module Preview

**Module 10: GitHub APIs, Webhooks & Apps** will cover:
- REST and GraphQL API deep dive
- Building custom integrations with webhooks
- Creating GitHub Apps for automation
- OAuth vs. GitHub Apps decision framework
