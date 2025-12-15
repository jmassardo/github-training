---
layout: training-module
title: "Guided Walkthrough"
permalink: /beginner/module-3/walkthrough/
module_number: 3
module_title: "GitHub Platform Essentials"
section_number: 3
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
  url: /beginner/module-3/concepts/
  title: "Core Concepts"
next_section:
  url: /beginner/module-3/labs/
  title: "Hands-On Labs"
---

## Exploring the GitHub Interface

Let's take a tour through a real repository to understand the GitHub interface.

<div class="callout callout-info">
<div class="callout-title">🔗 Follow Along</div>

Open [github.com/github/docs](https://github.com/github/docs) in another tab to follow along. This is GitHub's own documentation repository.
</div>

---

## The Repository Homepage

When you open a repository, you see the **Code** tab by default:

### Top Navigation Bar

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph NAV["🏠 github / docs"]
        direction LR
        ACTIONS["⭐ Star &nbsp; 🍴 Fork"]
        TABS["Code | Issues | Pull requests | Actions | Projects ..."]
    end
</div>
</div>

### Key Elements

1. **Owner/Repo Name** — `github/docs`
2. **Star Button** — Bookmark the repo
3. **Fork Button** — Copy to your account
4. **Tab Navigation** — Different sections of the repo


## Tab 1: Code

The Code tab shows:

### Branch Selector
- Click to switch branches
- See all branches
- Create new branches

### File Browser
- Navigate folders and files
- Click any file to view contents
- View file history with blame

### Sidebar Information
- **About** — Repository description
- **Releases** — Published versions
- **Contributors** — Who has contributed
- **Languages** — Code breakdown

### README
- Displayed below file list
- Project introduction and documentation
- Rendered Markdown

---

## Tab 2: Issues

The Issues tab is your work tracker:

### Issue List

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph ISSUES["🔍 is:open is:issue"]
        I1["☑ #4521 Fix broken link in quickstart<br/>🏷️ bug • 2h ago"]
        I2["☐ #4519 Add dark mode support<br/>🏷️ feat • 1d ago"]
        I3["☐ #4518 Typo in authentication docs<br/>🏷️ docs • 2d ago"]
    end
</div>
</div>

### Filtering Issues

Use the filter bar or search queries:

| Filter | What It Shows |
|--------|---------------|
| `is:open` | Open issues only |
| `is:closed` | Closed issues |
| `label:bug` | Issues with bug label |
| `assignee:@me` | Assigned to you |
| `author:username` | Created by user |
| `milestone:"v2.0"` | In specific milestone |

### Creating an Issue

1. Click **New Issue**
2. Choose template (if available)
3. Fill in title and description
4. Add labels, assignees, milestone
5. Submit


## Tab 3: Pull Requests

The Pull Requests tab shows code changes:

### PR List

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph PRS["🔍 is:open is:pr"]
        PR1["✅ #4520 Update dependencies<br/>@dev1 • 1h ago"]
        PR2["🔄 #4517 Add new API endpoint<br/>@dev2 • 3h ago"]
        PR3["❌ #4515 Fix login flow<br/>@dev3 • 1d ago"]
    end
    LEGEND["✅ Checks passed | 🔄 Running | ❌ Failed"]
</div>
</div>

### Inside a Pull Request

| Tab | Shows |
|-----|-------|
| **Conversation** | Description, comments, review summary |
| **Commits** | Individual commits in the PR |
| **Checks** | CI/CD status (Actions results) |
| **Files changed** | Diff view of all changes |

### The Files Changed View

```diff
  function login(username, password) {
-   return oldAuthMethod(username, password);
+   return newSecureAuth(username, password);
  }

```

- **Green (+)** — Lines added
- **Red (-)** — Lines removed
- **Comment** — Click line number to comment

---

## Tab 4: Actions

GitHub Actions shows CI/CD workflows:

### Workflow List

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph WORKFLOWS["📋 All workflows"]
        W1["✅ CI Tests<br/>Main branch • 2 min ago"]
        W2["✅ Deploy to Production<br/>v2.0.1 • 1 hour ago"]
        W3["❌ Security Scan<br/>PR #4520 • 30 min ago"]
    end
</div>
</div>

### Workflow Run Details

Click a workflow to see:

- Job steps and duration
- Logs for each step
- Artifacts produced
- Re-run options


## Tab 5: Projects

Projects tab shows planning boards:

### Project Views

**Board View:**

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    subgraph TODO["📋 To Do"]
        T1["#4521"]
        T2["#4518"]
    end
    subgraph PROGRESS["🔄 In Progress"]
        P1["#4519"]
        P2["#4517"]
    end
    subgraph DONE["✅ Done"]
        D1["#4510"]
        D2["#4508"]
        D3["#4505"]
    end
    TODO --> PROGRESS --> DONE
</div>
</div>

**Table View:**

```

| Title           | Status      | Assignee | Priority |
|-----------------|-------------|----------|----------|
| Fix login bug   | In Progress | @dev1    | High     |
| Add dark mode   | To Do       | @dev2    | Medium   |

```

---

## Tab 6: Wiki

The Wiki tab provides documentation space:

- Markdown-based pages
- Sidebar navigation
- Version history
- Anyone with write access can edit


## Tab 7: Security

The Security tab shows:

- **Security Overview** — Vulnerability summary
- **Dependabot Alerts** — Vulnerable dependencies
- **Code Scanning** — Security issues in code
- **Secret Scanning** — Exposed credentials

---

## Repository Settings

Click the ⚙️ **Settings** tab to configure:

### General
- Repository name
- Default branch
- Features (Issues, Wiki, Projects, Discussions)

### Collaborators & Teams
- Add collaborators
- Set permission levels (Read, Write, Admin)

### Branches
- Branch protection rules
- Require reviews before merge
- Require status checks

### Actions
- Workflow permissions
- Runner settings


## User Profile Tour

Navigate to any user profile (click a username):

### Profile Sections

| Section | Shows |
|---------|-------|
| **Overview** | Bio, pinned repos, contribution graph |
| **Repositories** | All public repos |
| **Projects** | User's projects |
| **Stars** | Repos they've starred |
| **Followers/Following** | Social connections |

### Contribution Graph

The green squares show daily activity:

- Darker green = more contributions
- Hover for specific counts
- Great for evaluating developer activity

---

## Organization Tour

Organizations have additional features:

### Organization Tabs

| Tab | Purpose |
|-----|---------|
| **Overview** | Org profile and pinned repos |
| **Repositories** | All org repos |
| **Packages** | Published packages |
| **Teams** | Team management |
| **People** | Member list |
| **Settings** | Org configuration |

### Teams

Teams organize members with shared permissions:

- Nested teams (parent/child)
- Team discussions
- Team-level project access


## Coming Up Next

Now that you've toured the interface, let's practice using it in hands-on labs.
