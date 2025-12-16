---
layout: training
title: "Module 14: GitHub Copilot Advanced"
description: "Master Copilot Agents, Model Context Protocol (MCP), Extensions, and Enterprise administration"
module_number: 14
duration: "10-12 hours"
level: "Advanced"
week: 14
prerequisites: ["Module 13: Copilot Fundamentals"]
sections:
  - title: "Context & Overview"
    url: "/advanced/module-14/overview/"
    short_title: "Overview"
    icon: "📋"
    time: "20 min"
  - title: "Copilot Agents"
    url: "/advanced/module-14/agents/"
    short_title: "Agents"
    icon: "🤖"
    time: "35 min"
  - title: "Model Context Protocol"
    url: "/advanced/module-14/mcp/"
    short_title: "MCP"
    icon: "🔌"
    time: "30 min"
  - title: "Copilot Extensions"
    url: "/advanced/module-14/extensions/"
    short_title: "Extensions"
    icon: "🧩"
    time: "30 min"
  - title: "Custom Instructions"
    url: "/advanced/module-14/instructions/"
    short_title: "Instructions"
    icon: "📝"
    time: "25 min"
  - title: "Enterprise Administration"
    url: "/advanced/module-14/enterprise/"
    short_title: "Enterprise"
    icon: "🏢"
    time: "30 min"
  - title: "Metrics & Adoption"
    url: "/advanced/module-14/metrics/"
    short_title: "Metrics"
    icon: "📊"
    time: "25 min"
  - title: "Hands-On Labs"
    url: "/advanced/module-14/labs/"
    short_title: "Labs"
    icon: "🧪"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/advanced/module-14/quiz/"
    short_title: "Quiz"
    icon: "📝"
    time: "15 min"
  - title: "Resources"
    url: "/advanced/module-14/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
objectives:
  - Understand and use Copilot Agents for complex tasks
  - Configure Model Context Protocol (MCP) servers
  - Build and deploy Copilot Extensions
  - Manage Copilot at enterprise scale
  - Implement custom instructions and knowledge bases
  - Measure and optimize Copilot adoption
tags: [copilot, agents, mcp, extensions, enterprise, ai]
prev_module:
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

- [Module 13: GitHub Copilot Fundamentals]({{ site.baseurl }}/advanced/module-13/)
- Hands-on experience with Copilot Chat and code completions
- Access to Copilot Business or Enterprise features

---

## 🚀 Ready to Start?

Extend Copilot with agents, MCP, extensions, and enterprise administration.

<div class="module-cta">
  <a href="{{ site.baseurl }}/advanced/module-14/overview/" class="btn btn-primary btn-lg">
    Start Section 1: Context & Overview →
  </a>
</div>

