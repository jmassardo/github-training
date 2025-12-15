---
layout: training-module
title: "Hands-On Labs"
permalink: /beginner/module-2/labs/
module_number: 2
module_title: "Git Fundamentals & Version Control"
section_number: 4
total_sections: 7
phase: beginner
estimated_time: "25 min"
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
  url: /beginner/module-2/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /beginner/module-2/quiz/
  title: "Knowledge Check"
---

## Lab Overview

Practice your Git skills with these hands-on exercises. Complete them in order.

<div class="callout callout-info">
<div class="callout-title">📋 Prerequisites</div>

- Completed the Guided Walkthrough section
- Git installed and configured
- GitHub account
- Terminal access
</div>

---

## Lab 1: Feature Branch Workflow

**Objective:** Practice the standard feature branch workflow.

**Time:** 10 minutes

### Scenario

You're working on a project and need to add a new feature without affecting the main branch.

### Instructions

1. **Create a new repository or use existing one:**
   ```bash
   mkdir git-lab-project
   cd git-lab-project
   git init
   echo "# Git Lab Project" > README.md
   git add README.md
   git commit -m "Initial commit"
   ```

2. **Create a feature branch:**
   ```bash
   git checkout -b feature/add-contributors
   ```

3. **Create a CONTRIBUTORS.md file:**
   ```bash
   cat > CONTRIBUTORS.md << 'EOF'
   # Contributors

   ## Maintainers
   - Your Name (@username)

   ## How to Contribute
   1. Fork the repository
   2. Create a feature branch
   3. Make your changes
   4. Submit a pull request
   EOF
   ```

4. **Stage and commit:**
   ```bash
   git add CONTRIBUTORS.md
   git commit -m "Add CONTRIBUTORS file with contribution guidelines"
   ```

5. **Switch back to main:**
   ```bash
   git checkout main
   ```

6. **Verify CONTRIBUTORS.md doesn't exist on main:**
   ```bash
   ls -la
   # CONTRIBUTORS.md should NOT be listed
   ```

7. **Merge the feature branch:**
   ```bash
   git merge feature/add-contributors
   ```

8. **Verify the file now exists:**
   ```bash
   ls -la
   # CONTRIBUTORS.md should now be listed
   ```

9. **Clean up:**
   ```bash
   git branch -d feature/add-contributors
   ```

### ✅ Success Criteria
- [ ] Feature branch created
- [ ] File created and committed on feature branch
- [ ] File not visible on main before merge
- [ ] Successful merge to main
- [ ] Feature branch deleted


## Lab 2: Resolving a Merge Conflict

**Objective:** Learn to resolve merge conflicts confidently.

**Time:** 10 minutes

### Scenario

Two team members edited the same file differently. You need to merge their changes.

### Instructions

1. **Continue with your project (or start fresh):**
   ```bash
   cd git-lab-project  # or create new
   ```

2. **Ensure you have a README on main:**
   ```bash
   git checkout main
   echo "# Git Lab Project" > README.md
   echo "Version: 1.0" >> README.md
   git add README.md
   git commit -m "Add version to README"
   ```

3. **Create two branches from the same point:**
   ```bash
   # Create first branch
   git checkout -b team-alice
   
   # Edit README (Alice's version)
   cat > README.md << 'EOF'
   # Git Lab Project
   Version: 1.0
   
   ## Description
   This project was set up by Alice.
   EOF
   git add README.md
   git commit -m "Alice: Add description"
   ```

4. **Create Bob's branch from main (not from Alice's branch):**
   ```bash
   git checkout main
   git checkout -b team-bob
   
   # Edit README (Bob's version)
   cat > README.md << 'EOF'
   # Git Lab Project
   Version: 1.0
   
   ## Description
   Bob created this awesome project.
   EOF
   git add README.md
   git commit -m "Bob: Add description"
   ```

5. **Merge Alice's changes to main:**
   ```bash
   git checkout main
   git merge team-alice
   # This should succeed (fast-forward)
   ```

6. **Try to merge Bob's changes:**
   ```bash
   git merge team-bob
   # This will create a CONFLICT!
   ```

   **You'll see:**
   ```
   Auto-merging README.md
   CONFLICT (content): Merge conflict in README.md
   Automatic merge failed; fix conflicts and then commit the result.
   ```

7. **View the conflict:**
   ```bash
   cat README.md
   ```

   **You'll see something like:**
   ```
   # Git Lab Project
   Version: 1.0
   
   ## Description
   <<<<<<< HEAD
   This project was set up by Alice.
   =======
   Bob created this awesome project.
   >>>>>>> team-bob
   ```

8. **Resolve the conflict:**
   
   Edit README.md to combine both contributions:
   ```bash
   cat > README.md << 'EOF'
   # Git Lab Project
   Version: 1.0
   
   ## Description
   This project was set up by Alice and Bob working together.
   EOF
   ```

9. **Complete the merge:**
   ```bash
   git add README.md
   git commit -m "Merge team-bob: Combine Alice and Bob's descriptions"
   ```

10. **Verify the history:**
    ```bash
    git log --oneline --graph
    ```

### ✅ Success Criteria
- [ ] Created two branches with conflicting changes
- [ ] Experienced a merge conflict
- [ ] Successfully resolved the conflict
- [ ] Completed the merge commit

<div class="callout callout-tip">
<div class="callout-title">💡 Pro Tip</div>

In VS Code, conflicts are highlighted with color coding and you can click "Accept Current Change", "Accept Incoming Change", or "Accept Both Changes" to resolve quickly.
</div>

---

## Lab 3: Interactive Git History

**Objective:** Explore commit history and understand Git log options.

**Time:** 5 minutes

### Instructions

Using your lab repository, try these commands:

1. **Basic log:**
   ```bash
   git log
   ```

2. **Compact log:**
   ```bash
   git log --oneline
   ```

3. **Graph view:**
   ```bash
   git log --oneline --graph --all
   ```

4. **With stats:**
   ```bash
   git log --stat
   ```

5. **Search commits by message:**
   ```bash
   git log --grep="Alice"
   ```

6. **Show specific file history:**
   ```bash
   git log --oneline -- README.md
   ```

7. **Show who changed each line (blame):**
   ```bash
   git blame README.md
   ```

### ✅ Success Criteria
- [ ] Ran each command and understood the output
- [ ] Can find specific commits in history


## Lab 4: Undoing Changes

**Objective:** Learn different ways to undo work in Git.

**Time:** 10 minutes

### Scenario 1: Unstage a File

```bash
# Make a change
echo "Temporary text" >> README.md

# Stage it
git add README.md

# Check status (file is staged)
git status

# Unstage without losing changes
git reset HEAD README.md

# Check status (file is modified but unstaged)
git status

# Discard the changes entirely
git checkout -- README.md

```

### Scenario 2: Amend the Last Commit

```bash
# Make a commit with a typo in the message
echo "New feature" >> README.md
git add README.md
git commit -m "Add new featre"  # Oops, typo!

# Fix the commit message
git commit --amend -m "Add new feature"

# Verify
git log --oneline -1

```

### Scenario 3: Revert a Commit

```bash
# Make a bad commit
echo "This is wrong" >> README.md
git add README.md
git commit -m "Add wrong content"

# Create a new commit that undoes the previous one
git revert HEAD

# This opens an editor - save and close
# Now you have a new "revert" commit
git log --oneline -3

```

### ✅ Success Criteria
- [ ] Successfully unstaged a file
- [ ] Amended a commit message
- [ ] Created a revert commit

<div class="callout callout-warning">
<div class="callout-title">⚠️ Important Note</div>

**Never amend commits that have been pushed** to a shared repository. This rewrites history and causes problems for others. Use `git revert` instead for pushed commits.
</div>

---

## 🎉 Lab Completion Checklist

- [ ] Lab 1: Feature Branch Workflow
- [ ] Lab 2: Resolving Merge Conflicts
- [ ] Lab 3: Exploring Git History
- [ ] Lab 4: Undoing Changes

<div class="callout callout-tip">
<div class="callout-title">📸 Keep Your Lab Repo</div>

Don't delete this repository! You can use it for future practice and experimentation.
</div>
