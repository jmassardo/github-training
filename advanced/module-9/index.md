---
layout: training
title: "Module 9: Enterprise Administration"
description: "Master GitHub Enterprise administration including identity management (EMU/SAML/SCIM), organization governance, audit logging, and enterprise migration strategies."
permalink: /advanced/module-9/
module_number: 9
phase: advanced
estimated_time: "3 hours total"
sections:
  - title: "Context & Overview"
    url: "/advanced/module-9/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "20 min"
  - title: "Core Concepts"
    url: "/advanced/module-9/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/advanced/module-9/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "35 min"
  - title: "Hands-On Labs"
    url: "/advanced/module-9/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/advanced/module-9/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "15 min"
  - title: "Resources"
    url: "/advanced/module-9/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/advanced/module-9/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "30 min"
objectives:
  - Configure enterprise identity providers with SAML SSO and SCIM provisioning
  - Implement Enterprise Managed Users (EMU) for centralized identity control
  - Design organizational hierarchies and governance structures
  - Leverage audit logs for compliance and security monitoring
  - Plan and execute GitHub migrations using GitHub Enterprise Importer
  - Configure GitHub Connect for hybrid enterprise deployments
prev_module:
  url: /intermediate/module-8/
  title: "Module 8: CI/CD Pipelines & Deployment"
next_module:
  url: /advanced/module-10/
  title: "Module 10: GitHub APIs & Apps"
---

## Welcome to Module 9

As a CSM supporting enterprise customers, you'll encounter organizations with complex requirements around identity management, compliance, and governance. Understanding GitHub Enterprise administration is essential for smooth onboarding, security compliance, migration success, and ongoing governance.

This module teaches you to configure enterprise identity providers, design organizational structures, leverage audit logs for compliance, and plan complex migrations using GitHub Enterprise Importer.

### Why This Matters

As a GitHub CSM, you'll help customers:
- Configure SSO, user provisioning, and organizational structures
- Ensure enterprise deployments meet regulatory and security requirements
- Guide customers through complex migrations from other platforms
- Support customers in maintaining healthy, well-governed GitHub estates

---

## Module Sections

Work through each section in order for the best learning experience:

<div class="section-cards">
{% raw %}{% for section in page.sections %}{% endraw %}
<a href="{% raw %}{{ section.url }}{% endraw %}" class="section-card">
  <span class="section-icon">{% raw %}{{ section.icon }}{% endraw %}</span>
  <div class="section-info">
    <h3>{% raw %}{{ section.title }}{% endraw %}</h3>
    <span class="section-time">{% raw %}{{ section.time }}{% endraw %}</span>
  </div>

  <span class="section-arrow">→</span>
</a>
{% raw %}{% endfor %}{% endraw %}
</div>

---

## Learning Objectives

By the end of this module, you will be able to:

{% raw %}{% for objective in page.objectives %}{% endraw %}
- ✅ {% raw %}{{ objective }}{% endraw %}
{% raw %}{% endfor %}{% endraw %}

---

## Prerequisites

Before starting this module, ensure you have completed:
- Module 3: GitHub Platform Essentials
- Module 4: SCM Process
- Module 6: Security with GHAS
- Understanding of enterprise identity management concepts

---

## The Enterprise Administration Landscape

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph GHEC["GITHUB ENTERPRISE CLOUD"]
    subgraph Enterprise["ENTERPRISE ACCOUNT"]
      EP["📋 Enterprise Policies"]
      AL["📊 Audit Logs"]
      IDP["🔐 Identity Provider"]
      GC["🔗 GitHub Connect"]
      EO["👥 Enterprise Owners"]
      BL["💳 Billing & Licensing"]
    end
    
    Enterprise --> OrgA & OrgB & OrgC
    
    subgraph OrgA["🏢 Org A"]
      A1["Teams"]
      A2["Repos"]
      A3["Runners"]
    end
    
    subgraph OrgB["🏢 Org B"]
      B1["Teams"]
      B2["Repos"]
      B3["Runners"]
    end
    
    subgraph OrgC["🏢 Org C"]
      C1["Teams"]
      C2["Repos"]
      C3["Runners"]
    end
    
    subgraph Identity["IDENTITY MANAGEMENT"]
      SAML["🔑 SAML SSO"]
      SCIM["👤 SCIM Prov."]
      EMU["🏢 Enterprise<br/>Managed Users"]
    end
  end
</div>
</div>

---

## 🚀 Ready to Start?

Master enterprise GitHub administration including identity management and governance.

<div class="module-cta">
  <a href="/advanced/module-9/overview/" class="btn btn-primary btn-lg">
    Start Section 1: Context & Overview →
  </a>
</div>

