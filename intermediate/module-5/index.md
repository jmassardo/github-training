---
layout: training
title: "Module 5: GitHub Actions & Automation"
description: "Master GitHub Actions for CI/CD automation, custom workflows, and integrating automation into your development process"
permalink: /intermediate/module-5/
module_number: 5
phase: intermediate
estimated_time: "4-5 hours total"
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
objectives:
  - Understand GitHub Actions architecture and components
  - Create and customize workflow files using YAML syntax
  - Implement CI pipelines for testing, building, and linting
  - Use marketplace actions and create reusable workflows
  - Manage secrets and environment variables securely
  - Debug and optimize workflow performance
prev_module:
  url: /beginner/module-4/
  title: "Module 4: SCM Process"
next_module:
  url: /intermediate/module-6/
  title: "Module 6: Security & GHAS"
---

## Welcome to Module 5

Before GitHub Actions, teams faced a fragmented automation landscape with multiple vendors, contexts, and credentials spread across different CI/CD tools. GitHub Actions unified this into a single platform where code, automation, and deployment live together natively.

### Why This Matters

As a CSM, you'll help customers:
- Migrate from legacy CI/CD tools (Jenkins, Travis, CircleCI)
- Design efficient automation pipelines
- Reduce build times and compute costs
- Implement security best practices for workflows
- Scale automation across their organization

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

## Prerequisites

- Completed Beginner Phase (Modules 1-4)
- GitHub account with Actions enabled
- Basic understanding of YAML syntax
- Familiarity with command-line tools

---

## Quick Reference

**Key Concepts You'll Learn:**

| Concept | Description |
|---------|-------------|
| **Workflow** | YAML file defining automation |
| **Job** | Set of steps running on same runner |
| **Step** | Individual task within a job |
| **Action** | Reusable unit of code |
| **Runner** | Machine executing workflows |
| **Trigger** | Event that starts a workflow |

---

## 🚀 Ready to Start?

Dive into GitHub Actions and learn how to automate your development workflows.

<div class="module-cta">
  <a href="/intermediate/module-5/overview/" class="btn btn-primary btn-lg">
    Start Section 1: Context & Overview →
  </a>
</div>

