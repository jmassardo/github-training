---
layout: training-module
title: "Module 7, Section 1: Context & Overview"
description: "Understand the software supply chain and why GitHub Packages matters"
permalink: /intermediate/module-7/overview/
module_number: 7
section_number: 1
phase: intermediate
prev_section:
  url: /intermediate/module-7/
  title: "Module 7 Home"
next_section:
  url: /intermediate/module-7/concepts/
  title: "Core Concepts"
toc: true
module_title: "Dependency & Package Management"
total_sections: 7
module_index: /intermediate/module-7/
sections:
  - title: "Context & Overview"
    url: "/intermediate/module-7/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "20 min"
  - title: "Core Concepts"
    url: "/intermediate/module-7/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/intermediate/module-7/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "50 min"
  - title: "Hands-On Labs"
    url: "/intermediate/module-7/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/intermediate/module-7/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "25 min"
  - title: "Resources"
    url: "/intermediate/module-7/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/intermediate/module-7/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "30 min"
---
{% raw %}

## The Software Supply Chain
Modern applications don't build everything from scratch:

```
Your Application
     │
     ├── Your Code (10%)
     │
     └── Dependencies (90%)
           │
           ├── Direct Dependencies
           │     ├── react
           │     ├── express
           │     └── lodash
           │
           └── Transitive Dependencies
                 ├── react-dom → scheduler → loose-envify
                 ├── express → body-parser → bytes
                 └── lodash → (none)
                 
Average npm project: 683 transitive dependencies

```

---

## Why Package Management Matters

| Challenge | Impact | Solution |
|-----------|--------|----------|
| Dependency sprawl | Security vulnerabilities | Automated scanning |
| Version conflicts | Build failures | Lock files, version pinning |
| Untrusted packages | Supply chain attacks | Package provenance |
| Slow builds | Developer friction | Package caching |
| Private packages | Credential management | Registry authentication |

## GitHub Packages Overview
GitHub Packages is a package hosting service integrated with GitHub:

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph PKG["📦 GitHub Packages"]
        direction TB
        subgraph ROW1[" "]
            direction LR
            NPM["npm<br/>Registry"]
            MVN["Maven<br/>Registry"]
            NUGET["NuGet<br/>Registry"]
        end
        subgraph ROW2[" "]
            direction LR
            GRADLE["Gradle<br/>Registry"]
            RUBY["RubyGems<br/>Registry"]
            GHCR["Containers<br/>(GHCR)"]
        end
        FEATURES["✨ Features:<br/>• Integrated permissions<br/>• Native Actions integration<br/>• Dependency graph integration<br/>• Package insights & stats"]
    end
</div>
</div>

---

## What You'll Learn
This module covers:

- Publishing packages to GitHub Packages
- GitHub Container Registry (GHCR)
- Package versioning and release strategies
- Supply chain security and provenance
- Integration with GitHub Actions
- Migration from other registries

## Customer Success Context
As a CSM, you'll help customers:

- Consolidate from multiple package registries
- Implement secure package workflows
- Set up private package sharing across teams
- Establish versioning and release standards
{% endraw %}
