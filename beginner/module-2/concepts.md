---
layout: training-module
title: "Core Concepts"
permalink: /beginner/module-2/concepts/
module_number: 2
module_title: "Git Fundamentals & Version Control"
section_number: 2
total_sections: 7
phase: beginner
estimated_time: "20 min"
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
  url: /beginner/module-2/overview/
  title: "Context & Overview"
next_section:
  url: /beginner/module-2/walkthrough/
  title: "Guided Walkthrough"
---

<div class="callout callout-info">
<div class="callout-title">📚 Official Documentation</div>
For comprehensive reference, see <a href="https://git-scm.com/doc">Git Documentation</a> and <a href="https://docs.github.com/en/get-started/getting-started-with-git">GitHub's Git Guide</a>.
</div>

## Git's Mental Model

Understanding Git requires understanding a few key concepts. Let's build your mental model step by step.

---

## The Repository (Repo)

A **repository** is a folder that Git tracks. It contains:

- Your project files (code, docs, images, etc.)
- A hidden `.git` folder with all the history and metadata

Think of a repo as a project with a complete memory of everything that's ever happened to it.

```
my-project/
├── .git/           # Git's database (hidden folder)
├── README.md
├── src/
│   └── index.js
└── package.json

```

### Local vs Remote Repositories

| Type | Location | Purpose |
|------|----------|---------|
| **Local repo** | On your computer | Where you work |
| **Remote repo** | On GitHub | Where you collaborate and back up |

You typically work locally and **push** changes to the remote. You **pull** others' changes from the remote.


## Commits: Snapshots of Your Work

A **commit** is a snapshot of your entire project at a specific moment. Each commit includes:

- ✅ The state of all files
- ✅ Who made the change
- ✅ When they made it
- ✅ Why they made it (commit message)
- ✅ A unique ID (SHA hash like `a3f2b91`)

**Commits are immutable** — once created, they never change. This creates an unbreakable chain of history.

```
Commit History (like a timeline):

a3f2b91 - "Add user login feature" (Dec 9, 2025)
    ↑
b7c8d42 - "Fix homepage typo" (Dec 8, 2025)
    ↑
c9e1f53 - "Initial project setup" (Dec 7, 2025)

```

### Writing Good Commit Messages

| ❌ Bad | ✅ Good |
|--------|--------|
| "Fix" | "Fix null pointer exception in user authentication" |
| "Update" | "Update homepage hero image for Q4 campaign" |
| "WIP" | "Add basic validation to contact form (WIP)" |
| "stuff" | "Refactor database queries for better performance" |

**Format tip:**

```
[type]: Short summary (50 chars or less)

Longer description if needed. Explain WHY, not just WHAT.
Reference issue numbers: Fixes #123

```

---

## The Three States of Files

Git files exist in three states:

1. **Modified**: You changed the file, but haven't told Git yet
2. **Staged**: You marked the file to be included in the next commit
3. **Committed**: The file is safely stored in Git's database

Think of staging as packing a box before shipping it. You choose what goes in.

```
Working Directory  →  Staging Area  →  Repository
(modified files)     (files ready      (committed
                     to commit)         snapshots)

[edit file]       →  [git add]     →  [git commit]

```

### Why a Staging Area?

The staging area lets you:

- **Be selective**: Commit only some changes, not everything
- **Review before committing**: Double-check what you're about to save
- **Create focused commits**: One logical change per commit


## Branches: Parallel Universes for Your Code

A **branch** is an independent line of development. Branches let you:

- Work on features without affecting the main codebase
- Experiment safely
- Collaborate without conflicts
- Organize work by feature, bug fix, or team

```
main:      A---B---C---------F
                \           /
feature:         D---E-----

```

### Key Branch Types

| Branch Type | Purpose | Example Name |
|------------|---------|--------------|
| **main** | Primary branch, production-ready | `main` |
| **Feature** | New functionality | `feature/user-auth` |
| **Bug fix** | Fix issues | `fix/login-error` |
| **Release** | Prepare releases | `release/v2.0` |
| **Hotfix** | Emergency production fixes | `hotfix/security-patch` |

### Creating and Switching Branches

```bash
# Create a new branch
git branch feature/new-feature

# Switch to that branch
git checkout feature/new-feature

# Or do both in one command
git checkout -b feature/new-feature

```

---

## Merging: Combining Branches

**Merging** brings changes from one branch into another. Git is smart — it automatically combines changes unless there's a conflict.

```
Before merge:
main:    A---B---C
              \
feature:       D---E

After merge:
main:    A---B---C-------F
              \         /
feature:       D---E----

```

### Types of Merges

**Fast-Forward Merge** — When main hasn't changed since you branched:

```
Before:  main: A---B
                   \
         feature:   C---D

After:   main: A---B---C---D (fast-forwarded)

```

**Three-Way Merge** — When both branches have new commits:

```
Before:  main:    A---B---E
                      \
         feature:      C---D

After:   main:    A---B---E---F (merge commit)
                      \      /
         feature:      C---D-

```


## Merge Conflicts: When Git Needs Help

A **conflict** happens when:

1. Two people edit the same line of the same file
2. Git can't automatically decide which change to keep

**Don't panic!** Conflicts are normal and solvable.

### What Conflicts Look Like

When you try to merge and there's a conflict, Git marks the file:

```javascript
function greeting() {
<<<<<<< HEAD
  return "Hello, World!";
=======
  return "Hi there!";
>>>>>>> feature/new-greeting
}

```

- `<<<<<<< HEAD` — Your current branch's version
- `=======` — Separator
- `>>>>>>> feature/new-greeting` — Incoming branch's version

### Resolving Conflicts

1. **Open the conflicted file**
2. **Choose which change to keep** (or combine them)
3. **Remove the conflict markers**
4. **Stage the resolved file**: `git add filename`
5. **Complete the merge**: `git commit`

**Resolved version:**

```javascript
function greeting() {
  return "Hello there!";  // Combined both versions
}

```

---

## The Git Workflow

Putting it all together, here's a typical workflow:

```
1. Pull latest changes        git pull origin main
2. Create feature branch      git checkout -b feature/my-feature
3. Make changes              [edit files]
4. Stage changes             git add .
5. Commit changes            git commit -m "Add feature"
6. Push to remote            git push origin feature/my-feature
7. Create Pull Request       [on GitHub]
8. Review & Merge            [on GitHub]
9. Delete branch             git branch -d feature/my-feature

```


## Visual Summary

<div class="callout callout-info">
<div class="callout-title">🎨 Key Concepts at a Glance</div>

**Repository**: Your project folder + Git history  
**Commit**: A snapshot with who/what/when/why  
**Branch**: Parallel line of development  
**Merge**: Combining branches together  
**Remote**: GitHub copy for collaboration  
**Conflict**: When Git needs human help to merge

</div>

---

## Coming Up Next

Now that you understand the concepts, let's put them into practice with a hands-on walkthrough of creating a repository, making commits, and pushing to GitHub.
