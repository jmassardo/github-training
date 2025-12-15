---
layout: training-module
title: "Core Concepts"
permalink: /beginner/module-3/concepts/
module_number: 3
module_title: "GitHub Platform Essentials"
section_number: 2
total_sections: 7
phase: beginner
estimated_time: "20 min"
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
  url: /beginner/module-3/overview/
  title: "Context & Overview"
next_section:
  url: /beginner/module-3/walkthrough/
  title: "Guided Walkthrough"
---

## Repositories: Your Project's Home

A **repository** (repo) is a project container that includes:

- All your code and files
- Complete Git history
- Issues, PRs, and discussions
- Wiki and documentation
- Settings and access controls

### Repository Visibility

| Visibility | Who Can See | Use Case |
|------------|-------------|----------|
| **Public** | Anyone | Open source projects |
| **Private** | Invited collaborators | Proprietary code |
| **Internal** | Anyone in organization | Shared company code (Enterprise) |

### Key Repository Components

```
my-repository/
├── 📁 Code (files and folders)
├── 📋 Issues (work tracking)
├── 🔀 Pull Requests (code changes)
├── ⚡ Actions (automation)
├── 📊 Projects (planning boards)
├── 📖 Wiki (documentation)
├── 💬 Discussions (community)
├── 🔒 Security (alerts, scanning)
└── ⚙️ Settings (configuration)
```

---

## Issues: Tracking Work

**Issues** are GitHub's universal work tracker. Use them for:

- 🐛 Bug reports
- ✨ Feature requests
- 📋 Tasks and to-dos
- 💬 Questions and discussions
- 📝 Documentation needs

### Anatomy of an Issue

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph ISSUE["🐛 Issue #42: Login button not working on mobile"]
        direction TB
        META["<b>Labels:</b> bug, high-priority, mobile<br/><b>Assignee:</b> @developer<br/><b>Milestone:</b> v2.0 Release<br/><b>Project:</b> Q4 Sprint 3"]
        DESC["<b>Description</b><br/>Users report the login button is unresponsive...<br/><br/><b>Steps to Reproduce</b><br/>1. Open site on iPhone Safari<br/>2. Tap login button<br/>3. Nothing happens"]
        ENGAGE["💬 Comments (5) &nbsp;&nbsp; 🔗 Linked PR: #45"]
    end
    META --> DESC --> ENGAGE
</div>
</div>

### Issue Templates

Organizations can create templates for consistent issue reporting:

- **Bug Report** — Steps to reproduce, expected vs actual behavior
- **Feature Request** — Problem statement, proposed solution
- **Question** — Context, what you've tried


## Labels: Organizing Issues

**Labels** categorize issues with colored tags:

### Common Label Patterns

| Label | Purpose |
|-------|---------|
| `bug` | Something isn't working |
| `enhancement` | New feature request |
| `documentation` | Documentation improvements |
| `good first issue` | Easy tasks for new contributors |
| `help wanted` | Needs community assistance |
| `priority: high` | Urgent attention needed |
| `status: in progress` | Currently being worked on |

### Label Strategy Tips

- Use consistent naming conventions
- Limit to 3-4 labels per issue
- Color-code by category (red for bugs, blue for features)
- Document label meanings in contributing guide

---

## Pull Requests: The Heart of Collaboration

A **Pull Request** (PR) is how code changes get reviewed and merged.

### Pull Request Workflow

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    B["1️⃣ Create branch<br/>feature/add-login"] --> C["2️⃣ Make changes<br/>Commits on branch"]
    C --> PR["3️⃣ Open PR<br/>Request merge"]
    PR --> R["4️⃣ Review<br/>Team reviews"]
    R --> F["5️⃣ Address feedback<br/>Make changes"]
    F --> A["6️⃣ Approve<br/>Reviewer approves"]
    A --> M["7️⃣ Merge<br/>Changes to main"]
    M --> D["8️⃣ Delete branch<br/>Clean up"]
</div>
</div>

### Anatomy of a Pull Request

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph PR["🔀 PR: Add user authentication system"]
        direction TB
        BRANCH["feature/user-auth → main"]
        DESC["<b>📝 Description</b><br/>This PR adds OAuth2 authentication...<br/>Closes #42"]
        CHECKS["<b>✅ Checks passed (3/3)</b><br/>• Build succeeded<br/>• Tests passed (142 tests)<br/>• Security scan clean"]
        META["👥 Reviewers: @senior-dev ✓<br/>🏷️ Labels: enhancement, authentication"]
    end
    BRANCH --> DESC --> CHECKS --> META
</div>
</div>

### Code Review

Reviewers can:

- **Comment** — Ask questions, suggest improvements
- **Approve** — Code looks good to merge
- **Request Changes** — Blocking issues must be addressed


## GitHub Projects: Planning Work

**Projects** provide Kanban-style boards and tables for planning.

### Project Views

| View Type | Best For |
|-----------|----------|
| **Board** | Kanban workflow (To Do → In Progress → Done) |
| **Table** | Spreadsheet-style data view |
| **Roadmap** | Timeline planning |

### Project Features

- **Custom fields** — Priority, size, sprint
- **Automation** — Move items when status changes
- **Filtering** — View specific items
- **Grouping** — Organize by assignee, label, status
- **Insights** — Charts and metrics

---

## Milestones: Tracking Releases

**Milestones** group issues and PRs toward a goal:

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph MILESTONE["🎯 Milestone: v2.0 Release"]
        DUE["<b>Due:</b> December 31, 2025"]
        PROGRESS["<b>Progress:</b> ████████░░ 80%"]
        STATS["<b>Open:</b> 4 issues, 2 PRs<br/><b>Closed:</b> 16 issues, 8 PRs"]
    end
</div>
</div>


## GitHub Discussions

**Discussions** enable community conversation outside of issues:

### Discussion Categories

| Category | Use Case |
|----------|----------|
| **Announcements** | Project news (maintainer posts) |
| **General** | Open conversation |
| **Ideas** | Feature brainstorming |
| **Q&A** | Questions with accepted answers |
| **Show and Tell** | Share what you've built |

### Discussions vs Issues

| Discussions | Issues |
|-------------|--------|
| Open-ended conversation | Trackable work items |
| Community engagement | Bug reports, features |
| Q&A format available | Assignable to developers |
| No close date | Can be closed |

---

## Repository Settings

Key settings CSMs should know:

### Features Toggle
- Issues on/off
- Projects on/off
- Wiki on/off
- Discussions on/off

### Access & Security
- Collaborator permissions
- Branch protection rules
- Deploy keys
- Webhooks

### Merge Settings
- Allow merge commits
- Allow squash merging
- Allow rebase merging
- Auto-delete head branches

---

## GitHub Mobile

**GitHub Mobile** brings the GitHub experience to iOS and Android devices, enabling developers to stay connected to their work from anywhere.

### Key Mobile Features

| Feature | Capability |
|---------|------------|
| **Notifications** | Triage, filter, and respond to notifications |
| **Issues** | Create, read, comment, and close issues |
| **Pull Requests** | Review code, approve PRs, merge changes |
| **Discussions** | Participate in community conversations |
| **Search** | Find repositories, issues, users, and code |
| **Profile** | View activity, contributions, and followers |

### Mobile-Specific Features

- **Push notifications** - Real-time alerts for mentions, reviews, CI status
- **Swipe gestures** - Quick actions on notifications and issues
- **Dark mode** - Follows system theme preference
- **Face ID / Touch ID** - Secure authentication
- **Home screen widgets** - Contribution graph, notifications count

### Best Use Cases for Mobile

✅ **Great for:**
- Triaging notifications during commute
- Quick code review approvals
- Responding to urgent issues
- Checking CI/CD status
- Team communication on the go

⚠️ **Better on desktop:**
- Writing significant code
- Complex merge conflict resolution
- Detailed code review with line comments
- Repository administration

### CSM Talking Point

> "GitHub Mobile turns waiting time into productive time. Your developers can triage notifications, approve reviews, and stay connected without opening their laptop."

---

## Visual Summary

<div class="callout callout-info">
<div class="callout-title">🎨 Key Concepts at a Glance</div>

**Repository**: Project container with code, history, and collaboration tools

**Issue**: Work item (bug, feature, task) with labels and assignees

**Pull Request**: Code change proposal with review workflow

**Project**: Kanban board or table for planning work

**Milestone**: Groups issues/PRs toward a deadline

**Discussion**: Community conversation outside issues

**Codespaces**: Cloud development environment in your browser

**GitHub Mobile**: iOS/Android app for on-the-go access
</div>

---

## Coming Up Next

Now let's take a guided tour of the GitHub interface to see these features in action.
