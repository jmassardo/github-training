---
layout: training-module
title: "Guided Walkthrough"
permalink: /beginner/module-2/walkthrough/
module_number: 2
module_title: "Git Fundamentals & Version Control"
section_number: 3
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
  url: /beginner/module-2/concepts/
  title: "Core Concepts"
next_section:
  url: /beginner/module-2/labs/
  title: "Hands-On Labs"
---

## Your First Git Repository

Let's walk through creating and using a Git repository step by step.

<div class="callout callout-info">
<div class="callout-title">📋 Prerequisites</div>

- Git installed on your computer ([Download Git](https://git-scm.com/downloads))
- A text editor (VS Code recommended)
- Terminal/Command Prompt access

**Check if Git is installed:**
```bash
git --version
# Should output something like: git version 2.x.x (version 2.39 or later recommended)
```

</div>

---

## Step 1: Configure Git (One-Time Setup)

Tell Git who you are:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"

```

Verify your configuration:

```bash
git config --list

```

<div class="callout callout-tip">
<div class="callout-title">💡 Why This Matters</div>

Every commit records who made it. This configuration appears in your commit history and GitHub contribution graphs.
</div>


## Step 2: Create a Project Folder

```bash
mkdir my-first-repo
cd my-first-repo

```

---

## Step 3: Initialize Git

Turn this folder into a Git repository:

```bash
git init

```

**What you'll see:**

```
Initialized empty Git repository in /path/to/my-first-repo/.git/

```

**What just happened:** Git created a hidden `.git` folder to store all history.


## Step 4: Create a File

```bash
echo "# My First Repository" > README.md

```

Check the status:

```bash
git status

```

**What you'll see:**

```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present

```

**Translation:** Git sees `README.md` but isn't tracking it yet (it's untracked).

---

## Step 5: Stage the File

Add the file to the staging area:

```bash
git add README.md

```

Check status again:

```bash
git status

```

**What you'll see:**

```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md

```

**Translation:** `README.md` is now staged and ready to commit.


## Step 6: Make Your First Commit

```bash
git commit -m "Initial commit: Add README"

```

**What you'll see:**

```
[main (root-commit) a3f2b91] Initial commit: Add README
 1 file changed, 1 insertion(+)
 create mode 100644 README.md

```

**Translation:** You created commit `a3f2b91` with 1 file and 1 line added.

---

## Step 7: View Your History

```bash
git log

```

**What you'll see:**

```
commit a3f2b918c7f6e5d4b3a2c1e9d8f7a6b5c4d3e2f1
Author: Your Name <your.email@example.com>
Date:   Mon Dec 9 10:30:00 2025 -0500

    Initial commit: Add README

```

🎉 **Congratulations!** You've created your first Git repository and commit!


## Making More Changes

### Edit the File

Add more content:

```bash
echo "This is my first Git project!" >> README.md

```

### Check What Changed

```bash
git diff

```

**What you'll see:**

```diff
diff --git a/README.md b/README.md
index 4d1ae35..8ab12cd 100644
--- a/README.md
+++ b/README.md
@@ -1 +1,2 @@
 # My First Repository
+This is my first Git project!

```

**Translation:** Shows added lines (green +) and removed lines (red -).

### Stage and Commit

```bash
git add README.md
git commit -m "Add project description to README"

```

---

## Working with Branches

### Create a Feature Branch

```bash
git checkout -b feature/add-about

```

**What you'll see:**

```
Switched to a new branch 'feature/add-about'

```

### Make Changes on the Branch

Create a new file:

```bash
echo "# About\n\nThis project teaches Git basics." > ABOUT.md
git add ABOUT.md
git commit -m "Add ABOUT page"

```

### View All Branches

```bash
git branch

```

**What you'll see:**

```
* feature/add-about
  main

```

The `*` shows your current branch.


## Merging Branches

### Switch Back to Main

```bash
git checkout main

```

### Merge Your Feature

```bash
git merge feature/add-about

```

**What you'll see:**

```
Updating a3f2b91..d4e5f67
Fast-forward
 ABOUT.md | 3 +++
 1 file changed, 3 insertions(+)
 create mode 100644 ABOUT.md

```

### Delete the Feature Branch

```bash
git branch -d feature/add-about

```

---

## Pushing to GitHub

Now let's connect your local repo to GitHub.

### Create a Repository on GitHub

1. Go to [github.com/new](https://github.com/new)
2. Name it `my-first-repo`
3. **Do NOT** initialize with README (we already have one)
4. Click **Create repository**

### Connect Local to Remote

GitHub shows you commands — use the "existing repository" option:

```bash
git remote add origin https://github.com/YOUR-USERNAME/my-first-repo.git
git branch -M main
git push -u origin main

```

**What you'll see:**

```
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (6/6), 548 bytes | 548.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To https://github.com/YOUR-USERNAME/my-first-repo.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.

```

### Verify on GitHub

Refresh your GitHub repository page — your code is now there!


## Common Commands Reference

| Command | What It Does |
|---------|--------------|
| `git init` | Initialize a new repository |
| `git status` | Show current state |
| `git add <file>` | Stage a file |
| `git add .` | Stage all changes |
| `git commit -m "msg"` | Create a commit |
| `git log` | View commit history |
| `git diff` | Show unstaged changes |
| `git branch` | List branches |
| `git checkout -b name` | Create and switch to branch |
| `git merge branch` | Merge branch into current |
| `git push` | Upload to remote |
| `git pull` | Download from remote |

---

## Coming Up Next

Now that you've walked through the basics, let's practice with hands-on labs where you'll work through common Git scenarios.
