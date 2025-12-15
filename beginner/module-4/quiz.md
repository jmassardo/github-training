---
layout: training-module
title: "Knowledge Check"
permalink: /beginner/module-4/quiz/
module_number: 4
module_title: "SCM Process"
section_number: 5
total_sections: 7
phase: beginner
estimated_time: "20 min"
module_index: /beginner/module-4/
sections:
  - title: "Context & Overview"
    url: "/beginner/module-4/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/beginner/module-4/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/beginner/module-4/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/beginner/module-4/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/beginner/module-4/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/beginner/module-4/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/beginner/module-4/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
prev_section:
  url: /beginner/module-4/labs/
  title: "Hands-On Labs"
next_section:
  url: /beginner/module-4/resources/
  title: "Resources"
---

# Knowledge Check

Test your understanding of SCM process concepts.

## Questions

**Question 1:** A customer wants to ensure all production code is reviewed by at least two people, but their emergency response team needs to be able to push hotfixes directly in critical situations. How should you configure this?

<details markdown="1">
<summary>View Answer</summary>

**Best approach: Use Repository Rulesets with bypass actors**

Configuration:

```yaml
Ruleset: Production Protection
Enforcement: Active
Bypass Actors:
  - @emergency-response-team (with audit logging)

Rules:
  - Require pull request
      Required approvals: 2
  - Block force pushes
  - Require status checks

```

This provides:

- Normal workflow requires 2 approvals
- Emergency team can bypass when needed
- All bypasses are logged for audit
- Better than disabling protection entirely

Alternative with branch protection:

- Would need to use "Allow specified actors to bypass"
- Less flexible than rulesets for complex scenarios
</details>

**Question 2:** What is the correct syntax to require both the security team AND the backend team to review any Python files in the authentication module?

<details markdown="1">
<summary>View Answer</summary>

In CODEOWNERS:

```
/src/auth/**/*.py    @org/security-team @org/backend-team

```

Important notes:

- Both teams will be requested as reviewers
- By default, only ONE approval from listed owners is required
- To require ALL listed owners to approve, you need branch protection rule:
  - "Require review from Code Owners" alone requires any CODEOWNER
  - Combined with required approvals count for multiple reviews

For requiring BOTH teams specifically:

1. Enable "Require review from Code Owners" in branch protection
2. Set "Required approvals" to 2
3. Each team must provide at least one approval
</details>

**Question 3:** A development team is experiencing frequent merge conflicts because developers work on long-lived feature branches. What branching strategy would you recommend to reduce this friction?

<details markdown="1">
<summary>View Answer</summary>

**Recommend: Trunk-Based Development or GitHub Flow**

For teams experiencing merge conflicts from long-lived branches:

1. **GitHub Flow** (moderate change):
   - Short-lived feature branches (1-3 days max)
   - Merge to main frequently
   - Deploy after each merge
   
2. **Trunk-Based Development** (significant change):
   - Very short-lived branches (< 1 day)
   - Feature flags for incomplete work
   - Continuous integration to main

Migration steps:

1. Establish CI/CD pipeline first
2. Implement feature flags
3. Set branch lifetime policies
4. Educate team on smaller, incremental changes

Supporting GitHub features:

- Require "up to date with base" in status checks
- Auto-merge when checks pass
- Branch auto-deletion after merge
</details>

**Question 4:** What happens when multiple CODEOWNERS patterns match the same file?

<details markdown="1">
<summary>View Answer</summary>

**The last matching pattern wins.**

Example:

```
*                   @default-team
/src/               @dev-team
/src/api/           @api-team
/src/api/auth.py    @security-team

```

For file `/src/api/auth.py`:

- `*` matches → @default-team
- `/src/` matches → @dev-team (overrides)
- `/src/api/` matches → @api-team (overrides)
- `/src/api/auth.py` matches → @security-team (final, wins)

Only `@security-team` is assigned as the code owner.

To require multiple teams, list them on the same line:

```
/src/api/auth.py    @security-team @api-team

```

</details>

**Question 5:** What is the difference between requiring "signed commits" and enabling "vigilant mode" for a user?

<details markdown="1">
<summary>View Answer</summary>

**Signed Commits (Repository/Branch Protection):**

- Requirement at repository level
- Blocks unsigned commits from being merged
- Enforces that all contributors sign their commits
- Shows ✅ Verified badge on signed commits

**Vigilant Mode (User Setting):**

- Personal user preference
- Shows ⚠️ Unverified for unsigned commits FROM THAT USER
- Helps identify if someone might be impersonating you
- Adds warning on commits that could have been signed but weren't

Key difference:

- Signed commits = "Prove who made this commit"
- Vigilant mode = "Flag anything that might not be from me"

Both together provide maximum trust:

- Repository requires signing (enforcement)
- User enables vigilant mode (visibility)
</details>

**Question 6:** A customer has 500 repositories and wants to enforce consistent branch protection across all of them. What's the most efficient approach?

<details markdown="1">
<summary>View Answer</summary>

**Use Organization Rulesets (Enterprise feature)**

Configuration:

1. Go to Organization Settings → Rules → Rulesets
2. Create new ruleset targeting all repositories:

```yaml
Ruleset: Enterprise Standard Protection
Enforcement: Active
Target repositories: All repositories (or pattern: *)
Target branches: 
  - main
  - release/*

Rules:
  - Require pull request (2 approvals)
  - Require status checks
  - Require signed commits
  - Block force pushes

```

Benefits over per-repo configuration:

- Single point of management
- Automatic application to new repos
- Consistent enforcement
- Audit log for changes
- Can't be overridden by repo admins (if desired)

For non-Enterprise customers:

- Use GitHub API/CLI to script branch protection
- Implement a GitHub App to enforce on repo creation
- Use Terraform or similar IaC for GitHub configuration
</details>

**Question 7:** Explain the difference between "Require branches to be up to date before merging" and "Require linear history."

<details markdown="1">
<summary>View Answer</summary>

**Require branches to be up to date before merging:**

- Feature branch must include all commits from base branch
- Prevents merging outdated branches
- Requires rebasing or merging main into feature branch first
- Ensures status checks run against final merged state

```
main:    A--B--C--D
feature: A--B--X--Y  (missing C, D - must update first)

After update:
feature: A--B--C--D--X--Y  ✓ Can now merge

```

**Require linear history:**

- Prevents merge commits on the protected branch
- Only allows "Squash and merge" or "Rebase and merge"
- Creates cleaner, linear commit history
- No merge bubbles in git log

```
Without linear history:
A--B--C--D--E--G (merge commit)
        \     /
         X--Y

With linear history:
A--B--C--D--E--X'--Y'  (squashed or rebased)

```

Combined effect:

- Up to date + Linear = Must rebase feature branch onto latest main, then squash/rebase merge
</details>
