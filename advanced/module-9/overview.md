---
layout: training-module
title: "Context & Overview"
permalink: /advanced/module-9/overview/
module_number: 9
module_title: "Enterprise Administration"
section_number: 1
total_sections: 7
phase: advanced
estimated_time: "20 min"
module_index: /advanced/module-9/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-9/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/advanced/module-9/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/advanced/module-9/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/advanced/module-9/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/advanced/module-9/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/advanced/module-9/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/advanced/module-9/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
next_section:
  url: /advanced/module-9/concepts/
  title: "Core Concepts"
---

# Context & Overview

## Why Enterprise Administration Matters for Customer Success

As a CSM supporting enterprise customers, you'll encounter organizations with complex requirements around identity management, compliance, and governance. Understanding GitHub Enterprise administration is essential for:

- **Smooth onboarding**: Helping customers configure SSO, user provisioning, and organizational structures
- **Security compliance**: Ensuring enterprise deployments meet regulatory and security requirements
- **Migration success**: Guiding customers through complex migrations from other platforms
- **Ongoing governance**: Supporting customers in maintaining healthy, well-governed GitHub estates

## CSM Context: Common Enterprise Scenarios

| Scenario | Customer Need | Your Role |
|----------|---------------|-----------|
| New Enterprise Deployment | Configure SSO, provision users, set up orgs | Guide architecture decisions, validate configuration |
| Compliance Requirements | Audit logging, access controls, policy enforcement | Demonstrate capabilities, configure monitoring |
| Platform Migration | Move from GitLab, Bitbucket, Azure DevOps | Plan migration, execute import, validate data |
| Hybrid Deployment | GitHub.com with GitHub Enterprise Server | Configure GitHub Connect, design hybrid strategy |
| User Lifecycle Management | Automate provisioning/deprovisioning | Implement SCIM, design JML (Joiner/Mover/Leaver) processes |

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

## Key Topics in This Module

This module covers the essential components of GitHub Enterprise administration:

### Identity Management
- SAML Single Sign-On (SSO) configuration
- SCIM user provisioning
- Enterprise Managed Users (EMU)

### Organization Governance
- Enterprise account structure
- Organization management
- Team hierarchies

### Compliance & Security
- Audit logging and streaming
- Access controls and policies
- Security monitoring

### Migration & Integration
- GitHub Enterprise Importer
- GitHub Connect for hybrid deployments
- Platform migration strategies
