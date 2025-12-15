---
layout: training-module
title: "Module 6, Section 1: Context & Overview"
description: "Understand the shift-left security imperative and GHAS architecture"
permalink: /intermediate/module-6/overview/
module_number: 6
section_number: 1
phase: intermediate
prev_section:
  url: /intermediate/module-6/
  title: "Module 6 Home"
next_section:
  url: /intermediate/module-6/concepts/
  title: "Core Concepts"
toc: true
module_title: "Code Security with GHAS"
total_sections: 7
module_index: /intermediate/module-6/
sections:
  - title: "Context & Overview"
    url: "/intermediate/module-6/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "20 min"
  - title: "Core Concepts"
    url: "/intermediate/module-6/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/intermediate/module-6/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "50 min"
  - title: "Hands-On Labs"
    url: "/intermediate/module-6/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/intermediate/module-6/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "25 min"
  - title: "Resources"
    url: "/intermediate/module-6/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/intermediate/module-6/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "30 min"
---

## The Shift-Left Security Imperative
Traditional security approaches are broken:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    subgraph TRADITIONAL["❌ Traditional Security (Shift-Right)"]
        direction LR
        TC["Code"] --> TB["Build"] --> TT["Test"] --> TSR["Security Review"] --> TD["Deploy"] --> TM["Monitor"]
    end
    TSR -.->|"⚠️ Found vulnerabilities<br/>Fix it later backlog<br/>Expensive remediation"| TC
</div>
</div>

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    subgraph MODERN["✅ Modern Security (Shift-Left with GHAS)"]
        direction LR
        MC["Code<br/>🔑 Secret Scanning"] --> MS["Security<br/>🔍 CodeQL Analysis"] --> MB["Build<br/>📦 Dependabot"] --> MD["Deploy<br/>🛡️ Protection"]
    end
    BENEFIT["✨ Fix immediately, before production"]
    MODERN --> BENEFIT
</div>
</div>

---

## GitHub Advanced Security (GHAS) Components

| Component | What It Does | When It Runs |
|-----------|--------------|--------------|
| **Code Scanning (CodeQL)** | Finds vulnerabilities in your code | PR, push, schedule |
| **Secret Scanning** | Detects exposed credentials | Push, historical scan |
| **Push Protection** | Blocks secrets before they're committed | Pre-receive hook |
| **Dependabot Alerts** | Identifies vulnerable dependencies | Continuous |
| **Dependabot Updates** | Auto-creates PRs for updates | Schedule |
| **Dependency Review** | Analyzes dependency changes in PRs | PR |
| **Security Overview** | Enterprise-wide security dashboard | On-demand |

## GHAS Architecture

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph REPO["📦 GitHub Repository"]
        subgraph SECTAB["🔒 Security Tab"]
            CS["Code Scanning<br/>Alerts"]
            SS["Secret Scanning<br/>Alerts"]
            DA["Dependabot<br/>Alerts"]
        end
        
        subgraph ACTIONS["⚡ GitHub Actions Workflows"]
            CQ["CodeQL Analysis"]
            DR["Dependency Review Action"]
        end
        
        subgraph PUSH["🛡️ Push Protection"]
            PP["Pre-receive hook blocks<br/>secrets before commit"]
        end
    end
    
    PP --> ACTIONS --> SECTAB
</div>
</div>

---

## What You'll Learn
This module covers:

- CodeQL analysis configuration and custom queries
- Secret scanning patterns and push protection
- Dependabot configuration and automation
- Security policy creation and management
- Alert triage and remediation workflows
- Enterprise security rollout strategies

## Customer Success Context
As a CSM, you'll help customers:

- Roll out GHAS across their organization
- Configure security policies that balance security and velocity
- Reduce false positive rates
- Build developer security awareness
- Meet compliance requirements (SOC 2, PCI-DSS, etc.)

