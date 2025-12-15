---
layout: training-module
title: "CSM Scenarios"
permalink: /advanced/module-12/scenarios/
module_number: 12
module_title: "Governance, Compliance & DORA Metrics"
section_number: 7
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
prev_section:
  url: /advanced/module-12/resources/
  title: "Resources"
---

## Customer Success Scenarios

Practice handling customer conversations about governance, compliance, and engineering metrics.

---

## Scenario 1: The Executive Dashboard Request

### Situation

**Customer:** MegaCorp Industries (Enterprise, 5,000+ developers)

**Contact:** Jennifer, VP of Engineering

**Request:** "Our CEO keeps asking me how productive engineering is. I need a dashboard showing our team's performance. What can GitHub provide?"

### Discovery Questions

<details>
<summary>Show Discovery Questions</summary>

1. "What does 'productive' mean to your CEO? What outcome are they looking for?"
2. "How are you measuring engineering success today?"
3. "What decisions would this dashboard inform?"
4. "Are there specific concerns driving this request?"
5. "Who else needs access to this data?"
</details>

### Recommended Response

<details>
<summary>Show Response Strategy</summary>

**Phase 1: Reframe the Conversation**
"Great question, and one we hear often. Before diving into dashboards, let me share what research tells us about engineering measurement."

**Phase 2: Introduce DORA**
"Google's DORA research studied thousands of teams to find metrics that actually predict both organizational performance AND developer well-being. They identified four key metrics:

1. **Deployment Frequency** - How often you ship
2. **Lead Time** - How fast you go from code to production
3. **MTTR** - How quickly you recover from problems
4. **Change Failure Rate** - How often deploys cause issues

The key insight: Elite teams are both faster AND more stable."

**Phase 3: Connect to Business Value**
"Rather than measuring 'productivity' directly, these metrics show:
- How efficiently you deliver value (frequency, lead time)
- How resilient your systems are (MTTR, failure rate)
- Where bottlenecks exist in your process

This gives your CEO confidence that engineering is improving continuously."

**Phase 4: Propose Solution**
"GitHub can help you:
1. Collect these metrics through our APIs and Actions
2. Build dashboards using your existing BI tools
3. Set up automated weekly reports
4. Benchmark against industry standards

Would you like to start with a pilot on one or two teams?"
</details>

---

## Scenario 2: The Compliance Panic

### Situation

**Customer:** HealthFirst Systems (Healthcare, HIPAA-regulated)

**Contact:** David, IT Security Director

**Problem:** "Our auditor just told us we need to demonstrate change control for all code deployments. We have 30 days before the audit. Help!"

### Discovery Questions

<details>
<summary>Show Discovery Questions</summary>

1. "What specific evidence is the auditor requesting?"
2. "How many repositories are in scope?"
3. "Do you currently have branch protection or review requirements?"
4. "Are you using GitHub Enterprise Cloud or Server?"
5. "Do you have audit log streaming configured?"
</details>

### Recommended Response

<details>
<summary>Show Response Strategy</summary>

**Phase 1: Immediate Assessment**
"Let's assess what you have today and what gaps exist. GitHub Enterprise Cloud provides several compliance features out of the box."

**Phase 2: Quick Wins (This Week)**
1. **Enable audit log streaming** to capture all administrative actions
2. **Create organization ruleset** requiring PR reviews for all default branches
3. **Export current branch protection** settings as documentation
4. **Generate repository security report** showing current state

**Phase 3: Evidence Collection**
"For your auditor, you'll be able to provide:
- **Audit log exports** showing all code changes went through PRs
- **Branch protection evidence** showing review requirements
- **Approval records** from merged PRs
- **CODEOWNERS files** showing ownership

**Phase 4: Longer-term Hardening**
After the audit, let's:
- Implement comprehensive governance policies
- Set up drift detection
- Create automated compliance reporting

The good news: GitHub's features are designed with compliance in mind. We can get you compliant and provide ongoing evidence."
</details>

---

## Scenario 3: The Metrics Skeptic

### Situation

**Customer:** DevFirst Software (Mid-market, 200 developers)

**Contact:** Alex, Engineering Manager

**Objection:** "We tried measuring engineering metrics before and it backfired. Developers gamed the system and morale tanked. I'm not doing that again."

### Handling the Objection

<details>
<summary>Show Response Strategy</summary>

**Acknowledge Their Experience**
"That's a really common experience, and I appreciate your caution. Bad metrics programs can absolutely harm team culture. Can you share what metrics you tried before?"

**Diagnose the Problem**
Common issues:
- Measuring individual developers (creates competition)
- Lines of code or commits (encourages gaming)
- Using metrics for performance reviews (fear-based)
- Public rankings (shame culture)

**Introduce DORA Differently**
"DORA metrics are different because:

1. **They're team-level, not individual** - No comparing developers
2. **They measure outcomes, not activity** - Can't game by writing more code
3. **They're balanced** - Speed without stability doesn't help
4. **Research-validated** - Not just opinions

Most importantly: Teams that improve on DORA metrics also report higher job satisfaction."

**Propose Safe Implementation**
"What if we:
1. Start with private metrics only the team sees
2. Focus on improvement trends, not absolute numbers
3. Let the team choose what to focus on
4. Never tie directly to performance reviews

The goal isn't surveillance—it's helping teams identify where they're stuck and celebrating when they improve."
</details>

---

## Scenario 4: The Governance vs. Speed Debate

### Situation

**Customer:** FastShip Inc (Startup acquired by Enterprise)

**Contact:** Maria, CTO (from startup), and Robert, CISO (from enterprise)

**Conflict:** Maria wants developer freedom and speed. Robert wants controls and compliance. They're at an impasse.

### Facilitating Resolution

<details>
<summary>Show Response Strategy</summary>

**Acknowledge Both Perspectives**
"You're both right. Developer velocity IS critical for competitive advantage. AND security controls ARE necessary for enterprise scale. The question is: how do we get both?"

**Challenge the False Dichotomy**
"Here's what research shows: The fastest teams also have the best security posture. They're not trading off—they're using automation to get both."

**Present the 'Paved Path' Model**

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph PavedPath["The Paved Path Model"]
    subgraph Guardrails["🛡️ Guardrails (invisible to developers)"]
      G1["Branch protection (enforced)"]
      G2["Security scanning (automated)"]
      G3["Dependency updates (automated)"]
      G4["Audit logging (automatic)"]
    end
    
    subgraph SelfService["🚀 Self-Service (developer freedom)"]
      S1["Repository creation (templated)"]
      S2["Deployments (automated)"]
      S3["Environment management"]
      S4["Tool selection (within bounds)"]
    end
  end
</div>
</div>

**Propose Concrete Steps**
"For Maria: Developers can self-serve and ship fast, with automation handling compliance.

For Robert: Every repository automatically has security controls, with audit trail for everything.

Let's design guardrails that are invisible to developers unless they try to do something risky."
</details>

---

## Scenario 5: The Value Justification

### Situation

**Customer:** TechManufacturing Corp (Enterprise renewal coming up)

**Contact:** Patricia, Engineering VP

**Challenge:** "We're renewing GitHub Enterprise next quarter. Finance is asking what value we've gotten. Can you help me build the business case?"

### Building the Case

<details>
<summary>Show Response Strategy</summary>

**Phase 1: Gather Data**
"Let's collect concrete metrics from your usage:
- Deployment frequency before/after
- Lead time improvements
- Security vulnerabilities caught
- Developer survey results
- Time saved on manual processes"

**Phase 2: Calculate ROI**

```
Value Categories:

1. DEVELOPER PRODUCTIVITY
   - Hours saved per developer per week: X
   - Number of developers: Y
   - Cost per developer hour: $Z
   - Annual value: X × Y × Z × 52

2. SECURITY RISK REDUCTION
   - Vulnerabilities caught before production
   - Estimated cost per production vulnerability
   - Breaches prevented value

3. OPERATIONAL EFFICIENCY
   - Reduced infrastructure management
   - Automated compliance reporting
   - Faster onboarding

4. INNOVATION ACCELERATION
   - Faster time to market
   - More experiments shipped
   - Revenue from faster features

```

**Phase 3: Build the Narrative**
"Beyond the numbers, the story matters:
- Before: Manual processes, security gaps, slow delivery
- After: Automated pipelines, continuous security, faster innovation
- Future: AI-assisted development with Copilot

Position GitHub as foundational infrastructure for engineering excellence."

**Phase 4: Provide Comparison**
"What would it cost to NOT have GitHub?
- Build vs. buy analysis
- Alternative platform costs
- Migration risk and cost
- Lost productivity during transition"
</details>

---

## Role Play Exercise

Practice these scenarios with a colleague:

1. One person plays the CSM, the other plays the customer
2. Work through discovery and response
3. Switch roles and repeat

### Evaluation Criteria

| Criterion | What to Look For |
|-----------|------------------|
| Discovery | Did you ask questions before proposing? |
| DORA Knowledge | Did you accurately explain metrics? |
| Business Value | Did you connect features to outcomes? |
| Objection Handling | Did you acknowledge concerns first? |
| Actionable | Did you propose concrete next steps? |

---

## Module Complete! 🎉

You've completed Module 12: Governance, Compliance & DORA Metrics.

**Key Takeaways:**
- DORA metrics predict organizational AND team performance
- Governance enables speed through guardrails, not gates
- Compliance can be continuous, not just audit-time
- Metrics should improve teams, not punish individuals

**Next Module:** [Module 13: GitHub Copilot Fundamentals](/advanced/module-13/)
