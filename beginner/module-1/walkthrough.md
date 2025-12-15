---
layout: training-module
title: "Guided Walkthrough"
permalink: /beginner/module-1/walkthrough/
module_number: 1
module_title: "Understanding Software Development & SDLC"
section_number: 3
total_sections: 7
phase: beginner
estimated_time: "25 min"
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
  url: /beginner/module-1/concepts/
  title: "Core Concepts"
next_section:
  url: /beginner/module-1/labs/
  title: "Hands-On Labs"
---

## Walkthrough: Following a Feature from Idea to Production

Let's trace how a real feature moves through the SDLC using GitHub. We'll follow the journey of adding a "dark mode" feature to a web application.

---

## Step 1: Planning in GitHub Projects

### Creating the Epic

1. Navigate to your repository → **Projects** tab
2. Create a new project or open an existing one
3. Add a new item (Issue) with the Epic template:

```markdown
## Epic: Dark Mode Support

### Description
Add dark mode theme option to improve user experience and 
reduce eye strain for users working in low-light environments.

### User Stories
- [ ] As a user, I want to toggle dark mode from settings
- [ ] As a user, I want the app to remember my preference
- [ ] As a user, I want automatic dark mode based on system settings

### Success Metrics
- 30% of users enable dark mode within first month
- Positive feedback in app store reviews

```

### Breaking Down into User Stories

Create separate Issues for each user story:

**Issue #42: Dark mode toggle**

```markdown
### User Story
As a user, I want to toggle dark mode from settings 
so that I can reduce eye strain.

### Acceptance Criteria
- [ ] Toggle switch in Settings page
- [ ] Immediate visual feedback when toggled
- [ ] All components respect theme choice

### Technical Notes
- Use CSS custom properties for theming
- Consider accessibility (WCAG contrast ratios)

```

---

## Step 2: Design Phase

### Creating an Architecture Decision Record

In the repository, create `.github/docs/adr/`:

**ADR-001-dark-mode-implementation.md**

```markdown
# ADR 001: Dark Mode Implementation Approach

## Status
Accepted

## Context
We need to implement dark mode across our React application.
Options considered:

1. CSS-in-JS with theme context
2. CSS custom properties (variables)
3. Separate dark mode stylesheet

## Decision
Use CSS custom properties with a data attribute on <html>.

## Rationale
- No JavaScript runtime overhead
- Works with any CSS methodology
- Easy to override per-component
- Better performance than re-rendering

## Consequences
- Need to audit all hardcoded colors
- Will require browser fallback for IE11 (if supported)

```

---

## Step 3: Development Phase

### Creating a Feature Branch

```bash
# Create and switch to feature branch
git checkout -b feature/dark-mode

# Make changes and commit
git add .
git commit -m "feat: add dark mode CSS variables"

git commit -m "feat: add theme toggle component"

git commit -m "feat: persist theme preference"

```

### Opening a Pull Request

**Pull Request Title:** `feat: Add dark mode support (#42)`

**PR Description:**

```markdown
## Summary
Implements dark mode toggle as described in #42.

## Changes
- Added CSS custom properties for theming
- Created ThemeToggle component
- Added localStorage persistence
- Updated all color references

## Testing
- [x] Manual testing in Chrome, Firefox, Safari
- [x] Unit tests for ThemeToggle component
- [x] Verified WCAG AA contrast ratios

## Screenshots

| Light Mode | Dark Mode |
|------------|-----------|
| ![light](url) | ![dark](url) |

Closes #42

```

---

## Step 4: Code Review Process

### Reviewer Feedback

A teammate reviews the PR and leaves comments:

```markdown
**Comment on line 45 of theme.css:**
> Consider using `prefers-color-scheme` media query for 
> automatic detection based on OS settings.

**Comment on ThemeToggle.jsx:**
> Nice implementation! One suggestion: should we add a 
> "system default" option in addition to light/dark?

```

### Addressing Feedback

Developer makes updates based on review:

```bash
git commit -m "feat: add system preference detection"
git push

```

PR is approved ✅

---

## Step 5: Automated Testing (CI)

### GitHub Actions Workflow

When the PR is opened, Actions automatically runs:

```yaml
name: CI
on: [pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install dependencies
        run: npm ci
      
      - name: Run tests
        run: npm test
      
      - name: Run linter
        run: npm run lint
      
      - name: Build
        run: npm run build

```

**Status checks appear on the PR:**

- ✅ Tests passed (42 tests)
- ✅ Lint passed
- ✅ Build successful
- ✅ Code review approved

---

## Step 6: Merging and Deployment

### Merge the Pull Request

1. All checks pass ✅
2. Required reviews approved ✅
3. Click **"Squash and merge"**
4. PR merged to `main`

### Automated Deployment

A deployment workflow triggers:

```yaml
name: Deploy
on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v4
      
      - name: Build
        run: npm run build
      
      - name: Deploy to production
        run: ./deploy.sh

```

---

## Step 7: Release and Communication

### Creating a Release

1. Go to **Releases** → **Draft a new release**
2. Create tag: `v2.1.0`
3. Write release notes:

```markdown
## What's New in v2.1.0

### ✨ New Features
- **Dark Mode** - Toggle between light and dark themes (#42)
  - Manual toggle in Settings
  - Automatic detection based on system preference
  - Preference saved locally

### 🐛 Bug Fixes
- Fixed navigation menu alignment on mobile

### Full Changelog
https://github.com/org/repo/compare/v2.0.0...v2.1.0

```

---

## The Complete Journey

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    subgraph PLAN["📋 PLANNING"]
        I["Issue #42"]
    end
    
    subgraph DESIGN["🎨 DESIGN"]
        ADR["ADR Created"]
    end
    
    subgraph DEV["💻 DEVELOP"]
        BR["Branch + PRs"]
    end
    
    subgraph TEST["🧪 TEST"]
        CI["Actions CI"]
    end
    
    subgraph DEPLOY["🚀 DEPLOY"]
        REL["Merge & Release"]
    end
    
    I --> ADR --> BR --> CI --> REL
    BR <-.->|"Review"| CI
</div>
</div>

---

## Key Takeaways

1. **Everything is connected** — Issues link to PRs, PRs link to deployments
2. **Automation reduces friction** — Tests run automatically, deployments are hands-off
3. **Visibility is built-in** — Anyone can see status, history, and decisions
4. **Collaboration is first-class** — Reviews, discussions, and feedback are native

<div class="callout callout-tip">
<div class="callout-title">💡 CSM Tip</div>

When talking with customers, walk them through this flow with their own examples. Seeing how GitHub connects their workflow phases is often an "aha moment" that drives adoption.
</div>
