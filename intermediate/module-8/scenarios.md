---
layout: training-module
title: "Module 8, Section 7: Real-World CSM Scenarios"
description: "Practical deployment scenarios for customer success managers"
permalink: /intermediate/module-8/scenarios/
module_number: 8
section_number: 7
phase: intermediate
prev_section:
  url: /intermediate/module-8/resources/
  title: "Resources"
next_section:
  url: /intermediate/module-9/
  title: "Module 9: Enterprise Administration"
toc: true
module_title: "CI/CD Pipelines & Deployment"
total_sections: 7
module_index: /intermediate/module-8/
sections:
  - title: "Context & Overview"
    url: "/intermediate/module-8/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "25 min"
  - title: "Core Concepts"
    url: "/intermediate/module-8/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/intermediate/module-8/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "60 min"
  - title: "Hands-On Labs"
    url: "/intermediate/module-8/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/intermediate/module-8/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "30 min"
  - title: "Resources"
    url: "/intermediate/module-8/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/intermediate/module-8/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "35 min"
---
{% raw %}

## Scenario 1: Deployment Frequency Goals
**Situation:**
A customer currently deploys monthly and wants to move to weekly deployments. Their current process involves manual testing and approval from multiple stakeholders.
**Customer Question:**
"We want to deploy more frequently but we're worried about quality. How do we move faster without breaking things?"
**Your Response:**
"Moving from monthly to weekly is absolutely achievable. Let's build confidence through automation:
**Current State Assessment:**

- Monthly deploys = ~12 deployments/year
- Manual testing = days of lead time
- Multiple approvals = bottleneck
**Target State:**

- Weekly deploys = ~52 deployments/year
- Automated testing = minutes of validation
- Streamlined approvals = hours not days
**Week 1-2: Automate Testing**

```yaml
# Add comprehensive test suite
jobs:
  unit-tests:
    # 2-3 minutes
  integration-tests:
    # 5-10 minutes
  e2e-tests:
    # 10-15 minutes

```

**Week 3-4: Add Staging Environment**
- Mirror production
- Auto-deploy main branch
- Run integration tests against staging
**Week 5-6: Implement Deployment Pipeline**

```
Commit → CI (15 min) → Staging (auto) → Soak 24h → Production (approval)

```

**Week 7-8: Reduce Approval Bottleneck**
- Required reviewers: 1 (not 5)
- Clear approval criteria documented
- Automated checks replace manual checks
**Metrics to Track:**

- Lead time (commit to production)
- Deployment frequency
- Change failure rate
- Mean time to recovery
**Risk Mitigation:**

- Feature flags for new features
- Gradual rollout (canary)
- Automated rollback on failure
- Better monitoring and alerting
Start with low-risk services first, build confidence, then expand. Would you like help identifying pilot services?"
---

## Scenario 2: Zero-Downtime Requirement
**Situation:**
A customer operates a 24/7 service with strict uptime SLAs. They're concerned about deployment-related downtime.
**Customer Question:**
"We have 99.99% uptime SLA. How can we deploy without any downtime?"
**Your Response:**
"Zero-downtime deployment is achievable with the right architecture. Here's how:
**Strategy 1: Blue-Green Deployment**

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph BlueGreen["Zero-Downtime Blue-Green"]
    LB["⚖️ Load Balancer"]
    
    subgraph Envs["Environments"]
      direction LR
      BLUE["🔵 Blue v1<br/>← Live"]:::live
      GREEN["🟢 Green v2<br/>Testing"]:::idle
    end
    
    LB --> Envs
    
    subgraph Steps["Process"]
      S1["1. Deploy v2 to Green"]
      S2["2. Test Green thoroughly"]
      S3["3. Switch traffic to Green"]
      S4["4. Blue becomes standby/next"]
      S1 --> S2 --> S3 --> S4
    end
  end
  
  classDef live fill:#22c55e,color:#fff
  classDef idle fill:#6b7280,color:#fff
</div>
</div>

**Implementation:**

```yaml
# Azure App Service slots
- name: Deploy to staging slot
  uses: azure/webapps-deploy@v3
  with:
    app-name: my-app
    slot-name: staging
    
- name: Warm up staging
  run: curl https://my-app-staging.azurewebsites.net/health
  
- name: Swap to production
  run: az webapp deployment slot swap --slot staging --target-slot production

```

**Strategy 2: Rolling Deployment**

```yaml
# Kubernetes rolling update
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 25%        # Allow 25% extra pods
      maxUnavailable: 0    # Never less than desired

```

**Key Requirements:**

1. **Health checks:**
   - Readiness probe before receiving traffic
   - Liveness probe to detect unhealthy pods
2. **Graceful shutdown:**
   - Handle SIGTERM signal
   - Complete in-flight requests
   - Connection draining
3. **Database compatibility:**
   - Backward-compatible schema changes
   - Feature flags for data migrations
4. **Session management:**
   - Externalize session state (Redis)
   - Or use sticky sessions with drain
**Monitoring:**

```yaml
- name: Monitor deployment
  run: |
    # Watch error rate during deployment
    # Alert if error rate exceeds threshold
    # Auto-rollback if needed

```

Would you like help implementing health checks and graceful shutdown?"

## Scenario 3: Compliance and Audit Trail
**Situation:**
A financial services customer needs to demonstrate deployment controls for regulatory compliance.
**Customer Question:**
"Our auditors want to see who approved each production deployment and a complete audit trail. How do we provide this?"
**Your Response:**
"GitHub provides comprehensive audit capabilities. Here's your compliance package:
**1. Deployment Approval Evidence:**

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Compliance["Environment: production"]
    PR["🔐 Protection rules"]
    PR --> RR["Required reviewers: @compliance-team"]
    PR --> DB["Deployment branches: main only"]
    AU["📝 Audit"] --> LOG["Every approval logged with<br/>timestamp and approver"]
  end
</div>
</div>

**2. GitHub Audit Log:**

```
Repository → Settings → Audit log
Deployment events captured:

- deployment.created
- deployment_status.created  
- environment.created
- environment.updated
- protection_rule.created

```

**3. Workflow Run History:**

```
Actions → Workflow runs
Each run shows:

- Who triggered (actor)
- What commit (SHA)
- Approval timestamps
- Environment deployed to
- Success/failure status

```

**4. API Export for Long-term Retention:**

```bash
# Export deployment history
gh api /repos/{owner}/{repo}/deployments \
  --paginate \

  | jq '.[] | {id, environment, sha, creator: .creator.login, created_at}'
# Export audit log (Enterprise)
gh api /orgs/{org}/audit-log \
  --paginate \
  -f phrase='action:deployment' \
  > deployments-audit.json

```

**5. Deployment Approval Record:**

```yaml
- name: Record approval
  uses: actions/github-script@v7
  with:
    script: |
      // Create compliance record
      await github.rest.issues.create({
        owner: context.repo.owner,
        repo: 'compliance-records',
        title: `Deployment ${context.sha.slice(0,7)} to production`,
        body: `
          ## Deployment Record
          - **Commit:** ${context.sha}
          - **Environment:** production
          - **Approved by:** ${context.actor}
          - **Time:** ${new Date().toISOString()}
          - **Workflow run:** ${context.runId}
        `,
        labels: ['deployment', 'audit']
      });

```

**6. SIEM Integration:**

- Stream audit logs to Splunk/Datadog
- Real-time alerting on policy violations
- Long-term retention beyond GitHub limits
**Documentation for Auditors:**

- Screenshot of environment protection rules
- Sample deployment approval email/notification
- Export of recent deployment history
- Description of change management process
Want me to help generate a compliance report template?"
---

## Module Summary

### Key Takeaways
1. **Environments Provide Deployment Gates**
   - Protection rules for approvals and wait times
   - Environment-specific secrets and variables
   - Branch restrictions for deployment sources
2. **Choose the Right Deployment Strategy**
   - Rolling: Simple, gradual, no extra infrastructure
   - Blue-Green: Fast rollback, double infrastructure
   - Canary: Risk mitigation, complex routing
3. **OIDC Eliminates Long-Lived Secrets**
   - Short-lived tokens from cloud providers
   - No secrets to rotate or leak
   - Audit trail of all authentications
4. **Automation Enables Velocity**
   - Comprehensive test suites enable confidence
   - Automated deployments reduce human error
   - Monitoring enables fast detection and recovery
5. **Rollback is Not Optional**
   - Plan for failure
   - Test rollback procedures regularly
   - Keep previous versions available

### What's Next
In **Module 9: Enterprise Administration**, you'll learn:

- Organization and enterprise management
- User provisioning and SSO
- Policy enforcement at scale
- Enterprise audit and compliance
The deployment pipelines you've built here operate within the governance framework you'll learn to configure.
**Checklist: Module 8 Complete**
- [ ] Created GitHub Environments with protection rules
- [ ] Built a complete CI/CD pipeline
- [ ] Implemented at least one deployment strategy
- [ ] Configured cloud provider OIDC authentication
- [ ] Set up deployment notifications
- [ ] Created rollback workflow
- [ ] Completed all three labs
- [ ] Reviewed real-world scenarios
{% endraw %}
