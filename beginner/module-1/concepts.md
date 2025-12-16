---
layout: training-module
title: "Core Concepts"
permalink: /beginner/module-1/concepts/
module_number: 1
module_title: "Understanding Software Development & SDLC"
section_number: 2
total_sections: 7
phase: beginner
estimated_time: "30 min"
module_index: /beginner/module-1/
sections:
  - title: "Context & Overview"
    url: "/beginner/module-1/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/beginner/module-1/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/beginner/module-1/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/beginner/module-1/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/beginner/module-1/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/beginner/module-1/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/beginner/module-1/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
prev_section:
  url: /beginner/module-1/overview/
  title: "Context & Overview"
next_section:
  url: /beginner/module-1/walkthrough/
  title: "Guided Walkthrough"
---

<div class="callout callout-info">
<div class="callout-title">📚 Getting Started</div>
New to GitHub? Start with the <a href="https://docs.github.com/en/get-started">Getting Started Guide</a> and explore the <a href="https://github.com/skills">GitHub Skills</a> interactive tutorials.
</div>

## Software Development Lifecycle (SDLC)

The SDLC is the process of planning, creating, testing, and deploying software. While models vary, most include these phases:

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
    subgraph SDLC["Software Development Lifecycle"]
        P["📋 PLANNING<br/>What & Why"]
        D["🎨 DESIGN<br/>How"]
        DEV["💻 DEVELOP<br/>Build It"]
        T["🧪 TEST<br/>Verify It"]
        DEP["🚀 DEPLOY<br/>Release It"]
    end
    
    P --> D --> DEV --> T --> DEP
    DEP -.->|"Feedback Loop"| P
</div>
</div>

---

## Phase 1: Planning

**What:** Define what to build and why  
**Artifacts:** Requirements, user stories, specifications  
**GitHub Tools:** Issues, Projects, Discussions

### User Story Example

```
User Story: "As a customer, I want to reset my password via email 
so that I can regain access if I forget it."

Acceptance Criteria:

- User can click "Forgot Password" link
- System sends email with reset link within 2 minutes
- Link expires after 24 hours
- User can set new password meeting security requirements

```

### Key Planning Artifacts

| Artifact | Description | Example |
|----------|-------------|---------|
| **Epic** | Large feature/initiative | "User Authentication System" |
| **User Story** | Specific user need | "Password reset via email" |
| **Task** | Technical work item | "Implement email sending service" |
| **Acceptance Criteria** | Definition of done | "Email sent within 2 minutes" |

---

## Phase 2: Design/Architecture

**What:** Plan how to build it  
**Artifacts:** Architecture diagrams, API specs, data models  
**GitHub Tools:** Wikis, Markdown files, Architecture Decision Records (ADRs)

### Architecture Decision Record (ADR) Template

```markdown
# ADR 001: Use PostgreSQL for User Data

## Status
Accepted

## Context
We need a database for user authentication data.

## Decision
Use PostgreSQL for its reliability and JSON support.

## Consequences
- Need PostgreSQL expertise on team
- Migration from existing MySQL required

```

---

## Phase 3: Development

**What:** Write the code  
**Artifacts:** Source code, commits, branches  
**GitHub Tools:** Repositories, branches, commits, pull requests

### The Git Workflow

<div class="mermaid-container">
<div class="mermaid">
gitGraph
    commit id: "Initial"
    commit id: "v1.0"
    branch feature-1
    commit id: "Add login"
    commit id: "Fix bug"
    checkout main
    branch feature-2
    commit id: "Add cart"
    commit id: "Style cart"
    checkout feature-1
    commit id: "Tests"
    checkout main
    merge feature-1
    checkout feature-2
    commit id: "Checkout"
    commit id: "Payment"
    checkout main
    merge feature-2
    commit id: "v1.1"
</div>
</div>

Developers create **branches** for new features, make **commits** (snapshots of changes), then **merge** back to the main codebase via **pull requests**.

---

## Phase 4: Testing

**What:** Verify it works correctly  
**Artifacts:** Test code, test reports, coverage metrics  
**GitHub Tools:** GitHub Actions for automated testing

### Types of Testing

| Type | What It Tests | When It Runs |
|------|---------------|--------------|
| **Unit Tests** | Individual functions | Every commit |
| **Integration Tests** | Components working together | Every PR |
| **End-to-End Tests** | Full user workflows | Before release |
| **Security Tests** | Vulnerabilities | Continuous |

---

## Phase 5: Deployment

**What:** Release it to users  
**Artifacts:** Builds, containers, releases  
**GitHub Tools:** Actions, Packages, Releases, Environments

### Deployment Environments

```
Development → Staging → Production
     │            │           │
  Dev testing  QA testing  Real users

```

---

## SDLC Methodologies Compared

### Waterfall

Sequential phases, each completed before the next begins.

**Pros:** Clear milestones, good for regulated industries  
**Cons:** Inflexible, late feedback  
**Best for:** Projects with fixed requirements (construction, hardware)

### Agile

Iterative development in short cycles (sprints).

**Pros:** Adaptable, regular feedback  
**Cons:** Requires discipline, scope can creep  
**Best for:** Software products, evolving requirements

### DevOps

Combines development and operations with heavy automation.

**Pros:** Fast releases, high reliability  
**Cons:** Cultural shift required, tooling investment  
**Best for:** Teams needing frequent, reliable releases

---

## How GitHub Supports Each Methodology

| Methodology | GitHub Features |
|-------------|-----------------|
| **Waterfall** | Milestones, Projects (Board view), Releases |
| **Agile** | Projects (Sprint planning), Issues, Labels |
| **DevOps** | Actions (CI/CD), Packages, Environments, GHAS |

<div class="callout callout-info">
<div class="callout-title">💡 Key Insight</div>

GitHub is methodology-agnostic — it supports all approaches. Your job as a CSM is to understand which methodology your customer uses and highlight the relevant GitHub features.
</div>

---

## Quick Reference: SDLC to GitHub Mapping

| SDLC Phase | GitHub Feature | Customer Value |
|------------|----------------|----------------|
| Planning | Issues, Projects | Visibility, tracking |
| Design | Wikis, Discussions | Documentation, collaboration |
| Development | Repos, Branches, PRs | Version control, code review |
| Testing | Actions | Automated quality gates |
| Deployment | Actions, Environments | Reliable releases |
| Operations | Insights, GHAS | Monitoring, security |
