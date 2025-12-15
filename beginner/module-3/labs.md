---
layout: training-module
title: "Hands-On Labs"
permalink: /beginner/module-3/labs/
module_number: 3
module_title: "GitHub Platform Essentials"
section_number: 4
total_sections: 7
phase: beginner
estimated_time: "25 min"
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
  url: /beginner/module-3/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /beginner/module-3/quiz/
  title: "Knowledge Check"
---

## Lab Overview

Practice using GitHub's platform features with these hands-on exercises.

<div class="callout callout-info">
<div class="callout-title">📋 Prerequisites</div>

- GitHub account
- Repository from Module 2 labs (or create a new one)
</div>

---

## Lab 1: Create and Organize Issues

**Objective:** Practice creating issues with proper formatting and labels.

**Time:** 10 minutes

### Instructions

1. **Go to your practice repository** (or create one at github.com/new)

2. **Create three issues:**

   **Issue 1 — Bug Report:**
   - Click **Issues** → **New Issue**
   - Title: `[BUG] Homepage loads slowly on mobile`
   - Body:
   ```markdown
   ## Description
   The homepage takes over 5 seconds to load on mobile devices.
   
   ## Steps to Reproduce
   1. Open site on iPhone
   2. Navigate to homepage
   3. Measure load time
   
   ## Expected Behavior
   Page should load in under 2 seconds.
   
   ## Actual Behavior
   Page takes 5+ seconds to load.
   
   ## Environment
   - Device: iPhone 13
   - Browser: Safari
   - OS: iOS 16
   ```

   **Issue 2 — Feature Request:**
   - Title: `[FEATURE] Add dark mode support`
   - Body:
   ```markdown
   ## Problem Statement
   Users working at night report eye strain from the bright interface.
   
   ## Proposed Solution
   Add a dark mode toggle in user settings.
   
   ## Alternatives Considered
   - Browser extension for dark mode
   - CSS media query for system preference
   
   ## Additional Context
   Many users have requested this feature on social media.
   ```

   **Issue 3 — Task:**
   - Title: `[TASK] Update dependencies`
   - Body:
   ```markdown
   ## Description
   Run npm update and test all features.
   
   ## Checklist
   - [ ] Run `npm update`
   - [ ] Run test suite
   - [ ] Verify all features work
   - [ ] Update changelog
   ```

3. **Create labels (if they don't exist):**
   - Go to **Issues** → **Labels** → **New label**
   - Create: `bug` (red), `enhancement` (blue), `task` (yellow)

4. **Apply labels to your issues:**
   - Bug report → `bug`
   - Feature request → `enhancement`
   - Task → `task`

### ✅ Success Criteria
- [ ] Three issues created with proper formatting
- [ ] Labels created and applied
- [ ] Issues visible in Issues list


## Lab 2: Set Up a Project Board

**Objective:** Create a project to track your issues.

**Time:** 8 minutes

### Instructions

1. **Create a new Project:**
   - Go to your repository
   - Click **Projects** tab → **New project**
   - Select **Board** template
   - Name: "Sprint 1"

2. **Configure columns:**
   - Keep default columns or rename:
     - `To Do`
     - `In Progress`
     - `Done`

3. **Add your issues to the project:**
   - Click **+ Add item** in "To Do" column
   - Search for your issues by number or title
   - Add all three issues

4. **Add custom fields:**
   - Click **⚙️** → **Settings** → **Fields**
   - Add a new field: "Priority" (Single select)
   - Options: `High`, `Medium`, `Low`

5. **Set priorities:**
   - Bug: High
   - Feature: Medium
   - Task: Low

6. **Practice moving items:**
   - Drag the Bug to "In Progress"
   - This simulates starting work

### ✅ Success Criteria
- [ ] Project created with three columns
- [ ] All issues added to project
- [ ] Custom Priority field added
- [ ] Priorities assigned

---

## Lab 3: Create Your First Pull Request

**Objective:** Practice the complete PR workflow.

**Time:** 10 minutes

### Instructions

1. **Clone your repository locally** (if not already):
   ```bash
   git clone https://github.com/YOUR-USERNAME/YOUR-REPO.git
   cd YOUR-REPO
   ```

2. **Create a feature branch:**
   ```bash
   git checkout -b feature/add-contributing-guide
   ```

3. **Create a CONTRIBUTING.md file:**
   ```bash
   cat > CONTRIBUTING.md << 'EOF'
   # Contributing to This Project
   
   Thank you for your interest in contributing!
   
   ## How to Contribute
   
   1. Fork the repository
   2. Create a feature branch (`git checkout -b feature/amazing-feature`)
   3. Commit your changes (`git commit -m 'Add amazing feature'`)
   4. Push to the branch (`git push origin feature/amazing-feature`)
   5. Open a Pull Request
   
   ## Code Style
   
   - Use clear, descriptive variable names
   - Comment complex logic
   - Write tests for new features
   
   ## Reporting Bugs
   
   Use the bug report template when creating issues.
   
   ## Questions?
   
   Open a Discussion or reach out to maintainers.
   EOF
   ```

4. **Commit and push:**
   ```bash
   git add CONTRIBUTING.md
   git commit -m "Add contributing guidelines"
   git push -u origin feature/add-contributing-guide
   ```

5. **Create the Pull Request:**
   - Go to your repository on GitHub
   - You'll see a banner: "feature/add-contributing-guide had recent pushes"
   - Click **Compare & pull request**

6. **Fill in the PR details:**
   - Title: `Add contributing guidelines`
   - Description:
   ```markdown
   ## Summary
   This PR adds a CONTRIBUTING.md file to help new contributors get started.
   
   ## Changes
   - Added CONTRIBUTING.md with:
     - Contribution workflow
     - Code style guidelines
     - Bug reporting instructions
   
   ## Checklist
   - [x] File created
   - [x] Follows project conventions
   - [ ] Reviewed by maintainer
   
   Closes #[your task issue number]
   ```

7. **Link to an issue:**
   - In the description, add: `Closes #3` (use your task issue number)
   - This will auto-close the issue when merged

8. **Review the Files Changed tab:**
   - Click **Files changed**
   - See your additions in green

9. **Merge the PR:**
   - Click **Merge pull request**
   - Click **Confirm merge**
   - Click **Delete branch**

10. **Verify the issue was closed:**
    - Go to Issues
    - Your task issue should now be closed

### ✅ Success Criteria
- [ ] Feature branch created
- [ ] CONTRIBUTING.md file added
- [ ] Pull request created with description
- [ ] Issue linked and auto-closed
- [ ] PR merged and branch deleted


## Lab 4: Explore Repository Settings

**Objective:** Familiarize yourself with repository configuration.

**Time:** 5 minutes

### Instructions

1. **Navigate to Settings:**
   - Go to your repository
   - Click **⚙️ Settings** tab

2. **Explore General settings:**
   - Note the default branch setting
   - See feature toggles (Wiki, Issues, Projects)

3. **Check Branches settings:**
   - Click **Branches** in sidebar
   - Note: This is where branch protection rules go

4. **Review Collaborators:**
   - Click **Collaborators and teams**
   - This is where you'd add team members

5. **Look at Actions settings:**
   - Click **Actions** → **General**
   - See workflow permission options

### Reflection Questions
- What settings would you change for a team project?
- What branch protection rules would you recommend?
- How would you configure this for a customer's needs?

---

## 🎉 Lab Completion Checklist

- [ ] Lab 1: Created and labeled issues
- [ ] Lab 2: Set up project board
- [ ] Lab 3: Created and merged pull request
- [ ] Lab 4: Explored repository settings

<div class="callout callout-tip">
<div class="callout-title">📸 Keep Your Practice Repo</div>

This repository is now a great reference for demonstrating GitHub features to customers!
</div>
