---
layout: training
title: "Module 8: CI/CD Pipelines & Deployment"
description: "Master deployment pipelines, strategies, and GitHub's deployment features for reliable software delivery"
permalink: /intermediate/module-8/
module_number: 8
phase: intermediate
estimated_time: "4-5 hours total"
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
objectives:
  - Design end-to-end CI/CD pipelines
  - Implement deployment strategies (blue-green, canary, rolling)
  - Configure GitHub Environments with protection rules
  - Set up deployment workflows for cloud platforms
  - Implement rollback procedures
  - Monitor deployments and handle failures
prev_module:
  url: /intermediate/module-7/
  title: "Module 7: Dependency & Package Management"
next_module:
  url: /advanced/module-9/
  title: "Module 9: Enterprise Administration"
---

## Welcome to Module 8

This module covers CI/CD pipelines and deployment with GitHub Actions, including deployment strategies, GitHub Environments, cloud platform integrations, and rollback procedures. You'll learn how to build reliable deployment pipelines that meet enterprise requirements.

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

- **[Module 5: GitHub Actions](/intermediate/module-5/)** (for workflow automation)
- **[Module 7: Packages](/intermediate/module-7/)** (for artifact management)

---

## 🚀 Ready to Start?

Build complete CI/CD pipelines and deployment workflows.

<div class="module-cta">
  <a href="/intermediate/module-8/overview/" class="btn btn-primary btn-lg">
    Start Section 1: Context & Overview →
  </a>
</div>

