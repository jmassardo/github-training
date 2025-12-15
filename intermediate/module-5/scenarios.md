---
layout: training-module
title: "Module 5, Section 7: Real-World CSM Scenarios"
description: "Practice handling customer questions about GitHub Actions"
permalink: /intermediate/module-5/scenarios/
module_number: 5
section_number: 7
phase: intermediate
prev_section:
  url: /intermediate/module-5/resources/
  title: "Resources"
next_section:
  url: /intermediate/module-6/
  title: "Module 6: Security & GHAS"
toc: true
module_title: "GitHub Actions & Automation"
total_sections: 7
module_index: /intermediate/module-5/
sections:
  - title: "Context & Overview"
    url: "/intermediate/module-5/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "20 min"
  - title: "Core Concepts"
    url: "/intermediate/module-5/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/intermediate/module-5/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "50 min"
  - title: "Hands-On Labs"
    url: "/intermediate/module-5/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/intermediate/module-5/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "25 min"
  - title: "Resources"
    url: "/intermediate/module-5/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/intermediate/module-5/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "30 min"
---
{% raw %}

## Scenario 1: Jenkins Migration

### Situation
A customer is migrating from Jenkins to GitHub Actions. They have 200+ Jenkins jobs and are concerned about the migration effort and feature parity.

### Customer Question
"We've invested heavily in Jenkins. Can GitHub Actions do everything Jenkins does? How do we migrate?"

### Your Response
"Great question! Let me address feature parity and migration strategy:
**Feature Comparison:**

| Jenkins Feature | GitHub Actions Equivalent |
|----------------|--------------------------|
| Pipeline as Code | Workflow YAML |
| Shared Libraries | Reusable Workflows |
| Plugins | Marketplace Actions |
| Agents | Runners (hosted/self-hosted) |
| Jenkinsfile | .github/workflows/*.yml |
| Parameters | workflow_dispatch inputs |
| Stages | Jobs |
| Parallel stages | Matrix strategy |
| Credentials | Secrets |
**Migration Approach:**

1. **Assessment Phase (Week 1-2):**
   - Inventory all Jenkins jobs
   - Categorize by complexity (simple, medium, complex)
   - Identify shared libraries and plugins used
2. **Pilot Phase (Week 3-4):**
   - Convert 5-10 representative pipelines
   - Start with simple builds, add complexity
   - Use GitHub Actions Importer tool:
   ```bash
   gh actions-importer audit jenkins --output-dir audit
   gh actions-importer dry-run jenkins --source-url http://jenkins --output-dir output
   ```

3. **Migration Phase (Week 5+):**
   - Convert in batches
   - Run parallel (Jenkins + Actions) initially
   - Cut over once validated
4. **Optimization Phase:**
   - Create reusable workflows for common patterns
   - Implement caching for performance
   - Set up organization-level secrets
**What GitHub Actions does better:**

- Native GitHub integration (PRs, issues, deployments)
- No infrastructure to manage (hosted runners)
- Modern YAML syntax
- Built-in secret scanning on workflow files
Would you like me to help with a specific pipeline migration?"
---

## Scenario 2: Cost Optimization

### Situation
A customer's GitHub Actions usage has grown significantly, and they're seeing high costs from runner minutes.

### Customer Question
"Our Actions costs have tripled this quarter. How can we optimize without slowing down our development?"

### Your Response
"Let's analyze and optimize your Actions usage:
**Quick Wins (Immediate Impact):**

1. **Add path filters:**

```yaml
on:
  push:
    paths:
      - 'src/**'
      - '!**/*.md'

```

2. **Cancel redundant runs:**

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

```

3. **Optimize caching:**

```yaml
- uses: actions/cache@v4
  with:
    path: node_modules
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}

```

**Medium-Term Optimizations:**

4. **Self-hosted runners for heavy jobs:**
   - macOS minutes are 10x Linux cost
   - Consider self-hosted Mac Minis for iOS builds
   
5. **Larger runners for parallelization:**
   - Sometimes 1 large runner is cheaper than 4 small
   - Especially for compilation-heavy workflows
6. **Job consolidation:**

```yaml
# Instead of 3 separate jobs, each with checkout/setup
jobs:
  all-checks:
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run lint
      - run: npm test
      - run: npm run build

```

**Analysis Tools:**

```bash
# Get usage report
gh api /orgs/{org}/actions/usage
# Identify expensive workflows
gh run list --limit 100 --json conclusion,status,workflowName,createdAt

```

**Cost Reduction Checklist:**

- [ ] Add path filters to all workflows
- [ ] Implement concurrency cancellation
- [ ] Cache dependencies properly
- [ ] Use `ubuntu-latest` instead of `macos-latest` where possible
- [ ] Consider self-hosted for high-volume repos
- [ ] Review and remove unused workflows
Shall I help audit your specific workflows?"

## Scenario 3: Security Hardening

### Situation
A customer in a regulated industry needs to ensure their CI/CD pipelines meet security requirements.

### Customer Question
"Our security team is concerned about supply chain attacks through CI/CD. How do we secure our GitHub Actions?"

### Your Response
"Excellent focus on supply chain security. Here's a comprehensive hardening guide:
**1. Pin Actions to SHA:**

```yaml
# Instead of
- uses: actions/checkout@v4
# Use commit SHA
- uses: actions/checkout@8ade135a41bc03ea155e62e844d188df1ea18608

```

**2. Minimize GITHUB_TOKEN permissions:**

```yaml
permissions:
  contents: read    # Minimum required
  
jobs:
  deploy:
    permissions:
      contents: read
      deployments: write  # Only what's needed

```

**3. Use environments with protection:**

```yaml
jobs:
  deploy-prod:
    environment: production  # Requires approval
    steps:
      - run: ./deploy.sh

```

**4. Secure secrets handling:**

- Use environment-scoped secrets for production
- Enable secret scanning on workflow files
- Consider HashiCorp Vault for dynamic secrets
**5. Workflow security settings:**

- Repository Settings → Actions → General
  - ☑ Require approval for first-time contributors
  - ☑ Disable Actions for fork PRs (or require approval)
  - Limit Actions to specific repositories/actions
**6. Use OIDC instead of long-lived secrets:**

```yaml
permissions:
  id-token: write
- uses: aws-actions/configure-aws-credentials@v4
  with:
    role-to-assume: arn:aws:iam::123456789:role/deploy

```

**7. Add security scanning to workflows:**

```yaml
- uses: github/codeql-action/analyze@v3
- uses: snyk/actions/node@master

```

**8. Audit workflow changes:**

- Use branch protection for `.github/workflows/`
- Require CODEOWNERS review for workflow changes
- Monitor audit log for workflow modifications
**Security Checklist:**

- [ ] All third-party actions pinned to SHA
- [ ] GITHUB_TOKEN permissions minimized
- [ ] Production deploys use environments with approval
- [ ] OIDC used for cloud authentication
- [ ] Workflows require CODEOWNER approval to modify
- [ ] Secret scanning enabled
- [ ] Fork PR workflows require approval
Would you like me to review your specific workflows for security issues?"
---

## Scenario 4: Scaling Self-Hosted Runners

### Situation
A customer has specialized build requirements (GPU, specific hardware) and needs to scale their self-hosted runner infrastructure.

### Customer Question
"We need GPU runners for ML model training. How do we set this up and scale it appropriately?"

### Your Response
"Let me walk you through a scalable self-hosted runner architecture:
**Architecture Options:**

1. **Static Runners (Simple):**
   - Persistent VMs registered as runners
   - Good for: Consistent, predictable load
   - Drawback: Pay for idle capacity
2. **Actions Runner Controller (ARC) on Kubernetes:**
   - Runners scale based on demand
   - Good for: Variable load, existing K8s
   - GPU support via NVIDIA device plugin
3. **Ephemeral Runners with Auto-scaling:**
   - New runner per job, destroyed after
   - Good for: Security, clean environment
   - Can use AWS/Azure/GCP auto-scaling groups
**For GPU Runners (Recommended: ARC on Kubernetes):**

```yaml
# RunnerDeployment for GPU runners
apiVersion: actions.summerwind.dev/v1alpha1
kind: RunnerDeployment
metadata:
  name: gpu-runners
spec:
  replicas: 2
  template:
    spec:
      repository: your-org/ml-training
      labels:
        - gpu
        - cuda
      resources:
        limits:
          nvidia.com/gpu: 1
      nodeSelector:
        gpu: 'true'

```

**Workflow usage:**

```yaml
jobs:
  train-model:
    runs-on: [self-hosted, gpu, cuda]
    steps:
      - uses: actions/checkout@v4
      - name: Train model
        run: python train.py

```

**Auto-scaling configuration:**

```yaml
apiVersion: actions.summerwind.dev/v1alpha1
kind: HorizontalRunnerAutoscaler
metadata:
  name: gpu-runners-autoscaler
spec:
  scaleTargetRef:
    name: gpu-runners
  minReplicas: 1
  maxReplicas: 10
  metrics:
    - type: PercentageRunnersBusy
      scaleUpThreshold: '0.75'
      scaleDownThreshold: '0.25'
      scaleUpFactor: '2'
      scaleDownFactor: '0.5'

```

**Best Practices:**

- Use ephemeral runners for security
- Label runners by capability (gpu, high-memory, etc.)
- Monitor runner utilization
- Set up alerting for runner failures
- Regular security patching
Would you like help setting up ARC or another scaling solution?"

## Module Summary

### Key Takeaways
1. **Workflows are Event-Driven**
   - Multiple trigger types (push, PR, schedule, manual)
   - Path and branch filtering for efficiency
   - Concurrency controls prevent duplicate runs
2. **Jobs and Steps are the Building Blocks**
   - Jobs run in parallel by default
   - Steps run sequentially within jobs
   - Use `needs` for job dependencies
3. **Matrix Builds Enable Cross-Platform Testing**
   - Test across OS, language versions simultaneously
   - Use `include`/`exclude` for special cases
   - `fail-fast: false` continues testing on failures
4. **Reusable Workflows Enable DRY CI/CD**
   - Create once, use everywhere
   - Pass inputs, secrets, and receive outputs
   - Version with branches or tags
5. **Security is Multi-Layered**
   - Pin actions to SHA
   - Minimize GITHUB_TOKEN permissions
   - Use OIDC for cloud authentication
   - Protect sensitive environments
6. **Performance Matters**
   - Cache dependencies aggressively
   - Use path filters
   - Cancel redundant runs
   - Right-size runners for workloads
---

## What's Next
In **Module 6: Code Security with GHAS**, you'll learn how to:

- Implement code scanning with CodeQL
- Configure Dependabot for dependency updates
- Set up secret scanning
- Create security policies
The CI/CD pipelines you've built here become even more powerful when combined with GitHub's security features to shift security left in your development process.

## Checklist: Module 5 Complete
- [ ] Created a CI workflow with linting and testing
- [ ] Implemented matrix testing
- [ ] Set up caching for dependencies
- [ ] Created a reusable workflow
- [ ] Configured environment deployments
- [ ] Completed all three labs
- [ ] Reviewed real-world scenarios
{% endraw %}
