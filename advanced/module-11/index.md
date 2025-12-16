---
layout: training
title: "Module 11: Infrastructure as Code & Operations"
module_number: 11
estimated_time: "3 hours total"
difficulty: "Advanced"
learning_track: "Advanced"
week_number: 11
description: "Master Infrastructure as Code (IaC) for GitHub management using Terraform, Pulumi, and GitOps practices to automate and standardize GitHub organization configuration."
sections:
  - title: "Context & Overview"
    url: "/advanced/module-11/overview/"
    short_title: "Overview"
    icon: "📋"
    time: "20 min"
  - title: "Core Concepts"
    url: "/advanced/module-11/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "30 min"
  - title: "Guided Walkthrough"
    url: "/advanced/module-11/walkthrough/"
    short_title: "Walkthrough"
    icon: "🔄"
    time: "30 min"
  - title: "Hands-On Labs"
    url: "/advanced/module-11/labs/"
    short_title: "Labs"
    icon: "🧪"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/advanced/module-11/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "15 min"
  - title: "Resources"
    url: "/advanced/module-11/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/advanced/module-11/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "20 min"
objectives:
  - Implement GitHub's Terraform provider for declarative repository and organization management
  - Apply GitOps principles to GitHub administration
  - Use Pulumi and other IaC tools for GitHub automation
  - Design operational workflows for GitHub administration at scale
  - Create self-service platforms for repository provisioning
  - Implement drift detection and configuration compliance
prerequisites:
  - "Module 9: Enterprise Administration"
  - "Module 10: GitHub APIs, Webhooks & Apps"
  - Basic understanding of Infrastructure as Code concepts
  - Familiarity with Terraform or similar IaC tools
prev_module:
  url: /advanced/module-10/
  title: "Module 10: GitHub APIs, Webhooks & Apps"
next_module:
  url: /advanced/module-12/
  title: "Module 12: Governance, Metrics & DORA"
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

Before starting this module, you should have completed:

- [Module 9: Enterprise Administration]({{ site.baseurl }}/advanced/module-9/)
- [Module 10: GitHub APIs, Webhooks & Apps]({{ site.baseurl }}/advanced/module-10/)
- Basic understanding of Infrastructure as Code concepts
- Familiarity with Terraform or similar IaC tools

---

## 🚀 Ready to Start?

Master Infrastructure as Code for GitHub management using Terraform and GitOps practices.

<div class="module-cta">
  <a href="/advanced/module-11/overview/" class="btn btn-primary btn-lg">
    Start Section 1: Context & Overview →
  </a>
</div>

