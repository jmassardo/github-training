---
layout: training-module
title: "Guided Walkthrough"
permalink: /advanced/module-12/walkthrough/
module_number: 12
module_title: "Governance, Compliance & DORA Metrics"
section_number: 3
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
  url: /advanced/module-12/concepts/
  title: "Core Concepts"
next_section:
  url: /advanced/module-12/labs/
  title: "Hands-On Labs"
---

## Walkthrough: Building a Governance & Metrics Program

This walkthrough guides you through implementing enterprise governance and DORA metrics collection for a GitHub organization.

## Part 1: Governance Implementation

### Step 1: Enterprise Policy Configuration

Navigate to **Enterprise Settings → Policies** and configure:

**Repository Policies:**

```
Base permissions: Read
Repository creation: Disabled (admins only)
Repository deletion: Disabled (admins only)
Repository visibility change: Disabled (admins only)
Forking: Disabled for private/internal

```

**Actions Policies:**

```
Enabled for: All organizations
Allowed actions: Actions created by GitHub + verified creators
Fork pull request workflows: Require approval for first-time contributors
Default workflow permissions: Read repository contents

```

**Security Policies:**

```
Require 2FA: Enabled
Secret scanning: Enabled for all repositories
Push protection: Enabled
Dependabot alerts: Enabled
Dependabot security updates: Enabled

```

### Step 2: Create Organization Rulesets

Create rulesets that apply across all repositories:

**Production Branch Ruleset:**

```yaml
# Via API or UI: Organization Settings → Rulesets → New ruleset
name: "Production Branch Protection"
enforcement: active
targets:
  - ref_name:
      include: ["~DEFAULT_BRANCH", "refs/heads/release/*"]

rules:
  - type: pull_request
    parameters:
      required_approving_review_count: 1
      dismiss_stale_reviews_on_push: true
      require_code_owner_review: true
      require_last_push_approval: true
      
  - type: required_status_checks
    parameters:
      strict_required_status_checks_policy: true
      required_status_checks:
        - context: "build"
          integration_id: null  # Any integration
          
  - type: non_fast_forward

bypass_actors:
  - actor_id: 1234  # Release automation app
    actor_type: Integration
    bypass_mode: always

```

### Step 3: Audit Log Streaming

Configure audit log streaming to your SIEM or storage:

**For AWS S3:**
1. Go to **Enterprise Settings → Audit Log → Log streaming**
2. Select **Amazon S3**
3. Configure:
   - Bucket: `your-audit-logs-bucket`
   - Region: `us-east-1`
   - Access Key ID / Secret (or use OIDC)

**Sample audit log query (after streaming):**

```sql
-- Find all repository visibility changes
SELECT 
  timestamp,
  actor,
  repo,
  action,
  data.old_visibility,
  data.new_visibility
FROM github_audit_log
WHERE action = 'repo.access'
  AND timestamp > NOW() - INTERVAL '30 days'
ORDER BY timestamp DESC;

```

## Part 2: DORA Metrics Implementation

### Step 4: Define Deployment Events

Create a workflow that records deployments:

```yaml
# .github/workflows/deploy-production.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v4
      
      - name: Deploy Application
        run: |
          # Your deployment logic here
          echo "Deploying to production..."
          
      - name: Record Deployment
        uses: actions/github-script@v7
        with:
          script: |
            const deployment = await github.rest.repos.createDeployment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              ref: context.sha,
              environment: 'production',
              auto_merge: false,
              required_contexts: []
            });
            
            await github.rest.repos.createDeploymentStatus({
              owner: context.repo.owner,
              repo: context.repo.repo,
              deployment_id: deployment.data.id,
              state: 'success',
              environment_url: 'https://app.example.com'
            });
            
            console.log(`Deployment ${deployment.data.id} created`);

```

### Step 5: Collect PR Metrics

Create a workflow that collects PR lead time data:

```yaml
# .github/workflows/metrics-collector.yml
name: Collect PR Metrics

on:
  pull_request:
    types: [closed]

jobs:
  collect-metrics:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    steps:
      - name: Calculate Lead Time
        uses: actions/github-script@v7
        with:
          script: |
            const pr = context.payload.pull_request;
            
            // Calculate lead time
            const created = new Date(pr.created_at);
            const merged = new Date(pr.merged_at);
            const leadTimeMs = merged - created;
            const leadTimeHours = leadTimeMs / (1000 * 60 * 60);
            
            // Get first commit time
            const commits = await github.rest.pulls.listCommits({
              owner: context.repo.owner,
              repo: context.repo.repo,
              pull_number: pr.number
            });
            
            const firstCommit = new Date(commits.data[0].commit.author.date);
            const codeToMergeHours = (merged - firstCommit) / (1000 * 60 * 60);
            
            // Log metrics (or send to your metrics system)
            const metrics = {
              pr_number: pr.number,
              lead_time_hours: leadTimeHours.toFixed(2),
              code_to_merge_hours: codeToMergeHours.toFixed(2),
              additions: pr.additions,
              deletions: pr.deletions,
              changed_files: pr.changed_files,
              reviews: pr.requested_reviewers.length
            };
            
            console.log('PR Metrics:', JSON.stringify(metrics, null, 2));
            
            // Optionally: Send to external metrics system
            // await fetch('https://metrics.example.com/api/pr', {
            //   method: 'POST',
            //   body: JSON.stringify(metrics)
            // });

```

### Step 6: Build Metrics Dashboard

Create a scheduled workflow to aggregate metrics:

```yaml
# .github/workflows/dora-report.yml
name: Weekly DORA Report

on:
  schedule:
    - cron: '0 9 * * 1'  # Every Monday at 9 AM
  workflow_dispatch:

jobs:
  generate-report:
    runs-on: ubuntu-latest
    steps:
      - name: Collect DORA Metrics
        uses: actions/github-script@v7
        id: metrics
        with:
          script: |
            const owner = context.repo.owner;
            const oneWeekAgo = new Date();
            oneWeekAgo.setDate(oneWeekAgo.getDate() - 7);
            
            // Get all repos in org
            const repos = await github.paginate(github.rest.repos.listForOrg, {
              org: owner,
              per_page: 100
            });
            
            let totalDeployments = 0;
            let totalPRs = 0;
            let totalLeadTime = 0;
            let failures = 0;
            
            for (const repo of repos) {
              // Count deployments
              const deployments = await github.rest.repos.listDeployments({
                owner,
                repo: repo.name,
                per_page: 100
              });
              
              const weekDeployments = deployments.data.filter(d => 
                new Date(d.created_at) > oneWeekAgo
              );
              totalDeployments += weekDeployments.length;
              
              // Get merged PRs
              const prs = await github.rest.pulls.list({
                owner,
                repo: repo.name,
                state: 'closed',
                sort: 'updated',
                direction: 'desc',
                per_page: 100
              });
              
              const mergedPRs = prs.data.filter(pr => 
                pr.merged_at && new Date(pr.merged_at) > oneWeekAgo
              );
              totalPRs += mergedPRs.length;
              
              // Calculate lead times
              for (const pr of mergedPRs) {
                const leadTime = (new Date(pr.merged_at) - new Date(pr.created_at)) / (1000 * 60 * 60);
                totalLeadTime += leadTime;
              }
            }
            
            const avgLeadTime = totalPRs > 0 ? totalLeadTime / totalPRs : 0;
            const deploymentFrequency = totalDeployments / 7;  // Per day
            
            const report = {
              period: 'Last 7 days',
              deployment_frequency: deploymentFrequency.toFixed(2) + '/day',
              average_lead_time: avgLeadTime.toFixed(2) + ' hours',
              total_prs_merged: totalPRs,
              total_deployments: totalDeployments,
              repos_analyzed: repos.length
            };
            
            core.setOutput('report', JSON.stringify(report));
            return report;
            
      - name: Create Report Issue
        uses: actions/github-script@v7
        with:
          script: |
            const report = JSON.parse('${{ steps.metrics.outputs.report }}');
            
            const body = `## Weekly DORA Metrics Report
            
            **Period:** ${report.period}
            
            ### Key Metrics
            
            | Metric | Value | DORA Benchmark |
            |--------|-------|----------------|
            | Deployment Frequency | ${report.deployment_frequency} | Elite: Multiple/day |
            | Average Lead Time | ${report.average_lead_time} | Elite: < 1 hour |
            | PRs Merged | ${report.total_prs_merged} | - |
            | Total Deployments | ${report.total_deployments} | - |
            
            ### Analysis
            
            Repositories analyzed: ${report.repos_analyzed}
            
            ---
            *Generated automatically by DORA metrics workflow*`;
            
            await github.rest.issues.create({
              owner: context.repo.owner,
              repo: context.repo.repo,
              title: `DORA Report - Week of ${new Date().toISOString().split('T')[0]}`,
              body: body,
              labels: ['metrics', 'dora', 'automated']
            });

```

## Part 3: Compliance Reporting

### Step 7: Security Posture Dashboard

Create a compliance check workflow:

```yaml
# .github/workflows/compliance-check.yml
name: Weekly Compliance Check

on:
  schedule:
    - cron: '0 8 * * 1'
  workflow_dispatch:

jobs:
  check-compliance:
    runs-on: ubuntu-latest
    steps:
      - name: Check Repository Compliance
        uses: actions/github-script@v7
        with:
          script: |
            const org = context.repo.owner;
            
            const repos = await github.paginate(github.rest.repos.listForOrg, {
              org,
              per_page: 100
            });
            
            const compliance = {
              total_repos: repos.length,
              has_branch_protection: 0,
              has_codeowners: 0,
              has_security_policy: 0,
              vulnerability_alerts_enabled: 0,
              secret_scanning_enabled: 0,
              non_compliant: []
            };
            
            for (const repo of repos) {
              let isCompliant = true;
              const issues = [];
              
              // Check branch protection
              try {
                await github.rest.repos.getBranchProtection({
                  owner: org,
                  repo: repo.name,
                  branch: repo.default_branch
                });
                compliance.has_branch_protection++;
              } catch (e) {
                issues.push('No branch protection');
                isCompliant = false;
              }
              
              // Check for CODEOWNERS
              try {
                await github.rest.repos.getContent({
                  owner: org,
                  repo: repo.name,
                  path: 'CODEOWNERS'
                });
                compliance.has_codeowners++;
              } catch (e) {
                try {
                  await github.rest.repos.getContent({
                    owner: org,
                    repo: repo.name,
                    path: '.github/CODEOWNERS'
                  });
                  compliance.has_codeowners++;
                } catch (e2) {
                  issues.push('No CODEOWNERS file');
                }
              }
              
              // Check vulnerability alerts
              if (repo.has_vulnerability_alerts) {
                compliance.vulnerability_alerts_enabled++;
              } else {
                issues.push('Vulnerability alerts disabled');
                isCompliant = false;
              }
              
              if (!isCompliant) {
                compliance.non_compliant.push({
                  repo: repo.name,
                  issues: issues
                });
              }
            }
            
            // Calculate percentages
            const pct = (n) => ((n / compliance.total_repos) * 100).toFixed(1);
            
            console.log(`\n📊 Compliance Summary\n`);
            console.log(`Total Repositories: ${compliance.total_repos}`);
            console.log(`Branch Protection: ${pct(compliance.has_branch_protection)}%`);
            console.log(`CODEOWNERS: ${pct(compliance.has_codeowners)}%`);
            console.log(`Vulnerability Alerts: ${pct(compliance.vulnerability_alerts_enabled)}%`);
            console.log(`\nNon-compliant repos: ${compliance.non_compliant.length}`);
            
            if (compliance.non_compliant.length > 0) {
              console.log('\nNon-compliant repositories:');
              for (const nc of compliance.non_compliant.slice(0, 10)) {
                console.log(`  - ${nc.repo}: ${nc.issues.join(', ')}`);
              }
            }

```

### Step 8: Audit Log Analysis

Query patterns for common compliance questions:

```bash
# Using GitHub CLI to query audit log

# Find all admin permission grants
gh api /orgs/ORG/audit-log?phrase=action:repo.add_member+permission:admin

# Find repository deletions
gh api /orgs/ORG/audit-log?phrase=action:repo.destroy

# Find branch protection changes
gh api /orgs/ORG/audit-log?phrase=action:protected_branch.policy_override

# Find failed login attempts (Enterprise)
gh api /enterprises/ENTERPRISE/audit-log?phrase=action:user.failed_login

```

## Walkthrough Summary

You've implemented:

| Component | Purpose |
|-----------|---------|
| Enterprise Policies | Enforce security baseline |
| Repository Rulesets | Consistent branch protection |
| Audit Log Streaming | Compliance documentation |
| Deployment Tracking | DORA: Deployment Frequency |
| PR Metrics | DORA: Lead Time |
| Weekly Reports | Automated metrics aggregation |
| Compliance Checks | Security posture monitoring |

---

**Next:** Practice these implementations in the Hands-On Labs.
