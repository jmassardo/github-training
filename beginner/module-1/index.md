---
layout: training
title: "Module 1: Understanding Software Development & SDLC"
description: "Build foundational knowledge of software development concepts, methodologies, and how GitHub enables modern development practices."
permalink: /beginner/module-1/
module_number: 1
phase: beginner
estimated_time: "2-3 hours total"
sections:
  - title: "Context & Overview"
    url: "/beginner/module-1/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "15 min"
  - title: "Core Concepts"
    url: "/beginner/module-1/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "30 min"
  - title: "Guided Walkthrough"
    url: "/beginner/module-1/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "25 min"
  - title: "Hands-On Labs"
    url: "/beginner/module-1/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "30 min"
  - title: "Knowledge Check"
    url: "/beginner/module-1/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "15 min"
  - title: "Resources"
    url: "/beginner/module-1/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/beginner/module-1/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "20 min"
objectives:
  - Explain the Software Development Lifecycle (SDLC) and its phases
  - Understand different SDLC methodologies (Waterfall, Agile, DevOps)
  - Recognize how GitHub fits into the development workflow
  - Identify planning artifacts (user stories, epics, acceptance criteria)
next_module:
  url: /beginner/module-2/
  title: "Module 2: Git Fundamentals"
---

## Welcome to Module 1

Before diving into GitHub features and Git commands, let's understand the *why* behind modern software development. This module establishes the foundation for everything that follows.

### Why This Matters

As a CSM/CSA, you'll work with engineering teams who speak in terms of "sprints," "user stories," "CI/CD," and "deployments." Understanding their world is crucial for:

- **Building credibility** — Speaking their language shows you understand their challenges
- **Providing better solutions** — Knowing their workflow helps you recommend the right GitHub features  
- **Identifying opportunities** — Understanding pain points reveals expansion opportunities

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

- No prior technical experience required
- Curiosity about software development
- Access to a GitHub account (free tier is fine)

---

## 🚀 Ready to Start?

Begin with the first section to understand the software development landscape.

<div class="module-cta">
  <a href="{{ site.baseurl }}/beginner/module-1/overview/" class="btn btn-primary btn-lg">
    Start Section 1: Context & Overview →
  </a>
</div>

