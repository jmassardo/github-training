---
layout: training
title: "Module 3: GitHub Platform Essentials"
description: "Navigate and master the GitHub platform - Issues, Projects, Pull Requests, Codespaces, Discussions, and collaboration features."
permalink: /beginner/module-3/
module_number: 3
phase: beginner
estimated_time: "2-2.5 hours total"
sections:
  - title: "Context & Overview"
    url: "/beginner/module-3/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "10 min"
  - title: "Core Concepts"
    url: "/beginner/module-3/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "20 min"
  - title: "Guided Walkthrough"
    url: "/beginner/module-3/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "20 min"
  - title: "Hands-On Labs"
    url: "/beginner/module-3/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "25 min"
  - title: "Knowledge Check"
    url: "/beginner/module-3/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "10 min"
  - title: "Resources"
    url: "/beginner/module-3/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "5 min"
  - title: "CSM Scenarios"
    url: "/beginner/module-3/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "15 min"
  - title: "GitHub Codespaces"
    url: "/beginner/module-3/codespaces/"
    short_title: "Codespaces"
    icon: "☁️"
    time: "20 min"
  - title: "GitHub Discussions"
    url: "/beginner/module-3/discussions/"
    short_title: "Discussions"
    icon: "💬"
    time: "15 min"
objectives:
  - Navigate the GitHub web interface confidently
  - Use Issues for tracking work and bugs
  - Manage projects with GitHub Projects
  - Create and review Pull Requests
  - Configure repository settings and permissions
  - Set up GitHub Pages, Wikis, and Discussions
  - Use GitHub Codespaces for cloud development
  - Build community with GitHub Discussions
prev_module:
  url: /beginner/module-2/
  title: "Module 2: Git Fundamentals"
next_module:
  url: /beginner/module-4/
  title: "Module 4: SCM Process"
---

# 📘 Module 3: GitHub Platform Essentials
**Beginner Phase | Estimated Time: 90 minutes**

---

## Module Overview

You've mastered Git on your local machine. Now it's time to explore **GitHub.com** — the collaboration platform that transforms Git from a solo tool into a team superpower.

---

## What You'll Learn

- Navigate the GitHub web interface like a pro
- Use Issues for tracking work and bugs
- Manage projects with GitHub Projects
- Create and review Pull Requests
- Set up GitHub Pages, Wikis, and Discussions
- Configure repository settings and permissions

---

## Why This Matters to CSMs

GitHub.com is where your customers spend most of their time. Understanding every corner of the platform helps you:

- Answer customer questions without escalating
- Identify adoption gaps and opportunities
- Recommend features that solve specific pain points
- Demo capabilities confidently in customer calls

---

## Module Sections

Work through each section in order for the best learning experience:

<div class="section-cards">
{% for section in page.sections %}
<a href="{{ section.url }}" class="section-card">
  <span class="section-icon">{{ section.icon }}</span>
  <div class="section-info">
    <h3>{{ section.title }}</h3>
    <span class="section-time">{{ section.time }}</span>
  </div>

  <span class="section-arrow">→</span>
</a>
{% endfor %}
</div>

---

## Learning Objectives

By the end of this module, you will be able to:

{% for objective in page.objectives %}
- ✅ {{ objective }}
{% endfor %}

---

## The GitHub Ecosystem

Think of GitHub as a city built around Git:

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph PLATFORM["🏛️ GITHUB PLATFORM"]
        subgraph CODE["💻 CODE"]
            C1["Repositories"]
            C2["Pull Requests"]
            C3["Code Review"]
            C4["Branches"]
        end
        
        subgraph PLANNING["📋 PLANNING"]
            P1["Issues"]
            P2["Projects"]
            P3["Milestones"]
            P4["Discussions"]
        end
        
        subgraph AUTO["⚡ AUTOMATION"]
            A1["Actions"]
            A2["Packages"]
            A3["Pages"]
            A4["Codespaces"]
        end
        
        subgraph SECURITY["🔒 SECURITY"]
            S1["Dependabot"]
            S2["Code Scan"]
            S3["Secrets"]
            S4["GHAS"]
        end
    end
</div>
</div>

---

## What GitHub Adds to Git

| **Git (Local)** | **GitHub (Cloud Platform)** |
|-----------------|----------------------------|
| Track changes locally | Store code in the cloud |
| Commit history | Visual diff and blame views |
| Branches | Pull requests with reviews |
| N/A | Issues and project management |
| N/A | Actions (CI/CD automation) |
| N/A | Security scanning |
| N/A | Team permissions and access |

---

## Prerequisites

Before starting this module:

- ✅ Completed Module 2 (Git Fundamentals)
- ✅ GitHub account created
- ✅ Comfortable with basic Git commands

---

## 🚀 Ready to Start?

Let's explore the platform that over 100 million developers call home.

<div class="module-cta">
  <a href="/beginner/module-3/overview/" class="btn btn-primary btn-lg">
    Start Section 1: Context & Overview →
  </a>
</div>

