---
layout: training-module
title: "Resources & Further Learning"
permalink: /beginner/module-2/resources/
module_number: 2
module_title: "Git Fundamentals & Version Control"
section_number: 6
total_sections: 7
phase: beginner
estimated_time: "5 min"
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
  url: /beginner/module-2/quiz/
  title: "Knowledge Check"
next_section:
  url: /beginner/module-2/scenarios/
  title: "CSM Scenarios"
---

## Official Git Resources

### 📚 Documentation

- **[Pro Git Book](https://git-scm.com/book/en/v2)** — Free, comprehensive Git book (online)
- **[Git Reference Manual](https://git-scm.com/docs)** — Official command documentation
- **[Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)** — Quick reference PDF

### 🎓 Interactive Learning

- **[GitHub Skills](https://skills.github.com/)** — Free interactive Git/GitHub courses
- **[Learn Git Branching](https://learngitbranching.js.org/)** — Visual, gamified Git practice
- **[Oh My Git!](https://ohmygit.org/)** — Git learning game

> ⚠️ **External Resources:** Links to learngitbranching.js.org and ohmygit.org lead to third-party websites not maintained by GitHub. Content may change without notice.

---

## Git Cheat Sheet

### Setup & Configuration

```bash
# Configure user info
git config --global user.name "Your Name"
git config --global user.email "email@example.com"

# View configuration
git config --list

# Set default branch name
git config --global init.defaultBranch main

```

### Repository Operations

```bash
# Initialize new repo
git init

# Clone existing repo
git clone <url>

# Check status
git status

# View history
git log
git log --oneline --graph

```

### Working with Changes

```bash
# Stage files
git add <file>
git add .              # All changes

# Commit
git commit -m "message"
git commit --amend     # Fix last commit

# View changes
git diff              # Unstaged changes
git diff --staged     # Staged changes

```

### Branching & Merging

```bash
# List branches
git branch

# Create branch
git branch <name>

# Switch branch
git checkout <name>
git switch <name>       # Modern alternative

# Create and switch
git checkout -b <name>

# Merge
git merge <branch>

# Delete branch
git branch -d <name>

```

### Remote Operations

```bash
# Add remote
git remote add origin <url>

# Push
git push origin <branch>
git push -u origin main  # Set upstream

# Pull
git pull origin <branch>

# Fetch (download without merge)
git fetch origin

```

### Undoing Changes

```bash
# Unstage file
git reset HEAD <file>

# Discard changes
git checkout -- <file>

# Undo last commit (keep changes)
git reset HEAD~1

# Create "undo" commit
git revert <commit>

```


## Git GUIs & Tools

### Desktop Applications

| Tool | Platform | Best For |
|------|----------|----------|
| **[GitHub Desktop](https://desktop.github.com/)** | Mac/Windows | Beginners, simple workflows |
| **[Sourcetree](https://www.sourcetreeapp.com/)** | Mac/Windows | Visual history exploration |
| **[GitKraken](https://www.gitkraken.com/)** | All | Beautiful interface, teams |
| **[Fork](https://git-fork.com/)** | Mac/Windows | Fast, lightweight |

### IDE Integration

Most modern IDEs have excellent Git integration:

- **VS Code** — Built-in Git + GitLens extension
- **JetBrains IDEs** — Powerful Git tools included
- **Xcode** — Source Control navigator

---

## Recommended Reading

### Books

| Title | Author | Focus |
|-------|--------|-------|
| *Pro Git* | Scott Chacon | Comprehensive Git guide |
| *Git Pocket Guide* | Richard Silverman | Quick reference |
| *Version Control with Git* | Jon Loeliger | Deep technical dive |

### Articles

- **[A successful Git branching model](https://nvie.com/posts/a-successful-git-branching-model/)** — Git Flow (though consider trunk-based for modern teams)
- **[GitHub Flow](https://docs.github.com/en/get-started/using-github/github-flow)** — Simplified branch workflow


## Glossary

| Term | Definition |
|------|------------|
| **Repository (Repo)** | A folder tracked by Git containing all project history |
| **Commit** | A snapshot of your project at a specific point |
| **Branch** | An independent line of development |
| **Merge** | Combining changes from different branches |
| **Remote** | A copy of the repository on another computer (like GitHub) |
| **Clone** | Copying a remote repository to your computer |
| **Pull** | Downloading and integrating remote changes |
| **Push** | Uploading local commits to a remote |
| **HEAD** | Pointer to your current branch/commit |
| **SHA/Hash** | Unique identifier for each commit |
| **Staging Area** | Files marked for the next commit |
| **Working Directory** | Your project files on disk |
| **Conflict** | When Git can't automatically merge changes |
| **Fast-forward** | Simple merge where branch pointer moves forward |

---

## Quick Reference Cards

### Most Common Commands

```
Daily workflow:
git pull          # Get latest changes
git checkout -b   # Create feature branch  
git add .         # Stage all changes
git commit -m     # Save snapshot
git push          # Upload to remote

Investigation:
git status        # What's changed?
git log           # What happened?
git diff          # What did I change?
git blame         # Who changed this line?

Recovery:
git reset         # Unstage files
git checkout --   # Discard changes
git revert        # Undo a commit safely

```

### When Things Go Wrong

| Problem | Solution |
|---------|----------|
| Committed to wrong branch | `git cherry-pick` the commit to correct branch |
| Need to undo last commit | `git reset HEAD~1` (unpushed) or `git revert` (pushed) |
| Merge conflict | Edit file, remove markers, `git add`, `git commit` |
| Accidentally deleted branch | `git reflog` to find it, `git checkout -b name sha` |
| Committed sensitive data | `git reset` if unpushed, rotate credentials if pushed |


<div class="callout callout-tip" markdown="1">
<div class="callout-title">📌 Bookmark This Page</div>

Keep this cheat sheet handy! You'll reference these commands frequently as you practice.
</div>
