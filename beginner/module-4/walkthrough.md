---
layout: training-module
title: "Guided Walkthrough"
permalink: /beginner/module-4/walkthrough/
module_number: 4
module_title: "SCM Process"
section_number: 3
total_sections: 7
phase: beginner
estimated_time: "40 min"
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
  url: /beginner/module-4/concepts/
  title: "Core Concepts"
next_section:
  url: /beginner/module-4/labs/
  title: "Hands-On Labs"
---

# Guided Walkthrough

## Walkthrough 1: Configuring Branch Protection

Let's set up comprehensive protection for a production branch.

**Step 1: Navigate to Branch Protection**
1. Go to your repository on GitHub.com
2. Click **Settings** → **Branches**
3. Under "Branch protection rules", click **Add rule**

**Step 2: Define the Branch Pattern**

```
Branch name pattern: main

This will protect:
✓ main
✗ main-backup (doesn't match exactly)
✗ feature/main (doesn't match)

```

For multiple branches, use patterns:
- `release/*` - All release branches
- `main` - Exact match
- `*` - All branches (careful!)

**Step 3: Configure Protection Options**

```
☑ Require a pull request before merging
  ☑ Require approvals: 2
  ☑ Dismiss stale pull request approvals when new commits are pushed
  ☑ Require review from Code Owners
  ☑ Require approval of the most recent reviewable push

☑ Require status checks to pass before merging
  ☑ Require branches to be up to date before merging
  Status checks that are required:
    ├── ci/build
    ├── ci/test
    └── security/codeql

☑ Require conversation resolution before merging

☑ Require signed commits

☑ Require linear history

☐ Do not allow bypassing the above settings
  (Enable this for highest security)

☑ Restrict who can push to matching branches
  Allowed: @release-team

```

**Step 4: Save and Test**
1. Click **Create** or **Save changes**
2. Try pushing directly to main - should fail
3. Create a PR - protection rules apply

## Walkthrough 2: Creating a CODEOWNERS File

**Step 1: Create the File**

Create `.github/CODEOWNERS`:

```
# Default owners for everything
*                           @myorg/engineering-leads

# Frontend code
/src/components/            @myorg/frontend-team
/src/styles/                @myorg/frontend-team @myorg/design-team
*.css                       @myorg/frontend-team
*.tsx                       @myorg/frontend-team

# Backend code
/src/api/                   @myorg/backend-team
/src/services/              @myorg/backend-team
*.py                        @myorg/backend-team

# Infrastructure
/terraform/                 @myorg/platform-team
/kubernetes/                @myorg/platform-team
/.github/workflows/         @myorg/platform-team
Dockerfile                  @myorg/platform-team

# Security-sensitive files (multiple required reviewers)
/src/auth/                  @security-lead @myorg/security-team
/src/crypto/                @security-lead @myorg/security-team
*.pem                       @security-lead
*.key                       @security-lead

# Documentation
/docs/                      @myorg/docs-team
*.md                        @myorg/docs-team
README.md                   @myorg/engineering-leads

# Configuration files
package.json                @myorg/frontend-team
requirements.txt            @myorg/backend-team
.env.example                @myorg/platform-team @myorg/security-team

```

**Step 2: Enable CODEOWNERS Requirement**

In branch protection rules:

```
☑ Require review from Code Owners

```

**Step 3: Verify CODEOWNERS is Working**

1. Create a PR that modifies `/src/api/endpoint.py`
2. GitHub should automatically request review from `@myorg/backend-team`
3. The PR cannot be merged until a CODEOWNER approves

## Walkthrough 3: Setting Up Repository Rulesets

**Step 1: Navigate to Rulesets**
1. Repository **Settings** → **Rules** → **Rulesets**
2. Click **New ruleset** → **New branch ruleset**

**Step 2: Configure Ruleset Basics**

```
Ruleset Name: Production Branch Protection
Enforcement status: Active
Bypass list:
  ☑ Repository admin
  ☑ Specific actors:
      - github-actions[bot]
      - release-automation

```

**Step 3: Define Target Branches**

```
Target branches:
  ☑ Add target → Include by pattern
      Pattern: main
  ☑ Add target → Include by pattern
      Pattern: release/*

```

**Step 4: Configure Rules**

```
☑ Restrict creations
☑ Restrict deletions
☑ Restrict updates
☑ Block force pushes

☑ Require pull request before merging
    Required approvals: 2
    Dismiss stale reviews: ☑
    Require review from CODEOWNERS: ☑
    Require last push approval: ☑

☑ Require status checks to pass
    Required checks:
      - build
      - test
      - security-scan
    ☑ Require branches to be up to date

☑ Require signed commits
☑ Require linear history

```

**Step 5: Create Organization Ruleset (Enterprise)**

For organization-wide rules:
1. Go to **Organization Settings** → **Rules** → **Rulesets**
2. Create ruleset with target repositories:
   ```
   Target repositories:
     ☑ All repositories
     OR
     ☑ Include by pattern: *-production
   ```

## Walkthrough 4: Setting Up Commit Signing

### Option A: SSH Key Signing (Recommended)

**Step 1: Configure Git to Use SSH Signing**

```bash
# Set SSH signing format
git config --global gpg.format ssh

# Specify your SSH signing key
git config --global user.signingkey ~/.ssh/id_ed25519.pub

# Enable automatic signing
git config --global commit.gpgsign true
git config --global tag.gpgsign true

```

**Step 2: Add Signing Key to GitHub**
1. Go to **Settings** → **SSH and GPG keys**
2. Click **New SSH key**
3. Select **Signing Key** as Key type
4. Paste your public key

**Step 3: Create Allowed Signers File (for local verification)**

```bash
# Create allowed_signers file
echo "your.email@company.com $(cat ~/.ssh/id_ed25519.pub)" > ~/.ssh/allowed_signers

# Configure git to use it
git config --global gpg.ssh.allowedSignersFile ~/.ssh/allowed_signers

```

### Option B: GPG Signing

**Step 1: Generate GPG Key**

```bash
# Generate new GPG key
gpg --full-generate-key

# List keys to get ID
gpg --list-secret-keys --keyid-format=long

# Output shows:
# sec   ed25519/ABC123DEF456 2024-01-01
#       ABC123DEF456 is your key ID

```

**Step 2: Configure Git**

```bash
git config --global user.signingkey ABC123DEF456
git config --global commit.gpgsign true

```

**Step 3: Export and Add to GitHub**

```bash
# Export public key
gpg --armor --export ABC123DEF456

# Add to GitHub Settings → SSH and GPG keys → New GPG key

```

## Walkthrough 5: Creating a GitHub Release

**Step 1: Prepare Release Configuration**

Create `.github/release.yml`:

```yaml
changelog:
  exclude:
    labels:
      - skip-changelog
      - dependencies
    authors:
      - dependabot[bot]
      - github-actions[bot]
  categories:
    - title: 🎉 New Features
      labels:
        - enhancement
        - feature
    - title: 🐛 Bug Fixes
      labels:
        - bug
        - bugfix
    - title: ⚡ Performance
      labels:
        - performance
    - title: 🔒 Security
      labels:
        - security
    - title: 📚 Documentation
      labels:
        - documentation
        - docs
    - title: 🧰 Maintenance
      labels:
        - chore
        - maintenance
        - refactor
    - title: Other Changes
      labels:
        - "*"

```

**Step 2: Create Release via UI**

1. Go to repository → **Releases** → **Draft a new release**
2. Click **Choose a tag** → Enter `v1.2.0` → **Create new tag**
3. Set **Target** to `main` (or specific commit)
4. Enter **Release title**: `v1.2.0 - Feature Release`
5. Click **Generate release notes** to auto-populate
6. Edit release notes as needed
7. Attach binary assets (drag and drop)
8. Choose release type:
   - ☑ Set as the latest release
   - ☐ Set as a pre-release
9. Click **Publish release**

**Step 3: Create Release via CLI**

```bash
# Using GitHub CLI
gh release create v1.2.0 \
  --title "v1.2.0 - Feature Release" \
  --notes-file CHANGELOG.md \
  --target main \
  ./dist/app-linux.tar.gz \
  ./dist/app-macos.tar.gz \
  ./dist/app-windows.zip

# Create pre-release
gh release create v1.3.0-beta.1 \
  --title "v1.3.0 Beta 1" \
  --prerelease \
  --generate-notes

```
