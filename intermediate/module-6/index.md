---
layout: training
title: "Module 6: Code Security with GHAS"
description: "Master GitHub Advanced Security including code scanning, secret scanning, Dependabot, and security policies"
permalink: /intermediate/module-6/
module_number: 6
phase: intermediate
estimated_time: "4-5 hours total"
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
objectives:
  - Understand GitHub Advanced Security architecture
  - Configure CodeQL for code scanning
  - Implement secret scanning and push protection
  - Set up Dependabot for dependency management
  - Create and manage security policies
  - Analyze and triage security alerts
prev_module:
  url: /intermediate/module-5/
  title: "Module 5: GitHub Actions"
next_module:
  url: /intermediate/module-7/
  title: "Module 7: GitHub Packages"
---

## Welcome to Module 6

Traditional security approaches are broken—finding vulnerabilities after deployment is expensive and risky. GitHub Advanced Security (GHAS) shifts security left, catching issues in the development workflow before they reach production.

### Why This Matters

As a CSM, you'll help customers:

- Roll out GHAS across their organization
- Configure security policies that balance security and velocity
- Reduce false positive rates
- Build developer security awareness
- Meet compliance requirements (SOC 2, PCI-DSS, etc.)

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

- Completed [Module 5: GitHub Actions](/intermediate/module-5/)
- GitHub account with access to GHAS features
- Understanding of basic security concepts
- Familiarity with YAML configuration

---

## Quick Reference

**GHAS Components:**

| Component | What It Does | When It Runs |
|-----------|--------------|--------------|
| **Code Scanning (CodeQL)** | Finds vulnerabilities in your code | PR, push, schedule |
| **Secret Scanning** | Detects exposed credentials | Push, historical scan |
| **Push Protection** | Blocks secrets before they're committed | Pre-receive hook |
| **Dependabot Alerts** | Identifies vulnerable dependencies | Continuous |
| **Dependabot Updates** | Auto-creates PRs for updates | Schedule |
| **Dependency Review** | Analyzes dependency changes in PRs | PR |

---

## 🚀 Ready to Start?

Learn how to secure your code and dependencies with GitHub Advanced Security.

<div class="module-cta">
  <a href="/intermediate/module-6/overview/" class="btn btn-primary btn-lg">
    Start Section 1: Context & Overview →
  </a>
</div>

