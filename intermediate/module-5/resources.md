---
layout: training-module
title: "Module 5, Section 6: Resources"
description: "Documentation, tools, and references for GitHub Actions"
permalink: /intermediate/module-5/resources/
module_number: 5
section_number: 6
phase: intermediate
prev_section:
  url: /intermediate/module-5/quiz/
  title: "Knowledge Check"
next_section:
  url: /intermediate/module-5/scenarios/
  title: "CSM Scenarios"
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

## Official Documentation
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax Reference](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)
- [Context and Expression Syntax](https://docs.github.com/en/actions/learn-github-actions/contexts)
- [Events that Trigger Workflows](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)
- [Environment Variables](https://docs.github.com/en/actions/learn-github-actions/environment-variables)
- [Encrypted Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- [OIDC Authentication](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/about-security-hardening-with-openid-connect)
---

## Actions Marketplace
- [GitHub Marketplace](https://github.com/marketplace?type=actions)
- [actions/checkout](https://github.com/actions/checkout)
- [actions/setup-node](https://github.com/actions/setup-node)
- [actions/cache](https://github.com/actions/cache)
- [docker/build-push-action](https://github.com/docker/build-push-action)

## Learning Resources
- [GitHub Actions: The Full Course (Microsoft Learn)](https://docs.microsoft.com/en-us/learn/paths/automate-workflow-github-actions/)
- [Actions Runner Controller](https://github.com/actions/actions-runner-controller) - Kubernetes runner scaling
- [act](https://github.com/nektos/act) - Run Actions locally
---

## GitHub CLI for Actions

```bash
# List workflows
gh workflow list
# View workflow runs
gh run list
gh run view <run-id>
# Trigger workflow
gh workflow run <workflow-name>
gh workflow run ci.yml -f environment=staging
# View workflow logs
gh run view <run-id> --log
# Watch running workflow
gh run watch

```


