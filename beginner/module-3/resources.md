---
layout: training-module
title: "Resources & Further Learning"
permalink: /beginner/module-3/resources/
module_number: 3
module_title: "GitHub Platform Essentials"
section_number: 6
total_sections: 7
phase: beginner
estimated_time: "5 min"
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
  url: /beginner/module-3/quiz/
  title: "Knowledge Check"
next_section:
  url: /beginner/module-3/scenarios/
  title: "CSM Scenarios"
---

## Official GitHub Documentation

### 📚 Core Documentation

- **[GitHub Docs](https://docs.github.com/)** — Comprehensive documentation
- **[Getting Started](https://docs.github.com/en/get-started)** — Beginner guides
- **[GitHub Skills](https://skills.github.com/)** — Interactive learning

### 📋 Feature-Specific Guides

| Feature | Documentation Link |
|---------|-------------------|
| Issues | [About Issues](https://docs.github.com/en/issues) |
| Projects | [Planning with Projects](https://docs.github.com/en/issues/planning-and-tracking-with-projects) |
| Pull Requests | [About PRs](https://docs.github.com/en/pull-requests) |
| Code Review | [Reviewing Changes](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests) |
| Discussions | [About Discussions](https://docs.github.com/en/discussions) |
| Wiki | [About Wikis](https://docs.github.com/en/communities/documenting-your-project-with-wikis) |

---

## GitHub Product Pages

### For Customer Conversations

- **[GitHub Features](https://github.com/features)** — Feature overview
- **[GitHub Enterprise](https://github.com/enterprise)** — Enterprise offerings
- **[GitHub Pricing](https://github.com/pricing)** — Plan comparison
- **[Customer Stories](https://github.com/customer-stories)** — Success stories

### For ROI Discussions

- **[GitHub ROI Calculator](https://resources.github.com/roi/)** — Value estimation
- **[Total Economic Impact](https://resources.github.com/downloads/TEI-of-GitHub-Enterprise-Cloud-and-Advanced-Security.pdf)** — Forrester study


## Quick Reference: Issue Search Syntax

```
# Basic filters
is:open                    # Open issues only
is:closed                  # Closed issues
is:issue                   # Issues (not PRs)
is:pr                      # PRs only

# By attribute
author:username            # Created by user
assignee:username          # Assigned to user
label:bug                  # Has label
milestone:"v2.0"           # In milestone
project:"Sprint 1"         # In project

# By date
created:>2025-01-01        # Created after date
updated:<2025-01-01        # Updated before date
closed:2025-01-01          # Closed on date

# By state
review:required            # PR needs review
review:approved            # PR approved
draft:true                 # Draft PRs
merged:true                # Merged PRs

# Combining
is:open label:bug assignee:@me

```

---

## Quick Reference: Markdown for Issues

```markdown
# Headers
## Subheader
### Smaller header

**Bold text**
*Italic text*
~~Strikethrough~~

- Bullet list
- Item 2
  - Nested item

1. Numbered list
2. Item 2

[Link text](https://url.com)
![Image alt](image-url.png)

`inline code`

```language

code block

```

> Quote block

| Column 1 | Column 2 |
|----------|----------|
| Data     | Data     |

- [ ] Checkbox (unchecked)
- [x] Checkbox (checked)

@username mentions
#123 issue reference

```


## Quick Reference: PR Templates

Create `.github/PULL_REQUEST_TEMPLATE.md`:

```markdown
## Summary
Brief description of changes.

## Changes
- Change 1
- Change 2

## Related Issues
Closes #

## Testing
How was this tested?

## Checklist
- [ ] Tests pass
- [ ] Documentation updated
- [ ] No breaking changes

```

---

## Quick Reference: Issue Templates

Create `.github/ISSUE_TEMPLATE/bug_report.md`:

```markdown
name: Bug Report
about: Report a bug
title: '[BUG] '
labels: bug
assignees: ''
---

## Description
Clear description of the bug.

## Steps to Reproduce
1. Go to '...'
2. Click on '...'
3. See error

## Expected Behavior
What should happen.

## Actual Behavior
What actually happens.

## Screenshots
If applicable.

## Environment
- OS: [e.g., Windows 11]
- Browser: [e.g., Chrome 120]
- Version: [e.g., v2.0.1]

```


## Glossary

| Term | Definition |
|------|------------|
| **Repository** | Project container with code, history, and collaboration tools |
| **Issue** | Work item (bug, feature, task) |
| **Pull Request** | Code change proposal with review workflow |
| **Branch** | Independent line of development |
| **Merge** | Combining changes from branches |
| **Fork** | Personal copy of another's repository |
| **Clone** | Local copy of a repository |
| **Collaborator** | User with access to a private repository |
| **Organization** | Shared account for team collaboration |
| **Team** | Group of org members with shared permissions |
| **Milestone** | Groups issues toward a deadline |
| **Project** | Planning board/table for tracking work |
| **Label** | Categorization tag for issues |
| **CODEOWNERS** | File defining who reviews which code |
| **Branch Protection** | Rules preventing direct pushes |

---

<div class="callout callout-tip">
<div class="callout-title">📌 Bookmark This Page</div>

Keep these quick references handy for customer conversations and demo preparation!
</div>
