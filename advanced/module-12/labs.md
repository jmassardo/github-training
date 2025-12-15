---
layout: training-module
title: "Hands-On Labs"
permalink: /advanced/module-12/labs/
module_number: 12
module_title: "Governance, Compliance & DORA Metrics"
section_number: 4
total_sections: 7
phase: advanced
estimated_time: "45 min"
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
  url: /advanced/module-12/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /advanced/module-12/quiz/
  title: "Knowledge Check"
---

## Lab Overview

| Lab | Topic | Time | Difficulty |
|-----|-------|------|------------|
| Lab 1 | Create Repository Ruleset | 10 min | Beginner |
| Lab 2 | Build PR Lead Time Tracker | 15 min | Intermediate |
| Lab 3 | Create Compliance Report | 15 min | Intermediate |
| Lab 4 | Design Metrics Dashboard | 15 min | Advanced |

---

## Lab 1: Create Repository Ruleset

**Objective:** Create an organization-level ruleset that enforces branch protection across all repositories.

### Prerequisites
- Organization owner or admin access
- At least one test repository

### Steps

**Step 1:** Navigate to your organization settings:
- Go to `github.com/orgs/YOUR_ORG/settings/rules`
- Click "New ruleset"

**Step 2:** Configure the ruleset:

```yaml
Name: Default Branch Protection
Enforcement: Active
Target: Default branch

Rules to enable:
☑ Require a pull request before merging
  - Required approvals: 1
  - Dismiss stale approvals: Yes
  - Require review from code owners: Yes
  
☑ Require status checks to pass
  - Require branches to be up to date: Yes
  
☑ Block force pushes

```

**Step 3:** Apply to repositories:
- Under "Target repositories", select "All repositories" or specific repos
- Click "Create"

**Step 4:** Test the ruleset:
1. Go to a test repository
2. Try to push directly to main: `git push origin main`
3. Verify it's blocked

### Verification

✅ Ruleset appears in organization settings  
✅ Direct pushes to main are blocked  
✅ PRs require approval before merging

---

## Lab 2: Build PR Lead Time Tracker

**Objective:** Create a GitHub Action that calculates and logs PR lead time metrics.

### Setup

Create a new file `.github/workflows/pr-metrics.yml` in your repository.

### Steps

**Step 1:** Create the workflow file:

```yaml
name: PR Lead Time Metrics

on:
  pull_request:
    types: [closed]

jobs:
  calculate-lead-time:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    
    steps:
      - name: Calculate Metrics
        uses: actions/github-script@v7
        with:
          script: |
            const pr = context.payload.pull_request;
            
            // Timestamps
            const created = new Date(pr.created_at);
            const merged = new Date(pr.merged_at);
            
            // Lead time calculation
            const leadTimeMs = merged - created;
            const leadTimeHours = leadTimeMs / (1000 * 60 * 60);
            const leadTimeDays = leadTimeHours / 24;
            
            // Get reviews
            const reviews = await github.rest.pulls.listReviews({
              owner: context.repo.owner,
              repo: context.repo.repo,
              pull_number: pr.number
            });
            
            // Time to first review
            let timeToFirstReview = null;
            if (reviews.data.length > 0) {
              const firstReview = new Date(reviews.data[0].submitted_at);
              timeToFirstReview = (firstReview - created) / (1000 * 60 * 60);
            }
            
            // DORA classification
            let classification = 'Low';
            if (leadTimeHours < 1) classification = 'Elite';
            else if (leadTimeHours < 24 * 7) classification = 'High';
            else if (leadTimeHours < 24 * 30) classification = 'Medium';
            
            // Output metrics
            console.log('=== PR METRICS ===');
            console.log(`PR #${pr.number}: ${pr.title}`);
            console.log(`Lead Time: ${leadTimeHours.toFixed(1)} hours (${leadTimeDays.toFixed(1)} days)`);
            console.log(`DORA Classification: ${classification}`);
            console.log(`Time to First Review: ${timeToFirstReview ? timeToFirstReview.toFixed(1) + ' hours' : 'N/A'}`);
            console.log(`Lines Changed: +${pr.additions} / -${pr.deletions}`);
            console.log(`Files Changed: ${pr.changed_files}`);
            console.log(`Review Count: ${reviews.data.length}`);
            
            // Add label based on lead time
            let label = 'lead-time:elite';
            if (classification === 'High') label = 'lead-time:high';
            else if (classification === 'Medium') label = 'lead-time:medium';
            else if (classification === 'Low') label = 'lead-time:low';
            
            await github.rest.issues.addLabels({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: pr.number,
              labels: [label]
            });

```

**Step 2:** Create the labels in your repository:
- `lead-time:elite` (green)
- `lead-time:high` (blue)
- `lead-time:medium` (yellow)
- `lead-time:low` (red)

**Step 3:** Test by merging a PR and checking the workflow output.

### Verification

✅ Workflow runs when PR is merged  
✅ Lead time is calculated correctly  
✅ Appropriate label is applied  
✅ Metrics appear in workflow logs

### Challenge

Extend the workflow to:

- Store metrics in a JSON file
- Calculate weekly averages
- Post summary to a Slack channel

---

## Lab 3: Create Compliance Report

**Objective:** Build a script that audits repository security settings and generates a compliance report.

### Setup

This lab uses the GitHub CLI. Ensure you have `gh` installed and authenticated.

### Steps

**Step 1:** Create `compliance-check.sh`:

```bash
#!/bin/bash

ORG="${1:-your-org}"
OUTPUT_FILE="compliance-report-$(date +%Y%m%d).md"

echo "# Compliance Report" > $OUTPUT_FILE
echo "" >> $OUTPUT_FILE
echo "**Organization:** $ORG" >> $OUTPUT_FILE
echo "**Date:** $(date)" >> $OUTPUT_FILE
echo "" >> $OUTPUT_FILE

# Get all repos
repos=$(gh repo list $ORG --json name,visibility,isArchived --limit 1000 -q '.[] | select(.isArchived == false) | .name')

total=0
protected=0
has_codeowners=0
has_security_policy=0
vuln_alerts=0

echo "| Repository | Branch Protection | CODEOWNERS | Security Policy | Vuln Alerts |" >> $OUTPUT_FILE
echo "|------------|------------------|------------|-----------------|-------------|" >> $OUTPUT_FILE

for repo in $repos; do
  ((total++))
  
  # Check branch protection
  bp_status="❌"
  if gh api "repos/$ORG/$repo/branches/main/protection" &>/dev/null; then
    bp_status="✅"
    ((protected++))
  fi
  
  # Check CODEOWNERS
  co_status="❌"
  if gh api "repos/$ORG/$repo/contents/CODEOWNERS" &>/dev/null || \
     gh api "repos/$ORG/$repo/contents/.github/CODEOWNERS" &>/dev/null; then
    co_status="✅"
    ((has_codeowners++))
  fi
  
  # Check SECURITY.md
  sec_status="❌"
  if gh api "repos/$ORG/$repo/contents/SECURITY.md" &>/dev/null; then
    sec_status="✅"
    ((has_security_policy++))
  fi
  
  # Check vulnerability alerts
  va_status="❌"
  if gh api "repos/$ORG/$repo/vulnerability-alerts" &>/dev/null; then
    va_status="✅"
    ((vuln_alerts++))
  fi
  
  echo "| $repo | $bp_status | $co_status | $sec_status | $va_status |" >> $OUTPUT_FILE
done

# Summary
echo "" >> $OUTPUT_FILE
echo "## Summary" >> $OUTPUT_FILE
echo "" >> $OUTPUT_FILE
echo "| Metric | Count | Percentage |" >> $OUTPUT_FILE
echo "|--------|-------|------------|" >> $OUTPUT_FILE
echo "| Total Repositories | $total | 100% |" >> $OUTPUT_FILE
echo "| Branch Protection | $protected | $(echo "scale=1; $protected * 100 / $total" | bc)% |" >> $OUTPUT_FILE
echo "| CODEOWNERS | $has_codeowners | $(echo "scale=1; $has_codeowners * 100 / $total" | bc)% |" >> $OUTPUT_FILE
echo "| Security Policy | $has_security_policy | $(echo "scale=1; $has_security_policy * 100 / $total" | bc)% |" >> $OUTPUT_FILE
echo "| Vulnerability Alerts | $vuln_alerts | $(echo "scale=1; $vuln_alerts * 100 / $total" | bc)% |" >> $OUTPUT_FILE

echo "Report generated: $OUTPUT_FILE"

```

**Step 2:** Make executable and run:

```bash
chmod +x compliance-check.sh
./compliance-check.sh your-org

```

**Step 3:** Review the generated markdown report.

### Verification

✅ Script runs without errors  
✅ Report includes all repositories  
✅ Summary shows percentages  
✅ Non-compliant repos are easily identifiable

---

## Lab 4: Design Metrics Dashboard

**Objective:** Create a simple metrics dashboard using GitHub Pages and the API.

### Setup

Create a new repository or use an existing one.

### Steps

**Step 1:** Create `docs/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Engineering Metrics Dashboard</title>
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; margin: 40px; background: #f6f8fa; }
        .dashboard { max-width: 1200px; margin: 0 auto; }
        h1 { color: #24292f; }
        .metrics-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; margin-top: 20px; }
        .metric-card { background: white; border-radius: 8px; padding: 20px; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
        .metric-value { font-size: 48px; font-weight: bold; color: #238636; }
        .metric-label { color: #57606a; margin-top: 8px; }
        .metric-trend { font-size: 14px; margin-top: 4px; }
        .trend-up { color: #238636; }
        .trend-down { color: #cf222e; }
        .benchmark { font-size: 12px; color: #8b949e; margin-top: 8px; }
        .chart-container { background: white; border-radius: 8px; padding: 20px; margin-top: 20px; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
    </style>
</head>
<body>
    <div class="dashboard">
        <h1>🚀 Engineering Metrics Dashboard</h1>
        <p>Last updated: <span id="lastUpdated">-</span></p>
        
        <div class="metrics-grid">
            <div class="metric-card">
                <div class="metric-value" id="deployFreq">-</div>

                <div class="metric-label">Deployment Frequency</div>

                <div class="metric-trend" id="deployTrend"></div>

                <div class="benchmark">Elite: Multiple per day</div>

            </div>

            
            <div class="metric-card">
                <div class="metric-value" id="leadTime">-</div>

                <div class="metric-label">Lead Time (hours)</div>

                <div class="metric-trend" id="leadTimeTrend"></div>

                <div class="benchmark">Elite: &lt; 1 hour</div>

            </div>

            
            <div class="metric-card">
                <div class="metric-value" id="mttr">-</div>

                <div class="metric-label">MTTR (hours)</div>

                <div class="metric-trend" id="mttrTrend"></div>

                <div class="benchmark">Elite: &lt; 1 hour</div>

            </div>

            
            <div class="metric-card">
                <div class="metric-value" id="cfr">-</div>

                <div class="metric-label">Change Failure Rate</div>

                <div class="metric-trend" id="cfrTrend"></div>

                <div class="benchmark">Elite: 0-15%</div>

            </div>

        </div>

        
        <div class="chart-container">
            <h3>Weekly Trends</h3>
            <canvas id="trendsChart" width="400" height="200"></canvas>
        </div>

    </div>

    
    <script>
        // Sample data - replace with API calls
        const metrics = {
            deployFreq: { value: 4.2, trend: '+0.5', direction: 'up' },
            leadTime: { value: 8.3, trend: '-2.1', direction: 'up' },
            mttr: { value: 2.1, trend: '-0.5', direction: 'up' },
            cfr: { value: '12%', trend: '-3%', direction: 'up' }
        };
        
        document.getElementById('deployFreq').textContent = metrics.deployFreq.value + '/day';
        document.getElementById('leadTime').textContent = metrics.leadTime.value;
        document.getElementById('mttr').textContent = metrics.mttr.value;
        document.getElementById('cfr').textContent = metrics.cfr.value;
        
        document.getElementById('lastUpdated').textContent = new Date().toLocaleDateString();
        
        // Add trend indicators
        function setTrend(elementId, trend, direction) {
            const el = document.getElementById(elementId);
            el.textContent = (direction === 'up' ? '↑' : '↓') + ' ' + trend + ' vs last week';
            el.className = 'metric-trend trend-' + direction;
        }
        
        setTrend('deployTrend', metrics.deployFreq.trend, metrics.deployFreq.direction);
        setTrend('leadTimeTrend', metrics.leadTime.trend, metrics.leadTime.direction);
        setTrend('mttrTrend', metrics.mttr.trend, metrics.mttr.direction);
        setTrend('cfrTrend', metrics.cfr.trend, metrics.cfr.direction);
    </script>
</body>
</html>

```

**Step 2:** Create `docs/metrics.json` for data storage:

```json
{
  "updated": "2024-01-15",
  "current": {
    "deployment_frequency": 4.2,
    "lead_time_hours": 8.3,
    "mttr_hours": 2.1,
    "change_failure_rate": 12
  },
  "history": [
    {"week": "2024-W01", "df": 3.8, "lt": 10.4, "mttr": 2.6, "cfr": 15},
    {"week": "2024-W02", "df": 4.2, "lt": 8.3, "mttr": 2.1, "cfr": 12}
  ]
}

```

**Step 3:** Enable GitHub Pages:
- Go to Settings → Pages
- Source: Deploy from branch
- Branch: main, /docs folder

**Step 4:** Create workflow to update metrics:

```yaml
# .github/workflows/update-metrics.yml
name: Update Metrics Dashboard

on:
  schedule:
    - cron: '0 9 * * 1'  # Weekly
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Collect Metrics
        uses: actions/github-script@v7
        with:
          script: |
            // Collect metrics and update docs/metrics.json
            const fs = require('fs');
            
            // Your metrics collection logic here
            const metrics = {
              updated: new Date().toISOString().split('T')[0],
              current: {
                deployment_frequency: 4.5,
                lead_time_hours: 7.8,
                mttr_hours: 1.9,
                change_failure_rate: 11
              }
            };
            
            fs.writeFileSync('docs/metrics.json', JSON.stringify(metrics, null, 2));
            
      - name: Commit Updates
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add docs/metrics.json
          git commit -m "Update metrics data" || exit 0
          git push

```

### Verification

✅ Dashboard accessible via GitHub Pages  
✅ Metrics display correctly  
✅ Weekly updates automated  
✅ Trends show improvement/regression

---

## Lab Summary

| Lab | Skills Practiced |
|-----|------------------|
| Lab 1 | Organization governance, rulesets |
| Lab 2 | GitHub Actions, PR metrics, DORA |
| Lab 3 | Compliance auditing, shell scripting |
| Lab 4 | Dashboard design, data visualization |

---

**Next:** Test your knowledge in the Quiz section.
