---
layout: training-module
title: "CSM Scenarios"
permalink: /advanced/module-14/scenarios/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 11
total_sections: 11
phase: advanced
estimated_time: "30 min"
module_index: /advanced/module-14/
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
  - title: "CSM Scenarios"
    url: "/advanced/module-14/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "30 min"
toc: true
prev_section:
  url: /advanced/module-14/resources/
  title: "Resources"
---

# Real-World CSM Scenarios

These scenarios focus on advanced Copilot features including Agents, MCP, Extensions, and Enterprise administration. Practice handling these complex customer conversations.

---

## Scenario 14.1: Copilot Agents Governance Concerns

### Situation

An enterprise customer is excited about Copilot Agents but their security team is concerned: "Agents can execute terminal commands and modify files autonomously. How do we prevent an AI from accidentally deleting production code or running destructive commands?"

### Discovery Questions

- "What's your current policy for developers running scripts or commands?"
- "Do you use branch protection and required reviews today?"
- "Are there specific operations you want to prevent entirely?"

### CSM Response Framework

**1. Explain Agent safeguards:**

```markdown
## Built-in Agent Safeguards

### Approval Workflow
- Agents request permission before executing commands
- Developers see exactly what will be run
- Can approve, modify, or reject each action

### Scope Limitations
- Agents work within the current workspace
- Cannot access files outside the project
- Network operations require explicit approval

### Audit Trail
- All agent actions are logged
- Commands executed are recorded
- Can review agent session history
```

**2. Recommend governance controls:**

```markdown
## Enterprise Governance for Agents

### Policy Recommendations
1. **Agent Mode Policies**
   - Disable agent mode org-wide initially
   - Enable for specific teams after training
   - Require approval for production repos

2. **Command Restrictions**
   - Block destructive patterns (rm -rf, DROP TABLE)
   - Whitelist allowed commands
   - Require elevated approval for system commands

3. **Repository Protections**
   - Branch protection still applies
   - Agents cannot bypass required reviews
   - Changes require normal PR process

4. **Monitoring**
   - Audit log captures agent actions
   - Alert on unusual patterns
   - Regular review of agent usage
```

**3. Suggest a phased rollout:**

```markdown
## Agent Rollout Plan

### Phase 1: Sandbox (Week 1-2)
- Enable for test/sandbox repositories only
- Developer training on agent capabilities
- Document use cases and concerns

### Phase 2: Limited Production (Week 3-4)
- Enable for non-critical repositories
- Required approval for any production changes
- Security team monitoring

### Phase 3: Broader Rollout (Week 5+)
- Enable based on team readiness
- Continuous monitoring and feedback
- Adjust policies based on learnings
```

---

## Scenario 14.2: MCP Integration for Internal Systems

### Situation

A customer wants Copilot to understand their internal documentation and proprietary frameworks: "We have 10 years of architecture decisions, internal APIs, and coding standards. Copilot doesn't know any of this. Can we teach it?"

### CSM Response Framework

**1. Introduce MCP as the solution:**

> "Model Context Protocol (MCP) is designed exactly for this. It lets you create servers that provide Copilot with access to your internal knowledge—documentation, APIs, databases, and more."

**2. Explain MCP architecture:**

```markdown
## MCP Solution Architecture

### What MCP Enables
- Connect Copilot to internal documentation
- Query internal databases and APIs
- Access company knowledge bases
- Integrate with ticketing systems

### How It Works
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   VS Code   │────▶│ MCP Server  │────▶│ Internal    │
│  + Copilot  │◀────│  (Custom)   │◀────│ Systems     │
└─────────────┘     └─────────────┘     └─────────────┘

### Common MCP Use Cases
- @internal-docs - Query architecture docs
- @api-spec - Look up internal API definitions
- @standards - Check coding standards
- @jira - Query related tickets
```

**3. Provide implementation guidance:**

```markdown
## MCP Implementation Roadmap

### Phase 1: Identify High-Value Sources (Week 1)
- Survey developers: What knowledge do they look up most?
- Prioritize: Architecture docs, API specs, coding standards
- Assess: Data formats, access controls, freshness

### Phase 2: Build First MCP Server (Week 2-3)
- Start with static documentation (lowest risk)
- Use MCP SDK (TypeScript or Python)
- Deploy locally for testing

### Phase 3: Pilot with Team (Week 4)
- 5-10 developers test the integration
- Gather feedback on accuracy and usefulness
- Iterate on prompts and responses

### Phase 4: Production Deployment (Week 5+)
- Security review of data exposed
- Documentation for developers
- Monitoring and maintenance plan
```

**4. Set expectations:**

> "MCP is powerful but requires investment. You're essentially building a search and retrieval system for your internal knowledge. Start with one high-value source, prove the value, then expand."

---

## Scenario 14.3: Building Custom Copilot Extensions

### Situation

A customer asks: "Our developers use an internal deployment tool. Can we create a Copilot chat participant so they can ask '@deploy status production' instead of switching to another tool?"

### CSM Response Framework

**1. Confirm this is the right solution:**

> "Copilot Extensions are perfect for this. You can create a custom chat participant that integrates with your deployment system. Developers will be able to query deployment status, trigger deploys, and get help—all without leaving their IDE."

**2. Outline the architecture:**

```markdown
## Extension Architecture

### Components Required
1. **GitHub App** - Handles authentication and permissions
2. **Backend Service** - Your extension logic (receives messages, returns responses)
3. **API Integration** - Connects to your deployment system

### Request Flow
Developer: "@deploy status production"
     │
     ▼
┌─────────────────┐
│  GitHub App     │──── Verify user permissions
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Your Backend   │──── Parse intent, call deployment API
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Response       │──── "Production: ✅ Healthy (v2.3.1)"
└─────────────────┘
```

**3. Provide development guidance:**

```markdown
## Extension Development Plan

### Week 1: Foundation
- Create GitHub App in your organization
- Set up backend service (Node.js/Python/etc.)
- Implement basic request/response flow
- Test with "hello world" responses

### Week 2: Core Features
- Integrate with deployment API
- Handle common queries:
  - Status checks
  - Recent deployments
  - Environment comparison
- Error handling and timeouts

### Week 3: Polish
- Add rich formatting (tables, links)
- Implement conversation context
- Add help command (@deploy help)
- Security review

### Week 4: Rollout
- Documentation for developers
- Training on available commands
- Feedback collection
- Iteration based on usage
```

**4. Discuss alternatives:**

> "For simpler use cases, MCP might be sufficient—it's easier to implement than a full extension. Extensions are best when you need bidirectional interaction (like triggering deployments) or organization-wide availability."

---

## Scenario 14.4: Copilot Metrics and ROI Justification

### Situation

Six months into Copilot deployment, the CFO asks: "We're spending $X on Copilot. Can you prove it's worth it? What metrics should we be looking at?"

### CSM Response Framework

**1. Frame the value proposition:**

> "Copilot's value shows up in three areas: developer productivity, code quality, and developer satisfaction. Let me help you build a measurement framework that captures all three."

**2. Present metrics framework:**

```markdown
## Copilot ROI Metrics Framework

### Productivity Metrics
| Metric | How to Measure | Target |
|--------|---------------|--------|
| Acceptance rate | Copilot dashboard | >25% |
| Lines of code suggested/accepted | Copilot dashboard | Baseline + 20% |
| Time to complete tasks | Sprint velocity comparison | 15-20% improvement |
| Boilerplate time reduction | Developer survey | 2+ hours/week saved |

### Quality Metrics
| Metric | How to Measure | Target |
|--------|---------------|--------|
| Test coverage | CI reports before/after | Increase |
| Documentation coverage | Doc linting tools | Increase |
| Bug rate | Issue tracking | Decrease or stable |
| Code review iterations | PR metrics | Decrease |

### Satisfaction Metrics
| Metric | How to Measure | Target |
|--------|---------------|--------|
| Developer NPS for Copilot | Survey | >50 |
| Active usage rate | License dashboard | >60% |
| Feature requests | Feedback channel | Active engagement |
| Retention impact | HR data (optional) | Positive mentions |
```

**3. Help build the business case:**

```markdown
## ROI Calculation Template

### Costs
- Copilot licenses: $X/month × 500 developers = $Y/year
- Training time: 2 hours × 500 developers × $Z/hour = $W
- Total annual cost: $Y + $W

### Savings (Conservative)
- Time saved: 2 hours/week × 48 weeks × $Z/hour × 500 devs
- Reduced context switching: Hard to quantify, estimate 5-10%
- Faster onboarding: New hire productivity 2 weeks earlier

### Sample Calculation
If average developer costs $150K/year ($75/hour):
- 2 hours/week × 48 weeks = 96 hours saved/year
- 96 hours × $75 = $7,200 saved per developer
- 500 developers × $7,200 = $3.6M potential annual value

ROI = ($3.6M - $570K) / $570K = 531% 
(Adjust assumptions based on your data)
```

**4. Set up ongoing measurement:**

> "Let's schedule a quarterly review to track these metrics over time. The real value of Copilot often shows up in trends—developers getting faster at using it, acceptance rates improving, and satisfaction increasing."

---

## Scenario 14.5: Enterprise Policy Configuration

### Situation

A customer with 5,000 developers needs help configuring Copilot policies: "We have different security requirements for different teams. Our security team shouldn't have Copilot enabled, our open-source team wants all features, and our contractors should have limited access."

### CSM Response Framework

**1. Map requirements to policies:**

```markdown
## Policy Requirements Matrix

| Team | Copilot | Chat | Agents | CLI | Content Exclusions |
|------|---------|------|--------|-----|-------------------|
| Security | ❌ Disabled | ❌ | ❌ | ❌ | N/A |
| Open Source | ✅ All | ✅ | ✅ | ✅ | None |
| Contractors | ✅ Limited | ✅ | ❌ | ❌ | Sensitive repos |
| Engineering | ✅ All | ✅ | ✅ | ✅ | HR, Finance repos |
```

**2. Explain the policy hierarchy:**

```markdown
## Copilot Policy Hierarchy

### Enterprise Level (Highest)
- Set baseline policies for all organizations
- Cannot be overridden by orgs or users
- Use for: Universal restrictions (blocked repos)

### Organization Level
- Override enterprise policies (if allowed)
- Apply to all members of the org
- Use for: Team-specific enablement

### User Level (Lowest)
- Individual preferences
- Only within allowed boundaries
- Use for: Personal opt-in/opt-out

### Implementation
Enterprise Policy → Org Policy → User Settings
(Most restrictive wins at each level)
```

**3. Provide implementation plan:**

```markdown
## Policy Implementation

### Step 1: Content Exclusions (Day 1)
Define repositories that should never be processed:
- Security team repositories
- HR and legal repositories
- M&A related projects
- Customer data repositories

### Step 2: Feature Policies (Day 2)
Configure feature availability:
- Copilot in IDE: Enabled (default)
- Copilot Chat: Enabled (default)
- Copilot Agents: Disabled (default), opt-in
- Copilot CLI: Disabled (default), opt-in

### Step 3: Team Assignments (Day 3-5)
- Create policy groups
- Assign teams to appropriate policies
- Communicate changes to affected users

### Step 4: Verification (Day 6-7)
- Test access with users from each group
- Verify exclusions are working
- Document exceptions process
```

**4. Plan for ongoing management:**

```markdown
## Governance Process

### New Team Onboarding
1. Security review of team's work
2. Assign appropriate policy group
3. Training on allowed features
4. Quarterly access review

### Policy Change Requests
1. Request submitted with justification
2. Security team review
3. Approval from team lead + security
4. Implementation with audit trail

### Exception Process
1. Document business need
2. Time-limited approval (90 days max)
3. Automatic review at expiration
4. Logged in compliance system
```

---

## Scenario 14.6: Copilot Product Family Questions

### Situation

A customer is confused about the different Copilot capabilities: "There's Copilot in the IDE, agent mode, the coding agent on GitHub.com, and now Copilot code review. How do they all fit together? What should our developers be using?"

### CSM Response Framework

**1. Clarify the Copilot product family:**

```markdown
## GitHub Copilot Capabilities

### Copilot Code Completions (Module 13)
- Inline code suggestions in your editor
- Ghost text that appears as you type
- Works in VS Code, JetBrains, Neovim, etc.
- **Use for**: Line-by-line coding acceleration

### Copilot Chat (Module 13)
- AI conversation in your IDE, GitHub.com, or CLI
- Ask questions, explain code, generate snippets
- Supports @workspace, @github, and other participants
- **Use for**: Q&A, learning, code explanation

### Agent Mode (Module 14)
- Autonomous multi-file editing from a single prompt
- Runs terminal commands, installs dependencies
- Iterates on errors automatically
- **Use for**: Multi-step implementation from a natural language request

### Copilot Coding Agent (Module 14)
- Assign an issue to Copilot on GitHub.com
- Copilot opens a PR with the implementation
- Runs in a cloud environment asynchronously
- **Use for**: Well-defined issues, bug fixes, small features

### Copilot Code Review
- Automated review comments on pull requests
- Suggests improvements, catches issues
- Complements human reviewers
- **Use for**: Faster, more consistent code reviews

### Copilot CLI
- AI assistance in the terminal
- Explain commands, suggest solutions
- **Use for**: DevOps and command-line work
```

**2. Explain when to use each:**

> "Think of it as a spectrum: Code completions help line by line, Chat answers questions, Agent Mode handles bigger tasks interactively, and the Coding Agent works on issues in the background. Most developers use all of these throughout their day depending on the task."

**3. Address common confusion:**

```markdown
## Quick Decision Guide

| I want to... | Use |
|--------------|-----|
| Write code faster line by line | Code Completions |
| Ask a question about my code | Copilot Chat |
| Implement a feature across multiple files | Agent Mode |
| Assign a GitHub issue to AI | Coding Agent |
| Get AI feedback on a pull request | Copilot Code Review |
| Get help with terminal commands | Copilot CLI |
```

---

## Key Takeaways

| Scenario | Key Response Strategy |
|----------|----------------------|
| **Agent governance** | Emphasize built-in safeguards; recommend phased rollout |
| **MCP integration** | Position as knowledge bridge; start small and expand |
| **Custom extensions** | Validate use case; provide clear development roadmap |
| **ROI justification** | Framework approach; conservative assumptions; track trends |
| **Enterprise policies** | Map requirements to hierarchy; plan implementation |
| **Product confusion** | Clear taxonomy of capabilities; explain when to use each |

### Resources for Advanced Copilot Conversations

- [Copilot Enterprise Administration](https://docs.github.com/en/copilot/managing-copilot/managing-copilot-for-your-enterprise)
- [MCP Documentation](https://modelcontextprotocol.io/)
- [Building Copilot Extensions](https://docs.github.com/en/copilot/building-copilot-extensions)
- [Copilot Metrics API](https://docs.github.com/en/rest/copilot/copilot-metrics)
- [Copilot Trust Center](https://copilot.github.trust.page/)
