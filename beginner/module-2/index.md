---
layout: training
title: "Module 2: Git Fundamentals & Version Control"
description: "Master Git version control fundamentals - repositories, commits, branches, and merging."
permalink: /beginner/module-2/
module_number: 2
phase: beginner
estimated_time: "1.5-2 hours total"
sections:
  - title: "Context & Overview"
    url: "/beginner/module-2/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "15 min"
  - title: "Core Concepts"
    url: "/beginner/module-2/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "20 min"
  - title: "Guided Walkthrough"
    url: "/beginner/module-2/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "20 min"
  - title: "Hands-On Labs"
    url: "/beginner/module-2/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "25 min"
  - title: "Knowledge Check"
    url: "/beginner/module-2/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "10 min"
  - title: "Resources"
    url: "/beginner/module-2/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "5 min"
  - title: "CSM Scenarios"
    url: "/beginner/module-2/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "15 min"
objectives:
  - Explain what version control is and why it matters
  - Understand Git's core concepts (repositories, commits, branches, merges)
  - Execute essential Git commands confidently
  - Handle merge conflicts effectively
  - Differentiate between Git (tool) and GitHub (platform)
prev_module:
  url: /beginner/module-1/
  title: "Module 1: Understanding SDLC"
next_module:
  url: /beginner/module-3/
  title: "Module 3: GitHub Platform"
---

# 📘 Module 2: Git Fundamentals & Version Control
**Beginner Phase | Estimated Time: 90 minutes**

---

## Module Overview

Now that you understand the software development lifecycle, let's dive into the foundational tool that makes modern collaboration possible: **Git**. Think of Git as a time machine for code — it tracks every change, lets you experiment safely, and enables teams to work together without stepping on each other's toes.

---

## What You'll Learn

- What version control is and why every developer relies on it
- Git's core concepts: repositories, commits, branches, and merges
- Essential Git commands you'll use daily
- How to handle merge conflicts like a pro
- The difference between Git (the tool) and GitHub (the platform)

---

## Why This Matters to CSMs

Your customers are using Git whether they realize it or not. Understanding Git helps you:

- Speak the same language as engineering teams
- Troubleshoot customer issues confidently
- Design better adoption strategies
- Explain the value of GitHub's Git enhancements

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

## Quick Reference: Git vs GitHub

| **Git** | **GitHub** |
|---------|-----------|
| Version control tool (software) | Cloud platform/service |
| Runs locally on your computer | Hosted in the cloud |
| Command-line or GUI tools | Web interface + APIs |
| Created by Linus Torvalds (2005) | Founded in 2008 (acquired by Microsoft 2018) |
| Free and open source | Free and paid plans |
| Manages your code history | Adds collaboration, CI/CD, security, project management |

**Analogy**: Git is like Microsoft Word (the software that tracks changes). GitHub is like Microsoft 365 (the cloud service that adds sharing, comments, and collaboration).

---

## Prerequisites

Before starting this module, you should have:

- ✅ Completed Module 1 (SDLC fundamentals)
- ✅ A GitHub account (free tier is fine)
- ✅ Git installed on your computer ([Download Git](https://git-scm.com/downloads))
- ✅ A text editor (VS Code recommended)

---

## 🚀 Ready to Start?

Begin with the first section to understand why version control exists and how Git became the industry standard.

<div class="module-cta">
  <a href="/beginner/module-2/overview/" class="btn btn-primary btn-lg">
    Start Section 1: Context & Overview →
  </a>
</div>

