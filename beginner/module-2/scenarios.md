---
layout: training-module
title: "Real-World CSM Scenarios"
permalink: /beginner/module-2/scenarios/
module_number: 2
module_title: "Git Fundamentals & Version Control"
section_number: 7
total_sections: 7
phase: beginner
estimated_time: "15 min"
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
  url: /beginner/module-2/resources/
  title: "Resources"
next_section:
  url: /beginner/module-3/
  title: "Module 3: GitHub Platform"
---

## Apply Your Knowledge

These scenarios simulate real customer conversations. Practice how you'd respond using your Git knowledge.

---

## Scenario 1: The Merge Conflict Crisis

### Situation

**Customer:** DataCorp (150 developers)

**Context:** Weekly check-in call. The engineering manager sounds frustrated.

**They say:**

> "We're drowning in merge conflicts. It's taking developers hours to resolve them. Every PR seems to have conflicts. This is killing our productivity."

### Your Task

How do you diagnose and address this issue?

<details markdown="1">
<summary>Show Suggested Approach</summary>

**Diagnostic questions:**

1. "How long do feature branches typically live before merging?"
2. "How often do developers pull from main into their branches?"
3. "How large is your codebase? Any hotspot files that everyone edits?"
4. "What's your branching strategy?"

**Common root causes:**

- **Long-lived branches** — More time = more divergence = more conflicts
- **Infrequent integration** — Not pulling main into feature branches
- **Monolithic codebase** — Everyone editing the same files
- **Large PRs** — More changes = more conflict surface area

**Recommendations:**

1. **Shorter branches:**
   - Feature branches should live days, not weeks
   - Break large features into smaller, mergeable chunks

2. **Regular integration:**
   ```bash
   # Daily habit: update feature branch from main
   git checkout main
   git pull
   git checkout feature/my-feature
   git merge main
   ```

3. **Smaller PRs:**
   - Aim for <400 lines changed
   - Easier to review, easier to merge

4. **GitHub features to help:**
   - **Protected branches** requiring up-to-date branches before merge
   - **Auto-merge** to merge when checks pass
   - **Require linear history** (rebase workflow) reduces conflict complexity

5. **Long-term:**
   - Consider breaking monolithic repo into smaller services
   - Use CODEOWNERS to distribute ownership

</details>


## Scenario 2: The "We Can't Work Offline" Complaint

### Situation

**Customer:** FieldService Inc. (80 developers, many remote/traveling)

**Context:** Discovery call for GitHub Enterprise evaluation.

**The IT Director says:**

> "Our developers often work from planes, trains, and remote sites with spotty internet. We're worried about Git and GitHub requiring constant connectivity."

### Your Task

How do you address this concern?

<details markdown="1">
<summary>Show Suggested Approach</summary>

**Key message:** Git's **distributed** nature is actually perfect for offline work!

**Explain the architecture:**

1. **Local repository = full history**
   - Every clone has complete history
   - Developers can commit, branch, view history — all offline
   
2. **What works offline:**
   - `git commit` — Save work locally
   - `git branch` — Create and switch branches
   - `git log` — View entire history
   - `git diff` — Compare changes
   - `git merge` — Combine branches locally
   - `git blame` — See who changed what

3. **What needs connectivity:**
   - `git push` — Upload to GitHub
   - `git pull` — Download from GitHub
   - GitHub features (Issues, PRs, Actions)

**Practical workflow:**

```
✈️ On the plane:

1. Make commits locally (they're saved!)
2. Create branches, merge, etc.

📶 Back online:

3. git push to sync with GitHub
4. Continue collaboration

```

**GitHub features that help:**

- **GitHub Desktop** — Visual client, queues changes for push
- **GitHub Mobile** — Review PRs, respond to issues on phone
- **Codespaces** — When you DO have internet, instant cloud dev environment

</details>

---

## Scenario 3: The Accidental Commit

### Situation

**Customer:** HealthTech Solutions (regulated industry)

**Context:** Urgent Slack message from your champion.

**They write:**

> "URGENT: A developer accidentally committed an API key to our public repository. It was pushed to main. What do we do??"

### Your Task

Walk them through incident response.

<details markdown="1">
<summary>Show Suggested Approach</summary>

**Immediate priority:** ROTATE THE KEY

1. **Step 1: Rotate the credential NOW**
   - The key is compromised the moment it's pushed
   - Generate new API key
   - Update applications to use new key
   - Revoke the old key

2. **Step 2: Remove from repository**
   ```bash
   # Remove file and commit
   git rm --cached api-keys.txt
   git commit -m "Remove accidentally committed API keys"
   git push
   ```

   ⚠️ **Warning:** This doesn't remove it from history!

3. **Step 3: Scrub from history (optional, complex)**
   - Use BFG Repo-Cleaner or `git filter-branch`
   - Requires force push
   - All team members must re-clone
   - May not be necessary if key is already rotated

4. **Step 4: Prevent future incidents**
   - Enable **GitHub Secret Scanning** (GHAS feature)
   - Add `.gitignore` entries for sensitive files
   - Use **pre-commit hooks** to scan for secrets
   - Consider **push protection** to block secrets before they're pushed

**What GitHub Secret Scanning does:**

- Scans commits for known secret patterns
- Alerts repository admins
- Can block pushes containing secrets (push protection)
- Partners with providers to auto-revoke leaked keys

**Customer conversation:**
> "The good news is you caught this quickly. Let's rotate that key immediately, then set up protections so this can't happen again."

</details>


## Scenario 4: The Migration Fear

### Situation

**Customer:** LegacyCorp (200 developers)

**Context:** They're considering migrating from SVN to GitHub.

**The VP of Engineering says:**

> "We've been on SVN for 15 years. All our history is there. I'm worried about losing everything in a migration. Can we keep our commit history?"

### Your Task

Address migration concerns and explain options.

<details markdown="1">
<summary>Show Suggested Approach</summary>

**Reassurance:** History can be preserved!

**Migration options:**

1. **Full history migration:**
   - `git svn clone` converts SVN history to Git commits
   - All commits, authors, dates preserved
   - Branch/tag structure can be maintained
   
2. **Partial history (snapshot):**
   - Import current state without history
   - Simpler, faster migration
   - Keep SVN as read-only archive

3. **Phased migration:**
   - New projects start on GitHub
   - Migrate repos gradually
   - Lower risk, longer timeline

**GitHub tools:**

- **GitHub Importer** — Web-based import from SVN, Mercurial, TFS
- **GitHub Enterprise Importer (GEI)** — Enterprise-grade migration tool
- **Professional Services** — GitHub experts to plan and execute

**Key benefits of migrating:**

- Distributed architecture (work offline)
- Better branching (SVN branches are expensive)
- Pull requests and code review
- GitHub Actions (CI/CD)
- Security scanning
- Modern developer experience

**Migration planning questions:**

1. How many repositories?
2. How much history is truly valuable?
3. What's your timeline?
4. Do you need to maintain SVN during transition?

</details>

---

## Scenario 5: The Training Request

### Situation

**Customer:** GrowthStartup (team doubling from 20 to 40 developers)

**Context:** Email from your champion.

**They write:**

> "We're hiring 20 new developers over the next quarter. Half of them have never used Git before (coming from enterprise backgrounds). Can GitHub help with training?"

### Your Task

Propose a training approach.

<details markdown="1">
<summary>Show Suggested Approach</summary>

**Assessment questions:**

1. What tools did they use before? (TFS, SVN, Perforce?)
2. What's their development experience level overall?
3. What workflow will they be using? (Git Flow, GitHub Flow?)
4. What's the timeline for onboarding?

**Training resources:**

**Self-paced (free):**

- **[GitHub Skills](https://skills.github.com/)** — Interactive courses
- **[Git documentation](https://git-scm.com/doc)** — Official guides
- **[Learn Git Branching](https://learngitbranching.js.org/)** — Visual practice

> ⚠️ **Note:** learngitbranching.js.org is a third-party resource not maintained by GitHub.

**Instructor-led options:**

- **GitHub Professional Services** — Custom training
- **GitHub Certified Partners** — Training delivery partners
- **Internal champions** — Train-the-trainer approach

**Recommended onboarding plan:**

**Week 1: Fundamentals**
- Git basics (commit, branch, merge)
- GitHub interface orientation
- Company workflow introduction

**Week 2: Workflow**
- Pull request process
- Code review practices
- Branch naming conventions

**Week 3: Advanced**
- Handling merge conflicts
- Git troubleshooting
- GitHub Actions basics

**Week 4: Practice**
- Paired programming with experienced devs
- First real PRs with support
- Q&A sessions

**Ongoing:**

- Office hours for questions
- Documentation wiki
- Slack channel for Git help

**Your role:**
> "I can connect you with our Professional Services team for formal training, and also share self-paced resources your team can use immediately."

</details>


## 🎯 Module 2 Complete!

Congratulations! You now understand:

- ✅ Why version control exists and how Git solves collaboration problems
- ✅ Git's core concepts: repos, commits, branches, merging
- ✅ The difference between Git and GitHub
- ✅ How to use essential Git commands
- ✅ How to address common customer Git questions

<div class="callout callout-success" markdown="1">
<div class="callout-title">🚀 Ready for Module 3?</div>

In the next module, we'll explore the GitHub platform in depth — the features that transform Git from a command-line tool into a collaboration powerhouse.

[Start Module 3: GitHub Platform →]({{ site.baseurl }}/beginner/module-3/)
</div>

---

## Feedback

How was this module? Share feedback with your training coordinator!
