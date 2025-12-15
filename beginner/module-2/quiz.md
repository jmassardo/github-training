---
layout: training-module
title: "Knowledge Check"
permalink: /beginner/module-2/quiz/
module_number: 2
module_title: "Git Fundamentals & Version Control"
section_number: 5
total_sections: 7
phase: beginner
estimated_time: "10 min"
module_index: /beginner/module-2/
sections:
  - title: "Context & Overview"
    url: "/beginner/module-2/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/beginner/module-2/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/beginner/module-2/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/beginner/module-2/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/beginner/module-2/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/beginner/module-2/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/beginner/module-2/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
prev_section:
  url: /beginner/module-2/labs/
  title: "Hands-On Labs"
next_section:
  url: /beginner/module-2/resources/
  title: "Resources"
---

## Test Your Knowledge

Answer these questions to check your understanding of Git concepts.

---

### Question 1: Git vs GitHub

**What's the key difference between Git and GitHub?**

<details markdown="1">
<summary>Show Answer</summary>

**Git** is a version control tool (software) that runs locally on your computer.

**GitHub** is a cloud platform/service that hosts Git repositories and adds collaboration features like pull requests, issues, Actions, and security scanning.

**Analogy**: Git is like Microsoft Word (tracks changes), GitHub is like Microsoft 365 (cloud sharing and collaboration).
</details>


### Question 2: Three States

**What are the three states a file can be in within Git?**

<details markdown="1">
<summary>Show Answer</summary>

1. **Modified** — Changed but not staged
2. **Staged** — Marked for the next commit (via `git add`)
3. **Committed** — Safely stored in Git's database

The workflow: modify → stage → commit
</details>

---

### Question 3: Commit Anatomy

**What information is stored in a Git commit?**

<details markdown="1">
<summary>Show Answer</summary>

Each commit contains:

- **Snapshot** of all tracked files
- **Author** (who made the commit)
- **Timestamp** (when it was made)
- **Message** (why it was made)
- **SHA hash** (unique identifier like `a3f2b91`)
- **Parent pointer** (link to previous commit)
</details>


### Question 4: Branch Purpose

**Your customer says: "We're afraid to try new things because we might break production." What Git feature addresses this?**

<details markdown="1">
<summary>Show Answer</summary>

**Branches** solve this problem.

Branches create isolated environments for:

- Developing new features
- Experimenting safely
- Fixing bugs

Code on a branch doesn't affect `main` until merged. This enables safe experimentation.

**Follow-up**: Pair branches with GitHub's **branch protection rules** to require tests and reviews before merging to main.
</details>

---

### Question 5: Merge Conflicts

**What causes a merge conflict in Git?**

<details markdown="1">
<summary>Show Answer</summary>

A conflict occurs when:

1. Two branches modify the **same lines** in the **same file**
2. Git cannot automatically determine which change to keep

Conflicts require human intervention to resolve by:

1. Opening the conflicted file
2. Choosing which changes to keep
3. Removing conflict markers
4. Staging and committing the resolution
</details>


### Question 6: Commands Match

**Match each command to its purpose:**

| Command | Purpose |
|---------|---------|
| 1. `git init` | A. Show uncommitted changes |
| 2. `git add` | B. Create a new repository |
| 3. `git commit` | C. Download remote changes |
| 4. `git push` | D. Stage files for commit |
| 5. `git pull` | E. Save a snapshot |
| 6. `git status` | F. Upload to remote |

<details markdown="1">
<summary>Show Answer</summary>

1. `git init` → **B. Create a new repository**
2. `git add` → **D. Stage files for commit**
3. `git commit` → **E. Save a snapshot**
4. `git push` → **F. Upload to remote**
5. `git pull` → **C. Download remote changes**
6. `git status` → **A. Show uncommitted changes**
</details>

---

### Question 7: Undo Operations

**Your customer accidentally committed sensitive data. The commit hasn't been pushed yet. What should they do?**

<details markdown="1">
<summary>Show Answer</summary>

**If not yet pushed**, they have options:

1. **Amend the commit** (if it was the last commit):
   ```bash
   # Remove the sensitive file
   git rm --cached sensitive-file.txt
   # Amend the commit
   git commit --amend
   ```

2. **Reset to before the commit**:
   ```bash
   git reset HEAD~1  # Undo last commit, keep changes
   # Remove sensitive file
   # Re-commit
   ```

**If already pushed**, the data is exposed. They should:
1. Rotate any credentials immediately
2. Use `git revert` to undo publicly
3. Consider BFG Repo-Cleaner to remove from history
4. Enable GitHub's **secret scanning** to prevent this in the future
</details>


### Question 8: Distributed Benefits

**Why is Git called a "distributed" version control system?**

<details markdown="1">
<summary>Show Answer</summary>

**Distributed** means every developer has a **complete copy** of the entire repository history, not just the current files.

Benefits:

- **Work offline** — No server connection needed to commit
- **Fast operations** — Everything is local
- **No single point of failure** — If server dies, any clone is a full backup
- **Flexible workflows** — Can sync with multiple remotes

This differs from **centralized** systems (SVN, CVS) where the server holds all history and you can only work when connected.
</details>

---

### Question 9: Branch Naming

**A customer shows you their branches: `test`, `fix`, `new`, `final`. What's the problem?**

<details markdown="1">
<summary>Show Answer</summary>

**Poor naming conventions** — these names don't convey:
- What feature is being developed
- Who is working on it
- The purpose or scope

**Better naming patterns:**

- `feature/user-authentication`
- `fix/login-error-null-pointer`
- `hotfix/security-patch-cve-2025-1234`
- `release/v2.1.0`

Good branch names are:

- **Descriptive** — Clear purpose
- **Consistent** — Follow a pattern
- **Prefixed** — Type of work (feature, fix, etc.)
</details>


### Question 10: Customer Scenario

**A customer says: "We want to know who introduced a bug and when." What Git command would help?**

<details markdown="1">
<summary>Show Answer</summary>

**`git blame`** shows who last modified each line of a file:

```bash
git blame problematic-file.js

```

For more investigation:

- **`git log -p filename`** — Shows commits that changed the file
- **`git bisect`** — Binary search to find which commit introduced a bug

**GitHub enhancement:** GitHub's blame view is visual and clickable, making it easier to navigate history.
</details>

---

## 🎯 Score Yourself

| Score | Assessment |
|-------|------------|
| 10/10 | 🌟 Excellent! You've mastered Git fundamentals |
| 8-9 | 👍 Great job! Review any areas you missed |
| 6-7 | 📚 Good progress — revisit the Core Concepts section |
| 0-5 | 🔄 Consider re-reading the module before moving on |

<div class="callout callout-tip">
<div class="callout-title">💡 Git Takes Practice</div>

Don't worry if Git concepts feel abstract. They become intuitive with practice. Keep using the commands and concepts will solidify!
</div>
