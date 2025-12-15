---
layout: training-module
title: "Hands-On Labs"
permalink: /beginner/module-1/labs/
module_number: 1
module_title: "Understanding Software Development & SDLC"
section_number: 4
total_sections: 7
phase: beginner
estimated_time: "30 min"
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
  url: /beginner/module-1/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /beginner/module-1/quiz/
  title: "Knowledge Check"
---

## Lab Overview

These hands-on exercises will help you practice what you've learned about SDLC and GitHub. Complete them in order for the best experience.

<div class="callout callout-info">
<div class="callout-title">📋 Prerequisites</div>

- GitHub account (free tier works)
- Web browser
- ~30 minutes of uninterrupted time
</div>

---

## Lab 1: Explore a Real Open Source Project

**Objective:** Understand how real projects organize their development workflow.

**Time:** 10 minutes

### Instructions

1. **Visit the GitHub CLI repository:** [github.com/cli/cli](https://github.com/cli/cli)

2. **Explore the Issues tab:**
   - How many open issues are there?
   - What labels do they use?
   - Find an issue labeled `good first issue`

3. **Check out the Projects:**
   - Navigate to the **Projects** tab
   - How do they organize work?

4. **Review a Pull Request:**
   - Go to **Pull requests** → find a merged PR
   - Look at the description, review comments, and linked issues
   - How does the PR connect to issues?

5. **Check the Releases:**
   - Navigate to **Releases**
   - What information is included in release notes?

### Reflection Questions

- How does this project communicate what they're working on?
- What SDLC phases can you identify from their GitHub usage?
- What could your customers learn from this project's organization?

---

## Lab 2: Create a Planning Project

**Objective:** Set up a GitHub Project for tracking work items.

**Time:** 10 minutes

### Instructions

1. **Create a new repository:**
   - Go to [github.com/new](https://github.com/new)
   - Name it `sdlc-practice`
   - Check "Add a README file"
   - Click **Create repository**

2. **Create a new Project:**
   - In your repo, go to **Projects** tab
   - Click **New project**
   - Select **Board** template
   - Name it "Feature Development"

3. **Add columns for SDLC phases:**
   - Rename columns to: `Backlog`, `In Design`, `In Development`, `In Review`, `Done`

4. **Create Issues (user stories):**
   - Create 3 issues in your repo:
   
   **Issue 1: User Authentication**
   ```markdown
   ### User Story
   As a user, I want to log in with my email so I can access my account.
   
   ### Acceptance Criteria
   - [ ] Login form with email/password fields
   - [ ] Validation errors shown inline
   - [ ] Successful login redirects to dashboard
   ```
   
   **Issue 2: Password Reset**
   ```markdown
   ### User Story
   As a user, I want to reset my password via email so I can recover my account.
   
   ### Acceptance Criteria
   - [ ] "Forgot password" link on login page
   - [ ] Email sent within 2 minutes
   - [ ] Reset link expires after 24 hours
   ```
   
   **Issue 3: Two-Factor Authentication**
   ```markdown
   ### User Story
   As a user, I want to enable 2FA so my account is more secure.
   
   ### Acceptance Criteria
   - [ ] Setup wizard in account settings
   - [ ] Support for authenticator apps
   - [ ] Backup codes provided
   ```

5. **Add Issues to Project:**
   - Add all 3 issues to your Project
   - Drag them to appropriate columns

### ✅ Success Criteria

- [ ] Repository created with README
- [ ] Project board with 5 columns
- [ ] 3 issues created with user story format
- [ ] Issues visible in project board

---

## Lab 3: Write an Architecture Decision Record

**Objective:** Practice documenting technical decisions.

**Time:** 10 minutes

### Instructions

1. **In your `sdlc-practice` repo, create a new file:**
   - Click **Add file** → **Create new file**
   - Name: `docs/adr/ADR-001-authentication-method.md`

2. **Use this template:**

```markdown
# ADR 001: Authentication Method Selection

## Status
Proposed

## Context
We need to implement user authentication for our application. 
We're evaluating different approaches:

1. **Build custom auth** - Full control but high maintenance
2. **Use Auth0** - Managed service, fast to implement
3. **Use GitHub OAuth** - Simple, but limits to GitHub users

## Decision
[Your decision here - pick one option and explain why]

## Consequences
### Positive
- [List benefits]

### Negative  
- [List drawbacks]

## References
- [Link to relevant documentation]

```

3. **Fill in the template:**
   - Make a decision (doesn't matter which — this is practice!)
   - Document your reasoning
   - List consequences

4. **Commit the file:**
   - Write a meaningful commit message
   - Commit directly to `main`

### Reflection Questions

- Why is documenting decisions valuable?
- How would future team members benefit from this ADR?
- When would you revisit and update this ADR?

---

## Lab 4: Analyze Your Customer's Workflow (Thought Exercise)

**Objective:** Apply SDLC concepts to a customer scenario.

**Time:** 5 minutes

### Scenario

You're meeting with a new customer, **TechCorp**, who is evaluating GitHub Enterprise. From your discovery call, you learned:

- Team of 50 developers
- Currently using Jira for planning
- Using GitLab for source control
- No automated testing or deployment
- Release to production monthly (manual process)
- Recent security audit found several vulnerabilities

### Analysis Questions

Answer these in your notes:

1. **What SDLC phases are they weak in?**
   - Where do you see gaps?

2. **What GitHub features would help most?**
   - Map their pain points to GitHub capabilities

3. **What methodology are they likely using?**
   - What clues tell you this?

4. **What quick wins could you demonstrate?**
   - What would show immediate value?

5. **What expansion opportunities exist?**
   - What could they grow into over time?

### Sample Analysis

<details markdown="1">
<summary>Click to reveal suggested answers</summary>

**Gaps identified:**
- Testing phase (no automation)
- Deployment phase (manual)
- Security (reactively found issues)

**Priority GitHub features:**
1. GitHub Actions for CI/CD
2. GitHub Advanced Security
3. GitHub Projects (could consolidate from Jira)

**Methodology guess:** Likely Waterfall or Agile without DevOps practices (monthly releases, manual deployment)

**Quick wins:**
- Demo Actions running tests automatically
- Show secret scanning catching credentials
- Dependabot finding vulnerable dependencies

**Expansion opportunities:**
- Copilot for developer productivity
- GitHub Advanced Security full rollout
- Migrate from Jira to GitHub Projects

</details>

---

## 🎉 Lab Completion Checklist

- [ ] Lab 1: Explored GitHub CLI repository
- [ ] Lab 2: Created planning project with issues
- [ ] Lab 3: Wrote an Architecture Decision Record
- [ ] Lab 4: Analyzed customer workflow scenario

<div class="callout callout-tip">
<div class="callout-title">📸 Capture Your Work</div>

Take screenshots of your completed labs! They're useful for:
- Reviewing later
- Showing during customer conversations
- Building your GitHub portfolio
</div>
