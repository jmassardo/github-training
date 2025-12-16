---
layout: training
title: "Module 7: Dependency & Package Management"
description: "Master GitHub Packages, container registries, and software supply chain security"
permalink: /intermediate/module-7/
module_number: 7
phase: intermediate
estimated_time: "3-4 hours total"
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
objectives:
  - Understand GitHub Packages architecture and supported registries
  - Publish and consume packages from GitHub Packages
  - Work with GitHub Container Registry (GHCR)
  - Implement package versioning strategies
  - Secure your software supply chain
  - Integrate packages with CI/CD workflows
prev_module:
  url: /intermediate/module-6/
  title: "Module 6: Security & GHAS"
next_module:
  url: /intermediate/module-8/
  title: "Module 8: CI/CD Pipelines & Deployment"
---

## Welcome to Module 7

This module covers dependency and package management with GitHub Packages, including npm, Maven, NuGet, RubyGems, and container registries. You'll learn how to publish, consume, and secure packages while implementing supply chain security best practices.

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

- **[Module 5: GitHub Actions]({{ site.baseurl }}/intermediate/module-5/)** (for workflow automation)
- **[Module 6: Security & GHAS]({{ site.baseurl }}/intermediate/module-6/)** (for security foundations)

---

## 🚀 Ready to Start?

Master dependency and package management with GitHub Packages.

<div class="module-cta">
  <a href="/intermediate/module-7/overview/" class="btn btn-primary btn-lg">
    Start Section 1: Context & Overview →
  </a>
</div>

