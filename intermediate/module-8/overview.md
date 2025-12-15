---
layout: training-module
title: "Module 8, Section 1: Context & Overview"
description: "Understand deployment challenges and GitHub's deployment stack"
permalink: /intermediate/module-8/overview/
module_number: 8
section_number: 1
phase: intermediate
prev_section:
  url: /intermediate/module-8/
  title: "Module 8 Home"
next_section:
  url: /intermediate/module-8/concepts/
  title: "Core Concepts"
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

## The Deployment Challenge
Shipping software reliably is hard:

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph TRADITIONAL["❌ Traditional Deployment Pain Points"]
        T1["'Works on my machine'"] --> T2["Manual deployment"] --> T3["Inconsistency"]
        T1 --> ED["Environment drift"]
        T2 --> HE["Human error"]
        T3 --> DT["Downtime"]
        PAIN["Friday 5pm deployment → Weekend ruined 😩"]
    end
</div>
</div>

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    subgraph MODERN["✅ Modern CI/CD"]
        C["Commit<br/>Tracked changes"] --> T["Test<br/>Automated quality"]
        T --> B["Build<br/>Immutable artifacts"]
        B --> D["Deploy<br/>Protected envs"]
        D --> M["Monitor<br/>Auto alerts"]
        M -.->|"Iterate"| C
    end
</div>
</div>

---

## Deployment Velocity Spectrum

| Level | Practice | Deploy Frequency | Lead Time |
|-------|----------|-----------------|-----------|
| 1 | Manual | Monthly | Weeks |
| 2 | Scripted | Weekly | Days |
| 3 | Automated | Daily | Hours |
| 4 | Continuous | On demand | Minutes |
| 5 | Progressive | Per commit | Minutes |

## GitHub's Deployment Stack

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph GITHUB["🏛️ GitHub Platform"]
        subgraph REPOS["📁 Repositories"]
            subgraph ACTIONS["⚡ GitHub Actions"]
                CI["CI"] --> BUILD["Build"] --> TEST["Test"]
                TEST --> ENVS
                
                subgraph ENVS["🌍 Environments"]
                    DEV["Dev"]
                    STAGING["Staging"]
                    PROD["Production"]
                end
                
                RULES["Protection Rules, Secrets, URLs"]
            end
        end
        
        subgraph API["📊 Deployments API"]
            HIST["Deployment history"]
            STATUS["Status tracking"]
            ROLLBACK["Rollback triggers"]
        end
    end
    
    ENVS --> API
</div>
</div>

---

## What You'll Learn
This module covers:
- CI/CD pipeline design patterns
- GitHub Environments configuration
- Deployment strategies (blue-green, canary, rolling)
- Cloud platform deployments (Azure, AWS, GCP)
- Kubernetes deployments
- Rollback procedures and incident response

## Customer Success Context
As a CSM, you'll help customers:
- Design deployment pipelines that match their risk tolerance
- Reduce deployment lead time
- Implement progressive delivery
- Meet compliance requirements for deployments
{% endraw %}
