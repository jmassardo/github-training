---
layout: training-module
title: "CSM Scenarios"
permalink: /advanced/module-13/scenarios/
module_number: 13
module_title: "GitHub Copilot Fundamentals"
section_number: 11
total_sections: 11
phase: advanced
estimated_time: "30 min"
module_index: /advanced/module-13/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-13/overview/"
    short_title: "Overview"
    icon: "📋"
    time: "20 min"
  - title: "Core Concepts"
    url: "/advanced/module-13/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "30 min"
  - title: "IDE Setup"
    url: "/advanced/module-13/setup/"
    short_title: "Setup"
    icon: "⚙️"
    time: "25 min"
  - title: "Prompt Crafting"
    url: "/advanced/module-13/prompts/"
    short_title: "Prompts"
    icon: "✍️"
    time: "30 min"
  - title: "Copilot Chat"
    url: "/advanced/module-13/chat/"
    short_title: "Chat"
    icon: "💬"
    time: "25 min"
  - title: "IDE Differences"
    url: "/advanced/module-13/ide-differences/"
    short_title: "IDE Differences"
    icon: "🔀"
    time: "20 min"
  - title: "Best Practices"
    url: "/advanced/module-13/best-practices/"
    short_title: "Best Practices"
    icon: "✅"
    time: "25 min"
  - title: "Hands-On Labs"
    url: "/advanced/module-13/labs/"
    short_title: "Labs"
    icon: "🧪"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/advanced/module-13/quiz/"
    short_title: "Quiz"
    icon: "📝"
    time: "15 min"
  - title: "Resources"
    url: "/advanced/module-13/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/advanced/module-13/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "30 min"
toc: true
prev_section:
  url: /advanced/module-13/resources/
  title: "Resources"
next_module:
  url: /advanced/module-14/
  title: "Module 14: GitHub Copilot Advanced"
---

# Real-World CSM Scenarios

These scenarios represent common customer situations you'll encounter when helping organizations adopt GitHub Copilot. Practice your responses and build confidence handling these conversations.

---

## Scenario 13.1: Developer Skepticism

### Situation

A customer just purchased Copilot Business for 500 developers. During the kickoff call, the engineering director mentions: "Honestly, my senior developers think AI coding assistants are overhyped. They say Copilot just produces buggy code that wastes more time reviewing than it saves."

### Discovery Questions

Before responding, gather more context:

- "Have any of your developers tried Copilot before? What was their experience?"
- "What types of projects do your senior developers work on? (Legacy code, greenfield, specific languages?)"
- "How does your team currently handle boilerplate code and documentation?"

### CSM Response Framework

**1. Acknowledge the concern (don't dismiss it):**

> "That's actually a common concern from experienced developers, and it's valid—Copilot isn't magic. The developers who get the most value approach it as a tool that needs to be learned, just like any other."

**2. Reframe expectations:**

> "Senior developers often expect Copilot to understand their entire codebase and write complex business logic. That's not what it's best at. Where it shines is eliminating the tedious stuff—boilerplate, test scaffolding, documentation, and common patterns."

**3. Suggest a targeted pilot:**

```markdown
## Pilot Program for Skeptical Teams

### Week 1-2: Focus on Quick Wins
- Unit test generation (@workspace /tests)
- Documentation and comments
- Boilerplate code (API endpoints, data models)
- Code explanations for unfamiliar codebases

### Week 3-4: Build Habits
- Prompt crafting workshop
- Share successful prompts in team channel
- Identify team-specific use cases

### Metrics to Track
- Time spent on documentation
- Test coverage before/after
- Developer sentiment (quick survey)
```

**4. Set realistic success criteria:**

> "Let's define success as 'developers find at least 2-3 use cases where Copilot saves them time.' Not every suggestion will be perfect, but if it eliminates even an hour of boilerplate work per week, that's 50+ hours per developer per year."

---

## Scenario 13.2: Security and Privacy Concerns

### Situation

The CISO reaches out before the Copilot rollout with concerns: "We have strict data handling requirements. I need to understand exactly what code leaves our environment and where it goes. Can you guarantee our proprietary code won't be used to train AI models?"

### Key Points to Address

| Concern | Response |
|---------|----------|
| **Data transmission** | Code context is sent to GitHub's Copilot service for processing |
| **Data retention** | With Copilot Business/Enterprise, prompts and suggestions are NOT retained |
| **Training data** | Business/Enterprise code is NOT used for model training |
| **Compliance** | Copilot runs on Azure infrastructure with SOC 2, ISO 27001, GDPR compliance |

### CSM Response Framework

**1. Provide clear documentation:**

> "These are exactly the right questions to ask. Let me share our Trust Center documentation that addresses each of these concerns with specifics."

**Resources to share:**
- [GitHub Copilot Trust Center](https://copilot.github.trust.page/)
- [Copilot Privacy FAQ](https://docs.github.com/en/copilot/copilot-business/about-github-copilot-business#privacy)
- [Data handling practices](https://docs.github.com/en/copilot/overview-of-github-copilot/about-github-copilot-individual#about-privacy)

**2. Highlight Business/Enterprise controls:**

```markdown
## Security Controls (Business/Enterprise)

### Data Handling
- ✅ Prompts and suggestions NOT retained after processing
- ✅ Code NOT used for training models
- ✅ Encrypted in transit (TLS 1.2+)

### Administrative Controls
- Content exclusion policies (block specific repos)
- IP allow lists
- Audit logs for Copilot usage
- Centralized enablement/disablement

### Compliance
- SOC 2 Type II certified
- ISO 27001 certified
- GDPR compliant
- Available in EU data residency (Enterprise)
```

**3. Offer a technical deep-dive:**

> "Would it be helpful to schedule a call with our security team to walk through the architecture in detail? They can answer specific questions about data flows and controls."

---

## Scenario 13.3: Low Adoption After Rollout

### Situation

Three months after deploying Copilot Business to 200 developers, the customer reports: "Our adoption metrics show only 30% of developers are actively using Copilot. We're not seeing the ROI we expected. What should we do?"

### Discovery Questions

- "How was Copilot introduced to the team? Was there training?"
- "Do developers know how to access it in their IDE?"
- "Are there specific teams or projects with higher/lower adoption?"
- "What feedback have you received from developers who aren't using it?"

### Common Root Causes

| Symptom | Likely Cause | Solution |
|---------|--------------|----------|
| Developers don't know about it | Poor communication | Re-launch with enablement |
| Developers tried it once and stopped | Bad first experience | Training on effective use |
| Works for some teams, not others | Language/IDE gaps | Check compatibility |
| Actively disabled by developers | Distrust or frustration | Address concerns directly |

### CSM Response Framework

**1. Diagnose the adoption blockers:**

```markdown
## Adoption Assessment

### Quick Survey Questions
1. Are you aware Copilot is available to you? (Y/N)
2. Have you enabled the extension in your IDE? (Y/N)
3. How often do you use Copilot? (Never/Rarely/Sometimes/Often)
4. What prevents you from using Copilot more?
   - [ ] Don't know how to use it effectively
   - [ ] Suggestions aren't helpful for my work
   - [ ] Slows down my workflow
   - [ ] Privacy concerns
   - [ ] Prefer to write code myself
   - [ ] Other: ___
```

**2. Propose a re-enablement program:**

```markdown
## 30-Day Adoption Sprint

### Week 1: Awareness
- All-hands announcement with executive sponsor
- Share success stories from early adopters
- Verify all developers have access

### Week 2: Enablement
- Live workshops: "Getting Started with Copilot"
- Focus on quick wins (tests, docs, boilerplate)
- Office hours for questions

### Week 3: Activation
- Team challenges (most creative use case)
- Share prompt libraries
- Peer learning sessions

### Week 4: Measurement
- Track active users daily
- Survey sentiment changes
- Identify and address remaining blockers
```

**3. Set realistic targets:**

> "Industry benchmarks suggest 60-70% active usage is a healthy target. Not every developer will use it daily, and that's okay. Let's focus on ensuring everyone has the skills to use it when it's valuable for them."

---

## Scenario 13.4: Copilot Suggests Incorrect Code

### Situation

A developer escalates through their manager: "Copilot suggested code that introduced a security vulnerability in our production app. How can we trust it for anything sensitive?"

### Immediate Response

**1. Acknowledge and investigate:**

> "I'm sorry to hear that happened. Can you share details about what was suggested and how it got to production? This will help us understand the situation and prevent it from happening again."

**2. Clarify the shared responsibility model:**

```markdown
## Copilot Code Review Responsibility

### What Copilot Does
- Suggests code based on context and patterns
- Provides a starting point for implementation
- Accelerates common coding tasks

### What Copilot Does NOT Do
- Guarantee code correctness
- Understand your security requirements
- Replace code review processes
- Test code before suggesting

### Developer Responsibility
- Review ALL suggestions before accepting
- Apply same scrutiny as any other code
- Run security scans and tests
- Follow existing code review processes
```

**3. Recommend guardrails:**

```markdown
## Preventing Copilot-Related Issues

### Technical Controls
- Enable GHAS code scanning for all repos
- Add security-focused CodeQL queries
- Require PR reviews for all changes
- Block direct pushes to main branch

### Process Controls
- Train developers: "Copilot suggests, humans decide"
- Add Copilot guidance to code review checklist
- Security team reviews AI-assisted code patterns

### Cultural Controls
- Blame the process, not the tool or developer
- Treat Copilot suggestions like Stack Overflow code
- Encourage reporting of bad suggestions
```

**4. Prevent recurrence:**

> "Let's set up a 30-minute call to review your code review process and identify where additional guardrails might help. This is a good opportunity to strengthen your security practices overall, not just for Copilot."

---

## Scenario 13.5: Feature Request – Copilot in Restricted Environment

### Situation

A financial services customer asks: "We love Copilot, but our trading systems team works in an air-gapped environment with no internet access. Can we run Copilot locally?"

### CSM Response Framework

**1. Clarify current limitations:**

> "Today, Copilot requires connectivity to GitHub's cloud service—there's no on-premises or offline version. The AI models are too large to run locally on developer machines."

**2. Discuss alternatives:**

```markdown
## Options for Restricted Environments

### Option 1: Network Segmentation
- Allow Copilot traffic through specific endpoints
- Use content exclusion for sensitive repos
- Works if "air-gapped" is policy vs. technical

### Option 2: Selective Rollout
- Enable Copilot for non-sensitive projects
- Developers use Copilot for open-source work
- Training transfers to restricted work

### Option 3: Copilot Enterprise (Future)
- Azure private endpoints
- Virtual network integration
- Check roadmap for availability
```

**3. Capture as feedback:**

> "I'll document this as a feature request. Copilot in restricted environments is something we hear about from regulated industries. While I can't promise a timeline, your use case helps prioritize roadmap decisions."

**4. Offer a workaround:**

> "Some customers with similar requirements use Copilot on a separate, less-restricted workstation for learning and prototyping, then apply those patterns manually in the restricted environment. It's not ideal, but it lets developers build AI-assisted coding skills."

---

## Key Takeaways

| Scenario | Key Response Strategy |
|----------|----------------------|
| **Developer skepticism** | Focus on targeted use cases, not magic; pilot with quick wins |
| **Security concerns** | Lead with documentation; highlight Business/Enterprise controls |
| **Low adoption** | Diagnose root cause; re-launch with training and support |
| **Incorrect suggestions** | Reinforce shared responsibility; add technical guardrails |
| **Restricted environments** | Be honest about limitations; capture feedback; suggest workarounds |

### Resources for Customer Conversations

- [GitHub Copilot Trust Center](https://copilot.github.trust.page/)
- [Copilot for Business FAQ](https://docs.github.com/en/copilot/copilot-business/about-github-copilot-business)
- [Copilot Research and Data](https://github.blog/2022-09-07-research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness/)
- [Best Practices Guide](https://docs.github.com/en/copilot/using-github-copilot/best-practices-for-using-github-copilot)
