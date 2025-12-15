---
layout: training-module
title: "Metrics & Adoption"
permalink: /advanced/module-14/metrics/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 7
total_sections: 10
phase: advanced
estimated_time: "30 min"
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
toc: true
prev_section:
  url: /advanced/module-14/enterprise/
  title: "Enterprise Administration"
next_section:
  url: /advanced/module-14/labs/
  title: "Hands-On Labs"
---

## Measuring Copilot Value

Understanding GitHub Copilot's impact helps justify investment, identify adoption gaps, and optimize configuration. This section covers the metrics framework, data collection, and ROI calculation.

### Why Measure?

| Goal | Metrics Help You |
|------|------------------|
| Justify investment | Quantify time savings and ROI |
| Improve adoption | Identify teams needing support |
| Optimize configuration | Tune settings based on data |
| Track progress | Show improvement over time |
| Make decisions | Data-driven feature rollout |

---

## Metrics Framework

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Framework["Metrics Framework"]
    Adoption["📊 ADOPTION METRICS<br/>• Active users / Total seats<br/>• Daily/Weekly active users<br/>• Feature utilization"]
    Quality["✅ QUALITY METRICS<br/>• Suggestion acceptance rate<br/>• Suggestions per user per day<br/>• Code retention rate"]
    Impact["🎯 IMPACT METRICS<br/>• Developer productivity<br/>• Time to first commit<br/>• Code review velocity<br/>• Developer satisfaction"]
    
    Adoption --> Quality --> Impact
  end
</div>
</div>

---

## Adoption Metrics

### Seat Utilization

Track how many assigned seats are actively being used.

```bash
# Get seat utilization via API
gh api /orgs/{org}/copilot/billing/seats --jq '{
  total_seats: (.seats | length),
  active_7d: [.seats[] | select(.last_activity_at > (now - 604800 | todate))] | length,
  active_30d: [.seats[] | select(.last_activity_at > (now - 2592000 | todate))] | length
}'

```

### Adoption Funnel

| Stage | Definition | Target |
|-------|------------|--------|
| **Assigned** | Users with Copilot seats | 100% of eligible developers |
| **Activated** | Used at least once | 95% of assigned |
| **Active** | Used in last 7 days | 80% of activated |
| **Power Users** | Daily usage | 60% of active |

### Tracking Dashboard Query

```sql
-- Example analytics query for adoption tracking
SELECT 
  DATE_TRUNC('week', activity_date) as week,
  COUNT(DISTINCT user_id) as active_users,
  COUNT(*) as total_suggestions,
  SUM(CASE WHEN accepted THEN 1 ELSE 0 END) as accepted,
  ROUND(100.0 * SUM(CASE WHEN accepted THEN 1 ELSE 0 END) / COUNT(*), 1) as acceptance_rate
FROM copilot_events
WHERE activity_date >= CURRENT_DATE - INTERVAL '90 days'
GROUP BY 1
ORDER BY 1;

```

---

## Quality Metrics

### Acceptance Rate

The percentage of shown suggestions that users accept.

```
                    Accepted Suggestions
Acceptance Rate = ─────────────────────────── × 100
                   Total Suggestions Shown

```

**Benchmarks**:
- 🔴 < 20% - Review configuration and training needs
- 🟡 20-30% - Normal range for most organizations
- 🟢 > 30% - High effectiveness, share best practices

### Code Assist Rate

The percentage of code written with Copilot assistance.

```
                    Lines Accepted from Copilot
Code Assist Rate = ────────────────────────────── × 100
                      Total Lines Committed

```

### Code Retention

How much accepted Copilot code remains in the codebase after review.

```
                  Copilot Code Still in Codebase (30d later)
Retention Rate = ────────────────────────────────────────────── × 100
                        Total Copilot Code Accepted

```

High retention indicates high-quality suggestions that pass code review.

---

## API-Based Reporting

### Organization Usage Summary

```bash
# Get daily usage metrics
gh api /orgs/{org}/copilot/usage \
  --jq '.usage_items[] | {
    date: .day,
    active_users: .total_active_users,
    acceptances: .total_acceptances_count,
    lines_suggested: .total_lines_suggested,
    lines_accepted: .total_lines_accepted
  }'

```

### Per-User Metrics

```bash
# Get individual user activity
gh api /orgs/{org}/copilot/billing/seats \
  --jq '.seats[] | {
    user: .assignee.login,
    last_active: .last_activity_at,
    editor: .last_activity_editor,
    created: .created_at
  }'

```

### Export to CSV

```bash
#!/bin/bash
# export-copilot-metrics.sh

ORG="${1:-myorg}"
OUTPUT_DIR="./reports"
DATE=$(date +%Y%m%d)

mkdir -p "$OUTPUT_DIR"

# Export usage metrics
echo "date,active_users,suggestions,acceptances,lines_suggested,lines_accepted" > "$OUTPUT_DIR/usage_$DATE.csv"
gh api /orgs/$ORG/copilot/usage --paginate \
  --jq '.usage_items[] | [
    .day,
    .total_active_users,
    .total_suggestions_count,
    .total_acceptances_count,
    .total_lines_suggested,
    .total_lines_accepted
  ] | @csv' >> "$OUTPUT_DIR/usage_$DATE.csv"

# Export seat assignments
echo "user,last_activity,editor,assigned_date" > "$OUTPUT_DIR/seats_$DATE.csv"
gh api /orgs/$ORG/copilot/billing/seats \
  --jq '.seats[] | [
    .assignee.login,
    .last_activity_at,
    .last_activity_editor,
    .created_at
  ] | @csv' >> "$OUTPUT_DIR/seats_$DATE.csv"

echo "Reports exported to $OUTPUT_DIR"

```

---

## Impact Measurement

### Developer Surveys

Quarterly surveys capture qualitative impact data.

**Survey Questions**:

1. **Time Savings**: "How much time does Copilot save you per week?"
   - No time saved
   - 1-2 hours
   - 3-5 hours
   - 5-10 hours
   - More than 10 hours

2. **Quality Impact**: "How has Copilot affected your code quality?"
   - Decreased
   - No change
   - Slightly improved
   - Significantly improved

3. **Task Helpfulness**: "What tasks is Copilot most helpful for?"
   - Boilerplate code
   - Test writing
   - Documentation
   - Learning new APIs
   - Debugging
   - Other

4. **Satisfaction**: "How satisfied are you with Copilot?" (1-10 scale)

### Productivity Indicators

| Indicator | How to Measure | Expected Impact |
|-----------|---------------|-----------------|
| PR Cycle Time | Commit to merge duration | 15-20% faster |
| Code Review Time | Review duration | 10-15% faster |
| Test Coverage | Coverage percentage | 5-10% increase |
| Bug Rate | Defects per commit | Neutral to improved |
| Developer Satisfaction | Survey NPS | +15-25 points |

### A/B Comparison Study

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph Study["Control vs Treatment"]
    subgraph Control["🔴 Control Group<br/>(No Copilot)"]
      C1["Avg PR Time: 4.2h"]
      C2["Lines/Day: 150"]
      C3["Test Coverage: 72%"]
      C4["Satisfaction: 7.2"]
    end
    
    subgraph Treatment["🟢 Treatment Group<br/>(With Copilot)"]
      T1["Avg PR Time: 3.1h"]
      T2["Lines/Day: 185"]
      T3["Test Coverage: 78%"]
      T4["Satisfaction: 8.4"]
    end
  end
  
  Results["📈 Results: 26% faster PRs, 23% more productive,<br/>6% better coverage, 17% happier"]
</div>
</div>

---

## Building a Metrics Program

### Phase 1: Baseline (Weeks 1-2)

- [ ] Document current development metrics before Copilot
- [ ] Set up API access for Copilot data collection
- [ ] Create initial dashboard with key metrics
- [ ] Define success criteria with stakeholders
- [ ] Establish baseline satisfaction survey

### Phase 2: Monitoring (Ongoing)

- [ ] Weekly usage reports for team leads
- [ ] Monthly trend analysis for leadership
- [ ] Quarterly business reviews with ROI
- [ ] Annual comprehensive assessment

### Phase 3: Optimization

- [ ] Identify low-adoption teams
- [ ] Conduct targeted training interventions
- [ ] Adjust policies based on feedback
- [ ] Share success stories and best practices
- [ ] Celebrate wins and recognize champions

---

## ROI Calculation

### Time Savings Model

```
Annual Time Savings = 
  (Hours Saved per Developer per Week) × 
  (Number of Active Users) × 
  (52 weeks)

Annual Cost Savings = 
  Annual Time Savings × 
  (Fully Loaded Hourly Cost)

Example:
  2 hours × 100 users × 52 weeks = 10,400 hours
  10,400 hours × $75/hour = $780,000 annual savings

```

### Cost-Benefit Analysis

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Costs["💰 COSTS"]
    C1["Copilot Licenses<br/>$39/user × 100 users × 12 months<br/>= $46,800"]
    C2["Training & Rollout<br/>= $5,000"]
    C3["Administration<br/>5 hrs/month × $100<br/>= $6,000"]
    CT["TOTAL COST: $57,800"]
    C1 --> CT
    C2 --> CT
    C3 --> CT
  end
  
  subgraph Benefits["📈 BENEFITS"]
    B1["Developer Time Savings<br/>= $780,000"]
    B2["Faster Onboarding (20%)<br/>= $25,000"]
    B3["Reduced Context Switching<br/>= $50,000"]
    BT["TOTAL BENEFIT: $855,000"]
    B1 --> BT
    B2 --> BT
    B3 --> BT
  end
  
  CT --> Result["🎯 NET BENEFIT: $797,200<br/>ROI: 1,379%"]
  BT --> Result
</div>
</div>

### Conservative vs Optimistic Estimates

| Scenario | Hours Saved/Week | Users | Annual Savings |
|----------|------------------|-------|----------------|
| Conservative | 1 hour | 100 | $390,000 |
| Moderate | 2 hours | 100 | $780,000 |
| Optimistic | 4 hours | 100 | $1,560,000 |

---

## Reporting Templates

### Weekly Report

```markdown
# Copilot Weekly Report - Week of [DATE]

## Summary Metrics

| Metric | This Week | Last Week | Change |
|--------|-----------|-----------|--------|
| Active Users | 87 | 83 | +4.8% |
| Acceptance Rate | 28% | 26% | +2pp |
| Suggestions/User | 145 | 138 | +5% |

## Highlights
- Adoption increased 5% over last week
- New team onboarded: Platform Engineering
- Top language by volume: TypeScript (45%)

## Concerns
- 13 users with no activity in 14+ days
- API team acceptance rate below average

## Action Items
- [ ] Follow up with 13 inactive users
- [ ] Schedule training session for API team

```

### Executive Summary (Quarterly)

```markdown
# Q4 Copilot Business Review

## Key Results vs Targets

| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Adoption Rate | 85% | 90% | ✅ |
| Acceptance Rate | 22% | 25% | ✅ |
| Developer NPS | 60 | 72 | ✅ |
| Est. Time Saved | 150 hrs/wk | 200 hrs/wk | ✅ |

## Financial Impact
- Estimated annual savings: $780,000
- Cost of Copilot licenses: $57,800
- Net benefit: $722,200
- ROI: 1,249%

## Qualitative Feedback
> "Copilot has changed how I write tests. What used to take
> 30 minutes now takes 5." - Senior Developer

## Recommendations
1. Expand to contractor workforce (50 additional seats)
2. Pilot Copilot Enterprise knowledge bases
3. Increase training investment for new hires

```

---

## Key Takeaways

1. **Track adoption first** - utilization is the foundation of value
2. **Acceptance rate** indicates suggestion quality and relevance
3. **Combine quantitative and qualitative** data for complete picture
4. **Calculate ROI** to justify continued investment
5. **Regular reporting** maintains stakeholder buy-in
6. **Act on insights** - use data to drive improvements
