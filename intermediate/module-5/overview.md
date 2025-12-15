---
layout: training-module
title: "Module 5, Section 1: Context & Overview"
description: "Understand the evolution of CI/CD and why GitHub Actions matters"
permalink: /intermediate/module-5/overview/
module_number: 5
section_number: 1
phase: intermediate
prev_section:
  url: /intermediate/module-5/
  title: "Module 5 Home"
next_section:
  url: /intermediate/module-5/concepts/
  title: "Core Concepts"
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

## The Evolution of CI/CD
Before GitHub Actions (2019), teams faced a fragmented automation landscape:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    subgraph TRADITIONAL["❌ Traditional CI/CD Stack"]
        direction LR
        SC1["Source Code<br/>GitHub/GitLab<br/>Bitbucket<br/>Azure Repos"]
        CI1["CI Server<br/>Jenkins<br/>CircleCI<br/>Travis CI"]
        DEP1["Deploy<br/>Scripts<br/>Octopus<br/>Spinnaker"]
    end
    SC1 --> CI1 --> DEP1
    TRADITIONAL --> PAIN["⚠️ Multiple vendors, contexts, credentials"]
</div>
</div>

GitHub Actions unified this:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    subgraph GITHUB["✅ GitHub Actions Model"]
        direction LR
        CODE["📁 Code<br/>(Repo)"]
        ACTIONS["⚡ Actions<br/>(CI/CD)"]
        DEPLOY["🚀 Deploy<br/>(Envs)"]
    end
    CODE --> ACTIONS --> DEPLOY
    GITHUB --> BENEFIT["✨ Native integration, single platform"]
</div>
</div>

---

## Why GitHub Actions Matters

| Feature | Benefit |
|---------|---------|
| Native Integration | No webhooks to configure, built into every repo |
| Matrix Builds | Test across OS, language versions in parallel |
| Marketplace | 15,000+ pre-built actions |
| Runners | GitHub-hosted or self-hosted |
| Secrets Management | Built-in encrypted secrets |
| Environments | Deployment gates and approvals |
| Reusable Workflows | DRY principles for CI/CD |

## What You'll Learn
This module covers:

- GitHub Actions architecture and YAML syntax
- Building CI pipelines (test, build, lint)
- Working with the Actions Marketplace
- Creating custom actions
- Security best practices
- Performance optimization
---

## Customer Success Context
As a CSM, you'll help customers:

- Migrate from Jenkins, CircleCI, Azure DevOps, etc.
- Optimize runner usage for cost savings
- Implement secure CI/CD practices
- Design reusable workflow architectures
