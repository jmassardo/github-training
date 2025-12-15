---
layout: training-module
title: "Knowledge Check"
permalink: /beginner/module-1/quiz/
module_number: 1
module_title: "Understanding Software Development & SDLC"
section_number: 5
total_sections: 7
phase: beginner
estimated_time: "10 min"
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
  url: /beginner/module-1/labs/
  title: "Hands-On Labs"
next_section:
  url: /beginner/module-1/resources/
  title: "Resources"
---

## Test Your Knowledge

Answer these questions to check your understanding of SDLC concepts and how they relate to GitHub.

---

### Question 1: SDLC Phases

**Which SDLC phase focuses on gathering user stories and defining acceptance criteria?**

<details markdown="1">
<summary>Show Answer</summary>

**Requirements/Planning Phase**

This phase involves gathering and documenting what needs to be built. User stories capture the "who, what, why" of features, while acceptance criteria define when a feature is complete.

In GitHub, this maps to creating **Issues** with detailed descriptions and **Projects** for organizing work.
</details>

---

### Question 2: Waterfall vs Agile

**A customer says: "We need complete requirements documents before writing any code, and we deliver one big release per quarter." Which methodology are they using?**

<details markdown="1">
<summary>Show Answer</summary>

**Waterfall**

Key indicators:
- Complete requirements before coding (sequential phases)
- Quarterly releases (infrequent, large releases)
- Emphasis on documentation upfront

A DevOps approach would suggest smaller, more frequent releases with continuous feedback.
</details>

---

### Question 3: GitHub Feature Mapping

**Match the SDLC phase to the GitHub feature:**

| SDLC Phase | GitHub Feature |
|------------|---------------|
| 1. Planning | A. GitHub Actions |
| 2. Development | B. Issues & Projects |
| 3. Testing | C. Code Review |
| 4. Deployment | D. Repositories & Codespaces |

<details markdown="1">
<summary>Show Answer</summary>

1. **Planning** → **B. Issues & Projects** - For tracking work items and organizing sprints
2. **Development** → **D. Repositories & Codespaces** - For writing and storing code
3. **Testing** → **C. Code Review** - Plus Actions for automated tests
4. **Deployment** → **A. GitHub Actions** - For CI/CD automation

Note: GitHub Actions spans both Testing (CI) and Deployment (CD).
</details>

---

### Question 4: DevOps Culture

**What does "shift left" mean in a DevOps context?**

<details markdown="1">
<summary>Show Answer</summary>

**"Shift left" means moving activities earlier in the development process.**

Examples:
- **Security shift left**: Running security scans during development, not just before release
- **Testing shift left**: Writing tests as code is written, not after
- **Quality shift left**: Code review and static analysis during development

GitHub features that enable shift left:
- **Dependabot** - Catches vulnerabilities early
- **Code scanning** - Finds issues during PR review
- **Branch protection** - Requires tests before merge
</details>

---

### Question 5: Customer Scenario

**Your customer says: "Our developers spend 3 days waiting for change approvals before they can deploy." What SDLC principle does this violate, and how could GitHub help?**

<details markdown="1">
<summary>Show Answer</summary>

**Violated Principle:** DevOps principle of fast feedback and continuous delivery

**Problems:**
- Slow lead time (time from commit to production)
- Developer frustration and context switching
- Batch deployments lead to higher risk

**GitHub Solutions:**
1. **Branch protection rules** - Automated checks replace manual gates
2. **Required reviewers** - Parallel code review (async, faster)
3. **GitHub Actions** - Automated testing gives confidence
4. **Environments with required reviewers** - Approval built into deployment workflow
5. **CODEOWNERS** - Right people auto-assigned for review

**Customer conversation tip:** Ask about what's in the 3-day approval process. Often it's manual testing that could be automated.
</details>

---

### Question 6: Well-Architected Framework

**In the GitHub Well-Architected Framework, what does "Collaborative Operational Excellence" focus on?**

<details markdown="1">
<summary>Show Answer</summary>

**Collaborative Operational Excellence** focuses on how teams work together efficiently through:

- **InnerSource practices** - Open source methodology inside the organization
- **Discoverability** - Making code and documentation easy to find
- **Knowledge sharing** - Documentation, wikis, discussions
- **Reducing silos** - Cross-team collaboration

Key enablers in GitHub:
- Internal visibility for repositories
- GitHub Discussions
- README files and wikis
- Code search across the organization
</details>

---

### Question 7: Shift Right Testing

**What does "shift right" testing mean, and what GitHub feature supports it?**

<details markdown="1">
<summary>Show Answer</summary>

**Shift right** means testing and monitoring in production (after deployment).

Concept: You can never fully simulate production, so observe real user behavior and catch issues in the wild.

**Supporting practices:**
- Feature flags (gradual rollout)
- Monitoring and observability
- Canary deployments
- A/B testing

**GitHub feature:** **Environments** with deployment protection rules enable staged rollouts and can integrate with monitoring tools to auto-rollback on errors.
</details>

---

### Question 8: CSM Conversation Starter

**A VP of Engineering asks: "What's the ROI of improving our SDLC?" Name three metrics you could track with GitHub.**

<details markdown="1">
<summary>Show Answer</summary>

**Three trackable metrics:**

1. **Lead Time for Changes**
   - Time from commit to production
   - GitHub Insights shows this
   - Baseline → improvement over time

2. **Deployment Frequency**
   - How often you deploy
   - GitHub Actions workflow runs
   - Daily/weekly vs monthly indicates maturity

3. **Mean Time to Recovery (MTTR)**
   - How fast you fix issues
   - Track time from issue creation to deployment of fix
   - Shows operational resilience

**Bonus metrics:**
- Pull request cycle time
- Code review turnaround
- Build success rate
- Security vulnerability remediation time

These are DORA metrics — we'll cover them in depth in Module 12!
</details>

---

## 🎯 Score Yourself

How'd you do?

| Score | Assessment |
|-------|------------|
| 8/8 | 🌟 Excellent! You've mastered Module 1 concepts |
| 6-7 | 👍 Great job! Review any areas you missed |
| 4-5 | 📚 Good start — revisit the Core Concepts section |
| 0-3 | 🔄 Consider re-reading the module before moving on |

<div class="callout callout-tip">
<div class="callout-title">💡 Pro Tip</div>

Don't worry if you didn't ace every question! The goal is learning, not perfection. These concepts will reinforce as you work through the remaining modules and apply them in real customer conversations.
</div>
