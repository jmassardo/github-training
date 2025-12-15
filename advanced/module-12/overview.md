---
layout: training-module
title: "Context & Overview"
permalink: /advanced/module-12/overview/
module_number: 12
module_title: "Governance, Compliance & DORA Metrics"
section_number: 1
total_sections: 7
phase: advanced
estimated_time: "20 min"
module_index: /advanced/module-12/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-12/overview/"
    short_title: "Overview"
    icon: "📋"
  - title: "Core Concepts"
    url: "/advanced/module-12/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/advanced/module-12/walkthrough/"
    short_title: "Walkthrough"
    icon: "🔄"
  - title: "Hands-On Labs"
    url: "/advanced/module-12/labs/"
    short_title: "Labs"
    icon: "🧪"
  - title: "Knowledge Check"
    url: "/advanced/module-12/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/advanced/module-12/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/advanced/module-12/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
next_section:
  url: /advanced/module-12/concepts/
  title: "Core Concepts"
---

## Why Governance & Metrics Matter

As organizations scale their GitHub adoption, two challenges consistently emerge:

1. **Governance**: How do we maintain security, compliance, and standards across thousands of repositories?
2. **Measurement**: How do we know if our engineering investment is paying off?

This module addresses both challenges, connecting GitHub's enterprise features to business outcomes.

## The Governance Challenge

### Enterprise Reality Check

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Enterprise["Typical Enterprise Environment"]
    Stats["📊 3,000+ repositories | 500+ developers | 50+ teams"]
    
    subgraph Challenges["Challenges"]
      C1["⚠️ Inconsistent security settings"]
      C2["❓ Unknown code ownership"]
      C3["⏰ Audit requests take weeks"]
      C4["👻 Shadow IT repositories"]
      C5["📉 No visibility into developer productivity"]
    end
  end
</div>
</div>

### Governance Without Bureaucracy

The goal isn't to slow teams down—it's to enable them safely:

| Without Governance | With Effective Governance |
|-------------------|---------------------------|
| Teams wait for approval | Self-service with guardrails |
| Manual security reviews | Automated policy enforcement |
| Point-in-time audits | Continuous compliance |
| Tribal knowledge | Documented standards |
| Reactive incident response | Proactive risk detection |

## The Measurement Challenge

### Why Measure Engineering?

Engineering leaders face constant pressure to demonstrate value:

- "How productive is our engineering team?"
- "Why does everything take so long?"
- "Are we getting ROI on our tooling investment?"
- "How do we compare to industry benchmarks?"

**The problem**: Traditional metrics often measure the wrong things.

### From Vanity Metrics to DORA

| Vanity Metrics ❌ | DORA Metrics ✅ |
|------------------|-----------------|
| Lines of code | Deployment Frequency |
| Commits per developer | Lead Time for Changes |
| Story points | Mean Time to Recovery |
| Hours worked | Change Failure Rate |

DORA metrics correlate with both organizational performance AND developer well-being.

## DORA: The Science of DevOps

### What is DORA?

The **DevOps Research and Assessment** (DORA) team conducted multi-year research to identify metrics that predict:

- Organizational performance (profitability, market share)
- Non-commercial performance (customer satisfaction, quality)
- Team culture and developer well-being

### The Four Key Metrics

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
  subgraph DORA["DORA Four Key Metrics"]
    subgraph Speed["📈 Speed Metrics"]
      DF["🚀 Deployment Frequency<br/>How often we deploy<br/>────────<br/>Elite: Multiple/day<br/>High: Weekly-Monthly<br/>Medium: Monthly-6mo<br/>Low: > 6 months"]
      LT["⏱️ Lead Time for Changes<br/>Code to production time<br/>────────<br/>Elite: < 1 hour<br/>High: < 1 week<br/>Medium: 1-6 months<br/>Low: > 6 months"]
    end
    
    subgraph Stability["📊 Stability Metrics"]
      MTTR["🔧 Mean Time to Recovery<br/>How fast we recover<br/>────────<br/>Elite: < 1 hour<br/>High: < 1 day<br/>Medium: < 1 week<br/>Low: > 6 months"]
      CFR["⚠️ Change Failure Rate<br/>% of deploys causing failures<br/>────────<br/>Elite: 0-15%<br/>High: 16-30%<br/>Medium: 16-30%<br/>Low: > 30%"]
    end
  end
</div>
</div>

### Why These Metrics Work

Traditional thinking: "Fast = Risky" (more deployments = more failures)

DORA research proved: **Elite performers are both faster AND more stable**

This is because:

- Small, frequent changes are easier to test and debug
- Fast feedback loops catch issues quickly
- Automated processes are more reliable than manual ones
- Recovery capability reduces fear of change

## GitHub's Role in Governance & Metrics

### GitHub Enterprise Cloud Features

| Feature | Governance Use | Metrics Use |
|---------|---------------|-------------|
| **Audit Log** | Compliance reporting, incident investigation | Activity tracking |
| **Enterprise Policies** | Enforce security standards | Measure policy adoption |
| **Code Scanning** | Prevent vulnerabilities | Track security posture |
| **Secret Scanning** | Detect credential exposure | Measure exposure trends |
| **Dependabot** | Enforce dependency updates | Track vulnerability remediation |
| **Actions Usage** | Standardize workflows | Measure automation adoption |
| **Insights API** | - | Pull request metrics, contributor stats |

### GitHub Copilot Metrics

With GitHub Copilot, organizations can measure AI-assisted development:

- **Acceptance Rate**: How often suggestions are accepted
- **Lines of Code Saved**: Estimated productivity gain
- **Active Users**: Adoption across the organization
- **Language Usage**: Where Copilot provides most value

## Customer Success Perspective

### Common Customer Goals

| Customer Says | What They Need |
|--------------|----------------|
| "We need to pass our SOC 2 audit" | Audit logging, access controls, change management |
| "Engineering is a black box to leadership" | DORA metrics, value stream mapping |
| "Teams are siloed, duplicating effort" | Innersource program, visibility tools |
| "We can't measure Copilot ROI" | Copilot usage analytics, productivity metrics |
| "Security can't keep up with dev" | Shift-left security, automated scanning |

### Conversation Starters

When engaging customers on governance and metrics:

1. **Start with their pain**: "What keeps you up at night about compliance?"
2. **Understand their audience**: "Who needs to see these metrics?"
3. **Identify existing measures**: "How do you measure engineering success today?"
4. **Explore maturity**: "Where are you on your DevOps journey?"

## Module Learning Path

This module covers:

1. **Core Concepts**: Deep dive into DORA, governance frameworks, and compliance
2. **Guided Walkthrough**: Implementing metrics collection and governance policies
3. **Hands-On Labs**: Build dashboards and compliance reports
4. **Knowledge Check**: Validate your understanding
5. **Resources**: Tools, documentation, and research
6. **CSM Scenarios**: Practice customer conversations

---

**Ready to dive deeper?** Continue to Core Concepts for detailed exploration of governance frameworks and DORA implementation.
