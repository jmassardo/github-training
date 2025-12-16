---
layout: training
title: "Module 12: Governance, Compliance & DORA Metrics"
module_number: 12
estimated_time: "3 hours total"
difficulty: "Advanced"
learning_track: "Advanced"
week_number: 12
description: "Master organizational governance, compliance frameworks, and engineering effectiveness metrics using DORA standards and GitHub's value stream insights."
sections:
  - title: "Context & Overview"
    url: "/advanced/module-12/overview/"
    short_title: "Overview"
    icon: "📋"
    time: "20 min"
  - title: "Core Concepts"
    url: "/advanced/module-12/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "30 min"
  - title: "Guided Walkthrough"
    url: "/advanced/module-12/walkthrough/"
    short_title: "Walkthrough"
    icon: "🔄"
    time: "30 min"
  - title: "Hands-On Labs"
    url: "/advanced/module-12/labs/"
    short_title: "Labs"
    icon: "🧪"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/advanced/module-12/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "15 min"
  - title: "Resources"
    url: "/advanced/module-12/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/advanced/module-12/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "20 min"
objectives:
  - Design and implement governance frameworks for GitHub at enterprise scale
  - Build compliance reporting systems for regulatory requirements
  - Implement DORA metrics to measure engineering performance
  - Create value stream maps to identify bottlenecks and improvements
  - Establish innersource programs for cross-team collaboration
  - Measure and communicate ROI of GitHub adoption
prerequisites:
  - "Module 9: Enterprise Administration"
  - "Module 10: GitHub APIs, Webhooks & Apps"
  - "Module 11: Infrastructure as Code & Operations"
  - Understanding of software development lifecycle metrics
prev_module:
  url: /advanced/module-11/
  title: "Module 11: Infrastructure as Code & Operations"
next_module:
  url: /advanced/module-13/
  title: "Module 13: GitHub Copilot Fundamentals"
---

## Module Sections

Work through each section in order for the best learning experience:

<div class="section-cards">
{% for section in page.sections %}
<a href="{{ site.baseurl }}{{ section.url }}" class="section-card">
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
- [Module 11: Infrastructure as Code & Operations]({{ site.baseurl }}/advanced/module-11/)
- Understanding of compliance frameworks (SOC 2, GDPR, etc.)
- Familiarity with audit requirements

---

## 🚀 Ready to Start?

Master organizational governance, compliance frameworks, and DORA metrics.

<div class="module-cta">
  <a href="{{ site.baseurl }}/advanced/module-12/overview/" class="btn btn-primary btn-lg">
    Start Section 1: Context & Overview →
  </a>
</div>

