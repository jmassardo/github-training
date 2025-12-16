---
layout: training-module
title: "Real-World CSM Scenarios"
permalink: /beginner/module-1/scenarios/
module_number: 1
module_title: "Understanding Software Development & SDLC"
section_number: 7
total_sections: 7
phase: beginner
estimated_time: "15 min"
module_index: /beginner/module-1/
sections:
  - title: "Context & Overview"
    url: "/beginner/module-1/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/beginner/module-1/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/beginner/module-1/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/beginner/module-1/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/beginner/module-1/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/beginner/module-1/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/beginner/module-1/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
prev_section:
  url: /beginner/module-1/resources/
  title: "Resources"
next_section:
  url: /beginner/module-2/
  title: "Module 2: Git & GitHub Fundamentals"
---

## Apply Your Knowledge

These scenarios simulate real customer conversations you'll encounter as a CSM or CSA. Practice how you'd respond using SDLC concepts.

---

## Scenario 1: The "We're Fine" Customer

### Situation

**Customer:** FinServe Bank (500 developers)

**Context:** Quarterly business review (QBR). They've used GitHub for 2 years but haven't adopted new features.

**The VP of Engineering says:**

> "We're happy with GitHub. We use it for source control and that's really all we need. Our current process works fine — developers commit code, it goes through a manual review process, QA tests for two weeks, and we release quarterly."

### Your Task

How do you identify opportunities and start a conversation about expanding their GitHub usage?

<details markdown="1">
<summary>Show Suggested Approach</summary>

**What you heard:**

- Only using for source control (underutilization)
- Manual review process (no automation)
- 2-week QA cycle (long testing phase)
- Quarterly releases (slow delivery)

**Questions to ask:**

1. "How long does it typically take from when a developer finishes a feature to when customers see it?"
2. "What does your manual review process look like? How many people are involved?"
3. "What happens when you need to deploy an urgent security fix?"

**Bridge to value:**

- Lead time sounds long — "Many of our customers have reduced this from months to days"
- Manual testing is expensive — "What if you could automate 80% of that?"
- Quarterly releases = higher risk — "Smaller, more frequent releases often reduce deployment failures"

**Features to introduce:**

1. **GitHub Actions** for automated testing
2. **Branch protection** for streamlined reviews
3. **GitHub Advanced Security** for continuous security

**Conversation tip:** Don't tell them they're doing it wrong. Ask questions that help them see the cost of their current process.

</details>

---

## Scenario 2: The Migration Customer

### Situation

**Customer:** TechStart (startup, 30 developers)

**Context:** They're migrating from GitLab to GitHub Enterprise Cloud.

**The CTO says:**

> "We moved to GitHub because our investors use it and we needed better security features. But honestly, I'm not sure we're getting value yet. Our team keeps complaining about the transition and we're not seeing the productivity gains we expected."

### Your Task

How do you help them realize value during this critical adoption phase?

<details markdown="1">
<summary>Show Suggested Approach</summary>

**Red flags:**

- Team complaints = adoption friction
- No perceived value = churn risk
- Investor-driven decision = not bottom-up buy-in

**Discovery questions:**

1. "What specific complaints are you hearing from the team?"
2. "What were they doing in GitLab that they can't do in GitHub?"
3. "What does 'productivity gains' look like to you — how would you measure it?"

**Common migration pain points:**

- CI/CD pipeline translation (GitLab CI → GitHub Actions)
- Different workflow (GitLab MRs vs GitHub PRs)
- Feature parity gaps (perceived or real)
- Learning curve on new UI

**Your action plan:**

1. **Office hours session** — Walk through GitHub Actions vs GitLab CI
2. **Quick wins** — Show something they couldn't do before:
   - GitHub Copilot demos
   - Security alerts catching real vulnerabilities
   - Codespaces for instant dev environments
3. **Success metrics** — Help them define what success looks like:
   - PR cycle time
   - Build success rate
   - Time to onboard new developer

**Key message:** "Let's identify 2-3 quick wins that show immediate value while we work on the longer-term improvements."

</details>

---

## Scenario 3: The Security-Focused Customer

### Situation

**Customer:** HealthTech Inc. (200 developers, HIPAA-regulated)

**Context:** New customer, post-sales handoff meeting.

**The CISO says:**

> "We bought GitHub because of GHAS (GitHub Advanced Security), but I need to show my board that we're actually more secure now. They're going to ask about our vulnerability count in 90 days. How do I prove ROI?"

### Your Task

How do you help them demonstrate security value?

<details markdown="1">
<summary>Show Suggested Approach</summary>

**Their need:** Quantifiable security metrics for board reporting

**Phase 1: Enable visibility (Week 1-2)**
1. Enable secret scanning on all repos
2. Enable Dependabot security updates
3. Enable code scanning on top 10 critical repos

**Phase 2: Measure baseline (Week 2-4)**
- Document current vulnerability count
- Categorize by severity (Critical, High, Medium, Low)
- Note mean time to remediation for any existing issues

**Phase 3: Show progress (Week 4-12)**
Track and report:

- **Vulnerabilities prevented** — Secrets blocked before commit
- **Vulnerabilities found** — Code scanning alerts
- **Vulnerabilities fixed** — Dependabot auto-PRs merged
- **MTTR** — Time from alert to fix

**Board-ready metrics:**

```
Before GHAS: Unknown vulnerability count
After 90 days:

- 47 critical secrets blocked
- 156 vulnerabilities identified
- 89% auto-fixed by Dependabot
- MTTR reduced from unknown to 3 days

```

**Your deliverable:** Help them create a simple dashboard or report template they can use for board meetings.

</details>

---

## Scenario 4: The DevOps Transformation

### Situation

**Customer:** RetailCorp (2,000 developers)

**Context:** Executive sponsor call. They're 18 months into a "DevOps transformation" initiative.

**The VP of Platform Engineering says:**

> "Our DevOps transformation is stalling. We've been preaching CI/CD for over a year but only 20% of teams have adopted it. Leadership is questioning whether GitHub is the right platform. How do we accelerate adoption?"

### Your Task

How do you help them drive organization-wide adoption?

<details markdown="1">
<summary>Show Suggested Approach</summary>

**Diagnose the problem:**

1. "What do the 20% who adopted have in common?"
2. "What are the other 80% citing as barriers?"
3. "Is there executive sponsorship and are there consequences for not adopting?"

**Common adoption barriers:**

- **Skills gap** — Teams don't know how to write CI/CD
- **Time** — "We don't have time to learn new tools"
- **Fear** — Automation means less control
- **No mandate** — Optional → not prioritized

**Acceleration strategies:**

**1. Make it easy (reduce friction)**
- Create reusable workflow templates
- Build a "starter kit" repo teams can fork
- Offer Codespaces for zero-setup onboarding

**2. Make it visible (social proof)**
- Publicize early adopter success stories
- Create internal leaderboard/gamification
- Executive shoutouts for teams that adopt

**3. Make it required (guardrails)**
- Branch protection requiring CI checks
- No deployments without Actions pipeline
- Compliance requirements for security scans

**4. Make it supported (enablement)**
- Champions program in each business unit
- Regular office hours
- Internal documentation and FAQs

**Your offer:** "Let's do a pilot with 5 teams who want to adopt but haven't yet. We'll create templates and document the process, then scale what works."

</details>

---

## Scenario 5: The "We Need Training" Request

### Situation

**Customer:** ManuTech (300 developers)

**Context:** Inbound request from your customer.

**The Engineering Manager emails:**

> "Hi, our team needs GitHub training. We just hired 50 new developers and they're struggling with our workflow. Can you provide training?"

### Your Task

How do you respond effectively to this request?

<details markdown="1">
<summary>Show Suggested Approach</summary>

**Don't just say "yes, here's training."** Dig deeper first.

**Discovery email response:**
> "Happy to help with training! To make sure we address the right topics, could you share:
> 1. What aspects are they struggling with most?
> 2. What does your current workflow look like?
> 3. Have they used Git/GitHub before, or is this brand new?
> 4. What does success look like after training?"

**Common underlying issues:**

- **No documentation** — They need internal onboarding docs, not generic training
- **Complex workflow** — The process is hard, not GitHub
- **Tool overload** — They use 10 tools and GitHub is just one more
- **Manager expectations** — Managers expect immediate productivity

**Your options:**

1. **GitHub Skills** — Self-paced, free, covers basics
2. **GitHub Professional Services** — Custom training for specific workflows
3. **Partner training** — Certified partners offer training programs
4. **Internal champions** — Train the trainers approach
5. **Documentation help** — Help them create internal guides

**Value-add opportunity:**
"Beyond training, let's look at whether there are workflow improvements that would make onboarding easier for everyone — not just new hires."

</details>

---

## 🎯 Module 1 Complete!

Congratulations on completing Module 1! You now understand:

- ✅ How software development lifecycle works
- ✅ Different development methodologies (Waterfall, Agile, DevOps)
- ✅ How GitHub features map to SDLC phases
- ✅ The GitHub Well-Architected Framework
- ✅ How to apply these concepts in customer conversations

<div class="callout callout-success">
<div class="callout-title">🚀 Ready for Module 2?</div>

In the next module, we'll dive deep into Git and GitHub fundamentals — the technical foundation that powers everything you learned today.

[Start Module 2: Git & GitHub Fundamentals →]({{ site.baseurl }}/beginner/module-2/)
</div>

---

## Feedback

How was this module? We're continuously improving this training program.

- Too long? Too short?
- Missing topics you need?
- Labs helpful or confusing?

Share feedback with your training coordinator!
