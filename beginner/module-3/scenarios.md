---
layout: training-module
title: "Real-World CSM Scenarios"
permalink: /beginner/module-3/scenarios/
module_number: 3
module_title: "GitHub Platform Essentials"
section_number: 7
total_sections: 7
phase: beginner
estimated_time: "15 min"
module_index: /beginner/module-3/
sections:
  - title: "Context & Overview"
    url: "/beginner/module-3/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/beginner/module-3/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/beginner/module-3/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/beginner/module-3/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/beginner/module-3/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/beginner/module-3/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/beginner/module-3/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
prev_section:
  url: /beginner/module-3/resources/
  title: "Resources"
next_section:
  url: /beginner/module-4/
  title: "Module 4: SCM Process"
---

## Apply Your Knowledge

Practice handling real customer situations using your GitHub platform knowledge.

---

## Scenario 1: The Jira Migration Question

### Situation

**Customer:** TechFlow (100 developers)

**Context:** Discovery call with the Engineering Director.

**They ask:**

> "We're using Jira for project management but considering GitHub Projects. What can Projects do that Jira can't? And what would we lose?"

### Your Task

How do you position GitHub Projects honestly?

<details markdown="1">
<summary>Show Suggested Approach</summary>

**GitHub Projects advantages:**

1. **Native integration** — Issues and PRs live in Projects automatically
2. **Simpler pricing** — Included with GitHub, no extra cost
3. **Developer-centric** — Built for code workflows
4. **Less context switching** — Everything in one place
5. **Custom fields** — Flexible metadata without admin overhead

**What Jira does better (be honest):**

1. **Advanced reporting** — Burndown charts, velocity tracking
2. **Portfolio management** — Multi-project roadmaps
3. **Customization depth** — Complex workflows, custom issue types
4. **Enterprise integrations** — Deep Atlassian ecosystem

**Honest positioning:**
> "GitHub Projects excels at developer workflow integration and simplicity. If your current Jira usage is straightforward sprint tracking, Projects can replace it and reduce tool sprawl. If you rely on advanced portfolio features or have heavily customized Jira workflows, you might use both — GitHub for day-to-day and Jira for executive reporting."

**Questions to ask:**

- "What Jira features do you use most?"
- "How much customization have you built in Jira?"
- "Who consumes your project data beyond engineering?"

</details>


## Scenario 2: The "Too Many Notifications" Complaint

### Situation

**Customer:** DataScale (50 developers)

**Context:** Check-in call with your champion.

**They say:**

> "Our developers are overwhelmed with GitHub notifications. They get hundreds per day and miss important ones. Some have started ignoring notifications entirely."

### Your Task

Help them manage notification overload.

<details markdown="1">
<summary>Show Suggested Approach</summary>

**Diagnose the problem:**

- "What types of notifications are flooding them?"
- "Are they subscribed to all repositories?"
- "Are they watching repositories vs participating?"

**Notification sources:**

- **Watching** — All activity on a repo
- **Participating** — Only threads you're in
- **@mentions** — When someone tags you
- **Review requests** — PRs assigned for review
- **Subscribed** — Specific threads you chose

**Solutions:**

1. **Adjust watching settings:**
   - Go to repository → Watch dropdown
   - Change from "All Activity" to "Participating and @mentions"
   - Unwatch repos they don't need

2. **Configure notification settings:**
   - Settings → Notifications
   - Customize what sends email vs web only
   - Set up notification routing (different email for different orgs)

3. **Use GitHub Mobile:**
   - Push notifications for important items
   - Quick triage on the go

4. **Leverage CODEOWNERS:**
   - Only get notified for code you own
   - Reduces noise on large repos

5. **Create team conventions:**
   - Don't @-mention large groups unnecessarily
   - Use targeted @-mentions (@username vs @team)
   - Clear guidelines on when to subscribe others

**Pro tip:** GitHub has a notification inbox (github.com/notifications) that allows filtering, marking as done, and organizing — better than email for triage.

</details>

---

## Scenario 3: The "We Need Better Visibility" Request

### Situation

**Customer:** FinanceApp (200 developers, multiple teams)

**Context:** QBR with VP of Engineering.

**They ask:**

> "Leadership wants to know what engineering is working on without bugging developers in Slack every week. How can we get visibility without adding overhead?"

### Your Task

Recommend a visibility strategy using GitHub features.

<details markdown="1">
<summary>Show Suggested Approach</summary>

**GitHub features for visibility:**

1. **Organization-level Projects:**
   - Create projects that span repositories
   - Executives can see work across teams
   - Custom views per audience (team view vs exec view)

2. **README files:**
   - Each repo has updated README with status
   - Organization profile README for overview

3. **GitHub Insights (Enterprise):**
   - Contribution metrics
   - PR velocity
   - Code frequency

4. **Milestone tracking:**
   - Release milestones show progress percentage
   - Easy to see "70% complete for v2.0"

5. **Repository topics:**
   - Tag repos by team, product, status
   - Searchable across organization

**Implementation recommendation:**

**Phase 1: Quick wins (Week 1)**
- Set up org-level Project board
- Create "Leadership View" with status column

**Phase 2: Automation (Week 2-3)**
- Auto-add issues/PRs to project when labeled
- Status updates from PR merge events

**Phase 3: Reporting (Week 4+)**
- Weekly project snapshot
- Milestone progress reports
- Consider GitHub API for custom dashboards

**Key message:**
> "The goal is passive visibility — information that updates automatically through normal work, not extra reporting burden on developers."

</details>


## Scenario 4: The Permission Question

### Situation

**Customer:** SecureCorp (Enterprise Cloud customer)

**Context:** Security team has questions.

**The CISO asks:**

> "We need to give contractors read access to certain repos without seeing our internal documentation or other projects. Can GitHub handle our complex permission needs?"

### Your Task

Explain GitHub's permission model.

<details markdown="1">
<summary>Show Suggested Approach</summary>

**GitHub permission levels:**

| Level | Capabilities |
|-------|-------------|
| **Read** | View code, clone, open issues |
| **Triage** | Manage issues/PRs without code write |
| **Write** | Push code, merge PRs |
| **Maintain** | Manage repo without dangerous actions |
| **Admin** | Full control including settings |

**Solutions for their scenario:**

1. **Create a Contractors team:**
   - Add external collaborators to specific team
   - Grant team Read access to specific repos

2. **Use repository visibility:**
   - Keep sensitive repos Private
   - Contractors can't see repos they're not invited to

3. **Internal documentation:**
   - Move sensitive docs to Private repo they can't access
   - Or use Wiki with restricted access

4. **Enterprise Managed Users (EMU):**
   - For stricter control, provision contractor accounts from your IdP
   - Full audit trail
   - Auto-deprovisioning when contract ends

5. **Custom repository roles (Enterprise):**
   - Create role between Read and Write
   - Example: "External Contributor" with limited capabilities

**Best practices:**

- Regularly audit collaborator access
- Use team-based access (not individual invites)
- Document who has access to what
- Review quarterly

</details>

---

## Scenario 5: The Feature Discovery

### Situation

**Customer:** StartupXYZ (20 developers, GitHub Team)

**Context:** Routine check-in call.

**During conversation, they mention:**

> "Oh, we built our own bot to auto-assign reviewers based on file changes. Took us a few weeks."

### Your Task

You recognize they've reinvented CODEOWNERS. How do you handle this?

<details markdown="1">
<summary>Show Suggested Approach</summary>

**The discovery:** They built something GitHub provides natively!

**Handle diplomatically:**
> "That's great that you automated reviewer assignment! I'm curious — have you explored CODEOWNERS? It's a built-in GitHub feature that does exactly this. It might simplify your maintenance if you're open to looking at it."

**Explain CODEOWNERS:**

- Simple text file in repo (`.github/CODEOWNERS`)
- Pattern-based ownership (by path, file type)
- Auto-requests reviews from code owners
- Works with branch protection

**Example CODEOWNERS:**

```
# Default owners
* @tech-leads

# Frontend team owns UI
/src/components/ @frontend-team

# Security reviews all auth code
/src/auth/ @security-team

# Specific file owners
package.json @dependency-managers

```

**Benefits over custom bot:**

- Zero maintenance
- Native GitHub integration
- Visible in PR UI
- Works with required reviews

**Be sensitive:**

- Don't make them feel bad about their work
- Acknowledge their solution worked
- Present CODEOWNERS as an option, not criticism
- Offer to help migrate if they want

**Broader lesson:**
> "This is a good reminder for me — let's schedule a feature discovery session. There might be other native features that could replace custom tooling and reduce maintenance."

</details>


## 🎯 Module 3 Complete!

You now understand:

- ✅ The GitHub platform architecture and product tiers
- ✅ Repositories, issues, pull requests, and projects
- ✅ Navigation and key features of the GitHub interface
- ✅ How to help customers with platform adoption

<div class="callout callout-success">
<div class="callout-title">🚀 Ready for Module 4?</div>

Next, we'll dive into SCM processes — branching strategies, code review workflows, and release management.

[Start Module 4: SCM Process →](/beginner/module-4/)
</div>

---

## Feedback

How was this module? Share feedback with your training coordinator!
