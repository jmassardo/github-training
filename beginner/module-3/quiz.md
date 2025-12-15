---
layout: training-module
title: "Knowledge Check"
permalink: /beginner/module-3/quiz/
module_number: 3
module_title: "GitHub Platform Essentials"
section_number: 5
total_sections: 7
phase: beginner
estimated_time: "10 min"
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
  url: /beginner/module-3/labs/
  title: "Hands-On Labs"
next_section:
  url: /beginner/module-3/resources/
  title: "Resources"
---

## Test Your Knowledge

Check your understanding of GitHub platform features.

---

### Question 1: Repository Visibility

**A customer asks: "We want code visible to all employees but not the public." What visibility should they use?**

<details markdown="1">
<summary>Show Answer</summary>

**Internal** visibility (Enterprise feature)

- **Public** — Anyone can see
- **Private** — Only explicitly invited collaborators
- **Internal** — Anyone in the organization (Enterprise Cloud/Server only)

If they don't have Enterprise, they'd need to use Private repos and manage collaborator access manually.
</details>


### Question 2: Issues vs Discussions

**When should a customer use Discussions instead of Issues?**

<details markdown="1">
<summary>Show Answer</summary>

**Discussions** are better for:
- Open-ended questions
- Community conversation
- Ideas and brainstorming
- Q&A (with accepted answers)
- General announcements

**Issues** are better for:
- Actionable work items
- Bug reports
- Feature requests that will be implemented
- Tasks with assignees and due dates

**Rule of thumb:** If it needs to be tracked and completed, use an Issue. If it's conversation, use Discussions.
</details>

---

### Question 3: Pull Request Workflow

**What happens when you include "Closes #42" in a PR description?**

<details markdown="1">
<summary>Show Answer</summary>

When the PR is merged, Issue #42 will automatically close.

**Linking keywords:**

- `Closes #42`
- `Fixes #42`
- `Resolves #42`

These only work when the PR is merged to the default branch. You can link multiple issues: `Closes #42, fixes #43`
</details>


### Question 4: Project Features

**Match the GitHub Project view to its best use case:**

| View | Use Case |
|------|----------|
| 1. Board | A. Quarterly planning with dates |
| 2. Table | B. Sprint workflow visualization |
| 3. Roadmap | C. Data analysis with custom fields |

<details markdown="1">
<summary>Show Answer</summary>

1. **Board** → **B. Sprint workflow visualization** (Kanban columns)
2. **Table** → **C. Data analysis with custom fields** (spreadsheet-style)
3. **Roadmap** → **A. Quarterly planning with dates** (timeline view)
</details>

---

### Question 5: Repository Tabs

**Which tab would you use to see if automated tests passed on a PR?**

<details markdown="1">
<summary>Show Answer</summary>

**Actions** tab (or the **Checks** section within a PR)

- **Actions** shows all workflow runs
- Within a PR, the **Checks** tab shows status checks for that specific PR
- Green checkmark = passed, Red X = failed
</details>


### Question 6: Labels Strategy

**A customer's repository has 47 labels and no one uses them. What would you recommend?**

<details markdown="1">
<summary>Show Answer</summary>

**Recommendations:**

1. **Audit and consolidate** — Remove unused labels, merge similar ones
2. **Create clear categories** — Type (bug, feature), Priority (high, low), Status (in progress)
3. **Document meanings** — Add descriptions to each label
4. **Enforce usage** — Require labels via issue templates
5. **Start simple** — 10-15 well-defined labels is often enough

**Label best practices:**

- Use consistent naming (prefix: `type:bug`, `priority:high`)
- Color-code by category
- Keep to 3-4 labels per issue
- Review and clean up quarterly
</details>

---

### Question 7: Milestones

**What's the difference between Milestones and Projects?**

<details markdown="1">
<summary>Show Answer</summary>

**Milestones:**

- Group issues toward a deadline
- Show progress percentage
- Simple: just a name, date, description
- Good for releases (v1.0, v2.0)

**Projects:**

- Flexible views (board, table, roadmap)
- Custom fields (priority, size, sprint)
- Cross-repository support
- Automation and filtering
- Good for sprint planning and workflows

**Use together:** A Project can show items from multiple milestones, or you can filter a Project by milestone.
</details>


### Question 8: Enterprise Features

**Which features require GitHub Enterprise (Cloud or Server)?**

<details markdown="1">
<summary>Show Answer</summary>

**Enterprise-only features:**

- Internal repository visibility
- SAML/SSO authentication
- Advanced audit log
- GitHub Advanced Security (GHAS)
- Enterprise Managed Users (EMU)
- Custom repository roles
- IP allow lists
- Multiple organizations under one enterprise

**Available on Team/Free:**

- Issues, PRs, Actions
- Projects
- Discussions
- Branch protection (basic)
- Dependabot
</details>

---

### Question 9: Code Review

**A PR has 3 reviewers: 2 approved, 1 requested changes. Can it be merged?**

<details markdown="1">
<summary>Show Answer</summary>

**It depends on branch protection rules:**

- **No rules:** Yes, anyone with write access can merge
- **Require approvals (2):** Yes, if only approval count matters
- **Dismiss stale reviews:** The "request changes" review would block merge

**Best practice:** Configure branch protection to require:
- At least 1-2 approving reviews
- Resolution of all conversations
- Dismissal of stale reviews after new commits

The "request changes" usually blocks merge until that reviewer approves or is dismissed.
</details>


### Question 10: Customer Scenario

**A customer says: "Our developers ignore issues and just fix things without tracking." What GitHub features could help?**

<details markdown="1">
<summary>Show Answer</summary>

**Solutions:**

1. **Branch protection** — Require PRs (no direct pushes to main)
2. **PR templates** — Include "Related Issue: #" field
3. **CODEOWNERS** — Require review from team leads who can check for linked issues
4. **Projects automation** — Auto-add PRs to project for visibility
5. **Required status checks** — Custom check that verifies issue linkage

**Cultural solutions:**

- Educate on why tracking matters
- Make issue creation fast (good templates)
- Celebrate completed issues
- Use issues in standups and planning
</details>

---

## 🎯 Score Yourself

| Score | Assessment |
|-------|------------|
| 10/10 | 🌟 Excellent! You know the GitHub platform inside out |
| 8-9 | 👍 Great job! Review the concepts you missed |
| 6-7 | 📚 Good progress — revisit the walkthrough section |
| 0-5 | 🔄 Consider re-reading the module before moving on |
