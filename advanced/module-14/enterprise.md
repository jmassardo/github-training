---
layout: training-module
title: "Enterprise Administration"
permalink: /advanced/module-14/enterprise/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 7
total_sections: 12
phase: advanced
estimated_time: "35 min"
module_index: /advanced/module-14/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-14/overview/"
    short_title: "Overview"
    icon: "📋"
  - title: "Copilot Agents"
    url: "/advanced/module-14/agents/"
    short_title: "Agents"
    icon: "🤖"
  - title: "Model Context Protocol"
    url: "/advanced/module-14/mcp/"
    short_title: "MCP"
    icon: "🔌"
  - title: "Copilot Extensions"
    url: "/advanced/module-14/extensions/"
    short_title: "Extensions"
    icon: "🧩"
  - title: "Custom Instructions"
    url: "/advanced/module-14/instructions/"
    short_title: "Instructions"
    icon: "📝"
  - title: "Copilot Spaces"
    url: "/advanced/module-14/spaces/"
    short_title: "Spaces"
    icon: "🗂️"
  - title: "Enterprise Administration"
    url: "/advanced/module-14/enterprise/"
    short_title: "Enterprise"
    icon: "🏢"
  - title: "Metrics & Adoption"
    url: "/advanced/module-14/metrics/"
    short_title: "Metrics"
    icon: "📊"
  - title: "Hands-On Labs"
    url: "/advanced/module-14/labs/"
    short_title: "Labs"
    icon: "🧪"
  - title: "Knowledge Check"
    url: "/advanced/module-14/quiz/"
    short_title: "Quiz"
    icon: "📝"
  - title: "Resources"
    url: "/advanced/module-14/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/advanced/module-14/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
prev_section:
  url: /advanced/module-14/spaces/
  title: "Copilot Spaces"
  title: "Custom Instructions"
next_section:
  url: /advanced/module-14/metrics/
  title: "Metrics & Adoption"
---

## Managing Copilot at Scale

GitHub Copilot Enterprise provides centralized controls for managing AI assistance across your organization. This section covers seat management, policy configuration, and compliance features.

### Enterprise vs Business

| Feature | Copilot Business | Copilot Enterprise |
|---------|-----------------|-------------------|
| Code completions | ✅ | ✅ |
| Chat in IDE | ✅ | ✅ |
| Chat in GitHub.com | ✅ | ✅ |
| Model selection | ✅ | ✅ |
| PR summaries | ✅ | ✅ |
| Copilot code review | ✅ | ✅ |
| Copilot Autofix | ✅ | ✅ |
| Coding agent | ✅ | ✅ |
| Knowledge bases | ❌ | ✅ |
| Copilot Dashboard | ❌ | ✅ |

---

## Administration Hierarchy

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph AdminHierarchy["Administration Hierarchy"]
    Enterprise["🏛️ Enterprise Level<br/>• Enterprise-wide policies<br/>• Cross-org license management<br/>• Consolidated billing &amp; AI Credit budgets"]
    
    Enterprise --> Org1 & Org2 & Org3
    
    subgraph Org1["🏢 Org Admin"]
      O1A["• Seats"]
      O1B["• Policy"]
      O1C["• Usage"]
    end
    
    subgraph Org2["🏢 Org Admin"]
      O2A["• Seats"]
      O2B["• Policy"]
      O2C["• Usage"]
    end
    
    subgraph Org3["🏢 Org Admin"]
      O3A["• Seats"]
      O3B["• Policy"]
      O3C["• Usage"]
    end
  end
</div>
</div>

Enterprise admins set global policies that organization admins can customize within allowed parameters.

---

## Seat Management

### Accessing Seat Management

Navigate to: **Organization** → **Settings** → **Copilot** → **Access**

### Assignment Methods

| Method | Description | Best For |
|--------|-------------|----------|
| **Disabled** | No access allowed | Cost control, compliance |
| **All members** | Automatic access | Small organizations |
| **Selected members** | Manual assignment | Budget constraints |
| **Selected teams** | Team-based access | Department rollout |

### Seat Assignment via CLI

```bash
# List current seat assignments
gh api /orgs/{org}/copilot/billing/seats \
  --jq '.seats[] | {user: .assignee.login, created: .created_at}'

# Get total seat information
gh api /orgs/{org}/copilot/billing \
  --jq '{total: .seat_breakdown.total, active: .seat_breakdown.active_this_cycle}'

# Assign seat to user
gh api -X POST /orgs/{org}/copilot/billing/selected_users \
  -f selected_usernames[]="newuser"

# Remove seat from user
gh api -X DELETE /orgs/{org}/copilot/billing/selected_users \
  -f selected_usernames[]="olduser"

# Bulk assignment from file
cat users.txt | while read user; do
  gh api -X POST /orgs/{org}/copilot/billing/selected_users \
    -f selected_usernames[]="$user"
done

```

### Team-Based Assignment

```bash
# Get team members
gh api /orgs/{org}/teams/{team}/members --jq '.[].login'

# Assign all team members
gh api /orgs/{org}/teams/{team}/members --jq '.[].login' | while read user; do
  gh api -X POST /orgs/{org}/copilot/billing/selected_users \
    -f selected_usernames[]="$user"
  echo "Assigned: $user"
done

```

---

## Policy Configuration

### Content Exclusions

Prevent Copilot from accessing sensitive code paths:

**Location**: Organization Settings → Copilot → Content Exclusion

```
# Repository-specific exclusions
repo:myorg/confidential-repo

# Directory patterns
**/secrets/**
**/internal-api/**
**/proprietary/**

# File type exclusions
**/*.env
**/*.env.*
**/*.pem
**/*.key

# Configuration files
**/config/production/**
**/config/secrets/**

```

### Managing Exclusions Programmatically (Public Preview)

As of February 2026, the **Copilot Content Exclusion REST API** is in public preview, enabling teams to manage exclusion rules programmatically — no UI required.

```bash
# List current content exclusions for an org
gh api /orgs/{org}/copilot/content-exclusions

# Add a new exclusion via API
gh api -X POST /orgs/{org}/copilot/content-exclusions \
  -f "content_exclusion_rules[][type]=repository_path" \
  -f "content_exclusion_rules[][pattern]=**/secrets/**"

# Remove an exclusion
gh api -X DELETE /orgs/{org}/copilot/content-exclusions/{exclusion_id}
```

This is especially useful for large enterprises that manage many organizations and want to enforce consistent exclusion policies through automation (CI/CD pipelines, Terraform, etc.) rather than manually through the UI.

> **📚 Learn More:** [Managing Content Exclusions](https://docs.github.com/en/copilot/managing-copilot/managing-copilot-for-your-enterprise/managing-policies-and-features-for-copilot-in-your-enterprise)

### How Exclusions Work

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Exclusions["Code Request Flow"]
    Request["📨 Code Request"] --> Check["🔍 Exclusion Check"]
    
    Check -->|"Path Matches<br/>Exclusion Rule"| Blocked["🚫 Blocked<br/>No Access"]
    Check -->|"Path Not Matched<br/>(Allowed)"| Processed["✅ Processed<br/>by Copilot"]
  end
</div>
</div>

### Feature Policies

Control which Copilot features are available:

**Location**: Organization Settings → Copilot → Policies

| Policy | Options | Recommendation |
|--------|---------|----------------|
| Copilot in IDE | Enable/Disable | Enable |
| Copilot Chat in GitHub.com | Enable/Disable | Enable |
| Copilot for PRs | Enable/Disable | Enable |
| Copilot coding agent | Enable/Disable | Pilot first |
| Copilot Extensions | Enable/Disable/Specific | Enable |

### Public Code Matching

Control whether suggestions matching public code are shown:

```yaml
# Organization policy
copilot:
  public_code_suggestions: 
    enabled: true          # Allow all suggestions
    # OR
    enabled: false         # Block matching public code
    # OR
    enabled: "filter"      # Show with notification

```

---

## Audit Logging

### Copilot Audit Events

| Event | Description |
|-------|-------------|
| `copilot.seat_assigned` | User given Copilot access |
| `copilot.seat_revoked` | User access removed |
| `copilot.content_exclusion_changed` | Path exclusion modified |
| `copilot.policy_changed` | Policy setting updated |
| `copilot.extension_installed` | Extension enabled for org |
| `copilot.extension_removed` | Extension disabled |

### Viewing Audit Logs

**Web Interface**: Organization → Settings → Audit log → Filter by "copilot"

**CLI Access**:

```bash
# Get all Copilot events
gh api /orgs/{org}/audit-log \
  --jq '.[] | select(.action | startswith("copilot"))'

# Get seat changes in last 30 days
gh api "/orgs/{org}/audit-log?phrase=action:copilot.seat" \
  --jq '.[] | {action, actor: .actor, user: .user, date: .created_at}'

# Export for compliance
gh api /orgs/{org}/audit-log \
  --paginate \
  --jq '.[] | select(.action | startswith("copilot")) | @json' \
  > copilot_audit_$(date +%Y%m%d).json

```

### SIEM Integration

Forward audit logs to your security information and event management (SIEM) system:

```json
// Webhook payload format
{
  "@timestamp": "2024-01-15T10:30:00Z",
  "action": "copilot.seat_assigned",
  "actor": "admin-user",
  "actor_id": 12345,
  "org": "myorg",
  "org_id": 67890,
  "user": "new-developer",
  "user_id": 11111,
  "actor_location": {
    "country_code": "US"
  },
  "created_at": 1705315800000
}

```

Configure streaming: Organization → Settings → Audit log → Log streaming

---

## Compliance & Data Governance

### Data Handling Commitments

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph DataFlow["Data Flow"]
    Code["📄 Your Code<br/>Controlled by Exclusions"] --> API["🔗 GitHub API<br/>Encrypted in Transit"] --> AI["🤖 AI Models<br/>Not Used for Training"]
  end
  
  subgraph Commitments["Key Commitments"]
    C1["❌ Code NOT used for model training"]
    C2["❌ Code NOT stored after processing"]
    C3["⏱️ Prompts retained 28 days (Enterprise: configurable)"]
    C4["✅ SOC 2 Type 2 certified"]
    C5["✅ GDPR compliant with DPA"]
  end
</div>
</div>

### Compliance Documentation

| Document | Purpose |
|----------|---------|
| [Trust Center](https://github.com/trust) | Security practices |
| [Privacy Statement](https://docs.github.com/en/site-policy/privacy-policies/github-copilot-trust-center) | Data handling |
| [DPA](https://github.com/customer-terms) | GDPR compliance |
| [SOC 2 Report](https://github.com/security) | Audit report |

### Compliance Checklist

- [ ] Review and configure content exclusion paths
- [ ] Set up audit log forwarding to SIEM
- [ ] Document acceptable use policy for developers
- [ ] Train users on sensitive data handling
- [ ] Schedule quarterly access reviews
- [ ] Establish incident response procedures
- [ ] Configure data residency (if required)

---

## Rollout Strategy

### Phase 1: Pilot (Weeks 1-4)

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Pilot["PILOT PHASE"]
    W1["📅 Week 1: Setup<br/>• Select pilot team (10-20 devs)<br/>• Configure policies & exclusions<br/>• Establish success metrics"]
    W2["📅 Week 2-3: Active Pilot<br/>• Enable access for pilot team<br/>• Provide training & resources<br/>• Collect feedback daily"]
    W4["📅 Week 4: Evaluation<br/>• Analyze usage metrics<br/>• Document issues & solutions<br/>• Refine policies based on learnings"]
    
    W1 --> W2 --> W4
  end
</div>
</div>

### Phase 2: Expansion (Weeks 5-12)

- Identify next wave of teams
- Conduct training sessions
- Enable access team by team
- Monitor adoption metrics
- Address feedback quickly

### Phase 3: Enterprise-Wide (Week 13+)

- Enable for all members
- Publish self-service documentation
- Establish office hours for support
- Create champions program
- Regular business reviews

---

## Troubleshooting

### Common Issues

| Issue | Possible Cause | Solution |
|-------|---------------|----------|
| User can't see Copilot | No seat assigned | Check seat assignment |
| No suggestions appearing | Content exclusion | Verify file path not excluded |
| Chat not working | Feature policy disabled | Check feature policies |
| Slow responses | Network/proxy issues | Review proxy configuration |
| Extension not available | Not enabled for org | Enable in policy settings |

### Diagnostic Commands

```bash
# Check user's seat status
gh api /orgs/{org}/copilot/billing/seats \
  --jq '.seats[] | select(.assignee.login == "username")'

# Verify organization policies
gh api /orgs/{org}/copilot/policies

# Check content exclusions
gh api /orgs/{org}/copilot/content-exclusions

# Review recent audit events
gh api /orgs/{org}/audit-log?phrase=copilot \
  --jq '.[0:10] | .[] | {action, actor: .actor, date: .created_at}'

```

### Support Escalation

1. Check [GitHub Status](https://www.githubstatus.com/) for service issues
2. Review audit logs for recent changes
3. Test with a different user account
4. Open support ticket with:
   - User login and organization
   - Error messages or screenshots
   - Steps to reproduce
   - Recent policy changes

---

## Cost Management

### Monitoring Utilization

```bash
# Get utilization summary
gh api /orgs/{org}/copilot/billing \
  --jq '{
    total_seats: .seat_breakdown.total,
    active_seats: .seat_breakdown.active_this_cycle,
    utilization_pct: ((.seat_breakdown.active_this_cycle / .seat_breakdown.total) * 100 | floor)
  }'

# Identify inactive users (no activity in 30 days)
gh api /orgs/{org}/copilot/billing/seats \
  --jq '[.seats[] | select(.last_activity_at < (now - 2592000 | todate))] | 
        {inactive_users: [.[].assignee.login], count: length}'

```

### Optimization Strategies

| Strategy | Impact | Effort |
|----------|--------|--------|
| Quarterly inactive seat review | High | Low |
| Team-based allocation | Medium | Medium |
| Chargeback to departments | Medium | High |
| Usage-based eligibility | High | Medium |

### AI Credits Billing (June 2026)

Starting June 1, 2026, Copilot uses **usage-based billing with AI Credits** alongside seat management:

| Aspect | Details |
|--------|---------|
| **Credit value** | 1 AI Credit = $0.01 USD |
| **Business allocation** | 1,900 credits/user/month (pooled across org) |
| **Enterprise allocation** | 3,900 credits/user/month (pooled across org) |
| **What consumes credits** | Copilot Chat, CLI, cloud agents, Spaces, code review, advanced models |
| **What stays free** | Standard code completions (ghost text) remain unlimited |
| **Budget controls** | Set spending limits at enterprise, cost center, or user level |
| **Overage** | Additional credits can be purchased; usage stops when credits are exhausted |

<div class="callout callout-tip">
<div class="callout-title">💡 CSM Talking Point</div>

When enterprise customers ask about cost predictability under the new model, emphasize:
- **Pooled credits** mean light users offset heavy users within the org
- **Budget controls** prevent surprise overages
- **Code completions are still free** — the highest-volume feature costs nothing extra
- **Model choice matters** — steering teams toward cost-effective models (like GPT-4.1 or GPT-5 mini) stretches credits further
</div>

> **📚 Learn More:** [Models and Pricing](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)

---

## Key Takeaways

1. **Centralized control** enables consistent policy enforcement across the organization
2. **Content exclusions** protect sensitive code from AI access
3. **Audit logging** supports compliance and security requirements
4. **Phased rollout** reduces risk and improves adoption success
5. **Regular reviews** optimize costs and maintain appropriate access
6. **AI Credits billing** requires new cost management strategies — monitor credit consumption alongside seat utilization
7. **Clear documentation** empowers users and reduces support burden
