---
layout: training
title: "Intermediate Phase: Development"
permalink: /intermediate/
---

# 🚀 Intermediate Phase: Development
**Weeks 5-8 | Automation, Security & CI/CD**

---

## Phase Overview

With GitHub fundamentals mastered, it's time to level up! This phase focuses on automation, security, and building production-ready deployment pipelines.

### What You'll Learn

By the end of this phase, you'll understand:

- GitHub Actions for automation and CI/CD
- Security scanning with GitHub Advanced Security (GHAS)
- Dependency management and supply chain security
- Package publishing and artifact management
- Multi-environment deployment strategies
- DORA metrics and DevOps performance measurement

### Phase Context

**Before:** You can navigate GitHub, use Git confidently, and understand basic collaboration workflows.

**After:** You'll build automated pipelines that test code, scan for vulnerabilities, and deploy to production - the heart of modern DevOps.

---

## Modules

### 📘 Module 5: GitHub Actions & Automation Basics
**Duration:** 10-12 hours | **Week:** 5

Learn the fundamentals of GitHub Actions and build your first CI workflows.

**Topics:**

- CI/CD principles and benefits
- GitHub Actions architecture (workflows, jobs, steps, runners)
- Workflow syntax and YAML basics
- Triggering workflows (push, PR, schedule, manual)
- Using Actions from the marketplace
- Secrets and environment variables
- Matrix builds for multiple environments
- Status checks and branch protection integration

**Learning Outcomes:**

- Create automated test workflows
- Use community Actions from the marketplace
- Configure secrets securely
- Integrate status checks with branch protection

[**Start Module 5 →**](/intermediate/module-5/)

---

### 📘 Module 6: Code Security with GitHub Advanced Security
**Duration:** 10-12 hours | **Week:** 6

Master security features to find and fix vulnerabilities before they reach production.

**Topics:**

- Security in the SDLC (shift-left security)
- Dependabot for dependency updates
- Secret scanning to prevent credential leaks
- CodeQL for static code analysis
- Security advisories and CVE reporting
- Supply chain security
- Security policies and SECURITY.md

**Learning Outcomes:**

- Enable and configure all GHAS features
- Triage security alerts effectively
- Create security policies for customers
- Explain supply chain security to stakeholders

[**Start Module 6 →**](/intermediate/module-6/)

---

### 📘 Module 7: Dependency & Package Management
**Duration:** 10-12 hours | **Week:** 7

Understand dependency management, GitHub Packages, and artifact publishing.

**Topics:**

- Dependency management strategies
- GitHub Packages (npm, Maven, NuGet, Docker)
- Publishing packages with Actions
- Versioning and release strategies
- Container registry and Docker images
- Self-hosted runners
- Actions caching and artifacts

**Learning Outcomes:**

- Publish packages to GitHub Packages
- Create container publishing workflows
- Implement versioning strategies
- Optimize workflows with caching

[**Start Module 7 →**](/intermediate/module-7/)

---

### 📘 Module 8: CI/CD Pipelines & Deployment Strategies
**Duration:** 10-12 hours | **Week:** 8

Build complete deployment pipelines with multiple environments and protection rules.

**Topics:**

- Continuous Deployment principles
- GitHub Environments and protection rules
- Multi-environment pipelines (dev, staging, prod)
- Deployment approval workflows
- OIDC for secure cloud authentication
- Reusable workflows
- Deployment strategies (blue/green, canary)
- DORA metrics for measuring performance

**Learning Outcomes:**

- Build complete multi-environment CI/CD pipelines
- Configure environment protection rules
- Implement OIDC for AWS/Azure
- Measure DORA metrics from deployment data

[**Start Module 8 →**](/intermediate/module-8/)

---

## Intermediate Phase Milestone Project

### 🎯 Project: Production-Ready Application Pipeline

**Objective:** Build a complete CI/CD pipeline with security scanning, testing, and multi-environment deployment.

**Requirements:**

1. Fork/create a sample application (Node.js, Python, or Java)
2. Implement automated testing with code coverage reports
3. Enable all GHAS features (Dependabot, secret scanning, CodeQL)
4. Create a package publishing workflow
5. Build a multi-environment deployment pipeline:
   - **Dev:** Deploys automatically on every push to `main`
   - **Staging:** Requires manual approval
   - **Production:** Requires security review + approval
6. Implement OIDC authentication (no long-lived secrets)
7. Add deployment status badges to README
8. Calculate and display DORA metrics (deployment frequency, lead time)

**Deliverables:**

- Live repository with all workflows configured
- Deployed application in 3 environments
- Security scan results with remediation plan
- 15-minute demo walking through the pipeline
- DORA metrics dashboard or report

**Assessment Criteria:**

- Pipeline functionality and reliability (30%)
- Security configuration and best practices (25%)
- Environment protection and approval process (20%)
- Code quality and documentation (15%)
- Presentation and DORA metrics (10%)

---

## Phase Completion Checklist

Before moving to the Advanced phase, ensure you can:

- [ ] Create GitHub Actions workflows from scratch
- [ ] Explain CI/CD benefits to non-technical stakeholders
- [ ] Configure all GitHub Advanced Security features
- [ ] Triage and remediate security vulnerabilities
- [ ] Publish packages to GitHub Packages
- [ ] Build multi-environment deployment pipelines
- [ ] Implement environment protection rules
- [ ] Use OIDC for cloud deployments
- [ ] Calculate and explain DORA metrics
- [ ] Complete the Intermediate Phase Milestone Project

---

## Resources & Support

### Official Training
- [GitHub Skills: GitHub Actions](https://github.com/skills/hello-github-actions)
- [Microsoft Learn: GitHub Actions](https://learn.microsoft.com/en-us/training/modules/github-actions-ci/)
- [GitHub Skills: Secure your repository supply chain](https://github.com/skills/secure-repository-supply-chain)

### Documentation
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [GitHub Advanced Security Documentation](https://docs.github.com/en/code-security)
- [GitHub Packages Documentation](https://docs.github.com/en/packages)

---

## Ready to Continue?

<div class="cta-buttons">
  <a href="/intermediate/module-5/" class="btn-primary">Start Module 5: GitHub Actions →</a>
  <a href="/beginner/" class="btn-secondary">← Back to Beginner Phase</a>
</div>

---

**Next Phase:** Once you complete the Intermediate phase, you'll move to [Advanced Phase](/advanced/) covering enterprise administration, APIs, and governance.
