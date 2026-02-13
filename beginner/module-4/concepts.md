---
layout: training-module
title: "Core Concepts"
permalink: /beginner/module-4/concepts/
module_number: 4
module_title: "SCM Process"
section_number: 2
total_sections: 7
phase: beginner
estimated_time: "35 min"
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
  url: /beginner/module-4/overview/
  title: "Context & Overview"
next_section:
  url: /beginner/module-4/walkthrough/
  title: "Guided Walkthrough"
---

<div class="callout callout-info" markdown="1">
<div class="callout-title">📚 Official Documentation</div>
For comprehensive reference, see <a href="https://docs.github.com/en/pull-requests">Pull Requests Documentation</a> and <a href="https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository">Branch Configuration</a>.
</div>

# Core Concepts

## 2.1 Branching Strategies

### Git Flow
Traditional approach with long-lived branches:

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    main["main\n(production)"] --> develop["develop\n(integration)"]
    develop --> feat1["feature/user-auth"]
    develop --> feat2["feature/payment-system"]
    develop --> release["release/v1.2.0"]
    release --> hotfix["hotfix/security-patch"]
</div>
</div>

**Best for:** Teams with scheduled releases, multiple versions in production

**Considerations:**

- More complex to manage
- Can lead to long-lived branches with merge conflicts
- Clear separation between development and production

### GitHub Flow
Simplified approach with short-lived branches:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    main["main\n(always deployable)"]
    feat["feature/add-login"] -->|PR| main
    fix["fix/button-color"] -->|PR| main
    docs["docs/api-update"] -->|PR| main
</div>
</div>

**Best for:** Teams practicing continuous deployment, SaaS products

**Characteristics:**

- `main` is always production-ready
- Features deployed as soon as merged
- Simple to understand and implement

### Trunk-Based Development
Extreme simplification with very short-lived branches:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    feat["feature/xyz\n(lives less than 1 day)"] -->|PR| main["main"]
</div>
</div>

**Best for:** High-performing teams, microservices, feature flags

**Requirements:**

- Strong automated testing
- Feature flag infrastructure
- High team discipline

## 2.2 Branch Protection Rules

Branch protection prevents direct pushes and enforces review workflows:

### Key Protection Options

| Option | Purpose | Impact |
|--------|---------|--------|
| Require pull request | No direct pushes | Medium friction |
| Require approvals | Peer review enforcement | High quality |
| Dismiss stale reviews | Fresh reviews after changes | Medium friction |
| Require status checks | Automated quality gates | Variable |
| Require signed commits | Identity verification | Low friction |
| Require linear history | Clean git history | Medium friction |
| Include administrators | No bypass for admins | High security |
| Restrict pushes | Limit who can push | High control |

### Status Check Requirements

Status checks are external validations that must pass:

```yaml
# Example: Required status checks from CI
Required Checks:
  - ci/build           # Must compile
  - ci/unit-tests      # Tests must pass
  - ci/lint            # Code style compliance
  - security/snyk      # No new vulnerabilities
  - coverage/codecov   # Coverage threshold met

```

## 2.3 CODEOWNERS

CODEOWNERS automatically assigns reviewers based on file paths:

```
# CODEOWNERS file (in .github/, root, or docs/)

# Global owners (fallback)
*                       @org/platform-team

# Directory owners
/src/api/               @org/api-team
/src/frontend/          @org/frontend-team
/infrastructure/        @org/devops-team

# File pattern owners
*.sql                   @org/database-team @org/security-team
*.tf                    @org/infrastructure-team
Dockerfile*             @org/platform-team

# Specific file owners
/src/auth/              @security-lead @org/security-team
/.github/workflows/     @org/devops-team
package.json            @org/frontend-team

```

### CODEOWNERS Syntax Deep Dive

```
# Pattern matching examples
docs/*                  # Files in docs/ (not subdirectories)
docs/**                 # All files in docs/ and subdirectories
*.js                    # All JavaScript files anywhere
/scripts/               # Only top-level scripts directory
**/logs/                # Any logs directory at any level

# Multiple owners (any can approve)
/critical/              @owner1 @owner2 @org/team

# Order matters - last matching pattern wins
*                       @default-team
/src/                   @dev-team       # Overrides * for /src/
/src/security/          @security-team  # Overrides /src/ for /src/security/

```

## 2.4 Repository Rulesets

Rulesets provide more flexible, scalable governance than branch protection:

### Rulesets vs Branch Protection

| Feature | Branch Protection | Rulesets |
|---------|------------------|----------|
| Scope | Single branch pattern | Multiple patterns |
| Management | Per-repository | Org or repo level |
| Bypass permissions | All or nothing | Granular actor bypass |
| Tag rules | Limited | Full support |
| Import/Export | No | Yes (API) |
| Status | Legacy (still supported) | Modern approach |

### Ruleset Structure

```yaml
Ruleset: "Production Protection"
Enforcement: Active
Bypass List:
  - Release Automation Bot
  - Emergency Response Team

Target Branches:
  - main
  - release/*

Rules:
  - Require pull request
      Required approvals: 2
      Dismiss stale reviews: true
      Require review from CODEOWNERS: true
  - Require status checks
      Checks: [ci/build, ci/test, security/scan]
      Require branches up to date: true
  - Require signed commits
  - Block force pushes
  - Require linear history

```

### Merge Queue

Merge queue ensures that pull requests are always tested against the latest base branch before merging. This prevents "semantic merge conflicts" — situations where two PRs pass CI independently but break when combined.

**How it works:**

1. PR is approved and added to the merge queue
2. GitHub creates a temporary branch with the PR changes merged on top of the latest base branch (plus any PRs ahead in the queue)
3. CI runs against this combined result
4. If CI passes, the PR is merged; if it fails, the PR is removed from the queue

**When to use merge queue:**

- High-traffic repositories where multiple PRs merge daily
- Projects where CI must pass on the exact merged result
- Teams that want to eliminate "green PR, red main" situations

Merge queue is configured through **repository rulesets** by adding the "Require merge queue" rule.

> **💡 CSM Insight:** Merge queue is especially valuable for customers with large monorepos or high-velocity teams. It eliminates the manual "update branch, wait for CI, merge" cycle that slows down development.

## 2.5 Commit Signing

Commit signing cryptographically verifies commit authorship:

### Why Sign Commits?

```
Unsigned commit:
  Author: John Smith <john@company.com>
  Status: Unknown - anyone could have set this email

Signed commit:
  Author: John Smith <john@company.com>
  Signature: Verified ✓
  Key: GPG/SSH key registered to john@company.com
  Status: Cryptographically proven to be from John

```

### Signing Methods

1. **GPG Signing** - Traditional, widely supported
2. **SSH Signing** - Simpler setup, uses existing SSH keys
3. **S/MIME Signing** - Certificate-based, enterprise PKI integration

### Vigilant Mode

GitHub's vigilant mode shows verification status:

- ✅ **Verified** - Signature valid, key belongs to committer
- ⚠️ **Partially verified** - Signature valid, key not linked to account
- ❌ **Unverified** - No signature or invalid

## 2.6 Release Management

GitHub Releases provide version packaging and distribution:

### Release Components

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph Release["Release: v2.1.0"]
        direction TB
        Tag["Tag: v2.1.0\n(git tag pointing to commit)"]
        Title["Title: Version 2.1.0 - Performance Release"]
        Notes["Release Notes\n(markdown description)"]
        subgraph Assets["Assets"]
            A1["app-2.1.0-linux-x64.tar.gz"]
            A2["app-2.1.0-macos-x64.tar.gz"]
            A3["app-2.1.0-windows-x64.zip"]
            A4["checksums.txt"]
        end
        PreRelease["Pre-release: false"]
        Latest["Latest: true"]
    end
</div>
</div>

### Semantic Versioning

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    SemVer["MAJOR.MINOR.PATCH\nExample: v2.1.0"] --> Major["MAJOR\nBreaking changes"]
    SemVer --> Minor["MINOR\nNew features\n(backward compatible)"]
    SemVer --> Patch["PATCH\nBug fixes\n(backward compatible)"]
    PreRelease["Pre-release versions"] --> Alpha["v2.1.0-alpha.1"]
    PreRelease --> Beta["v2.1.0-beta.2"]
    PreRelease --> RC["v2.1.0-rc.1"]
</div>
</div>

### Auto-Generated Release Notes

GitHub can automatically generate release notes from:

- Merged pull requests
- Associated issues
- Commit messages
- Contributors

Configuration in `.github/release.yml`:

```yaml
changelog:
  exclude:
    labels:
      - skip-changelog
    authors:
      - dependabot
  categories:
    - title: 🚀 Features
      labels:
        - enhancement
        - feature
    - title: 🐛 Bug Fixes
      labels:
        - bug
        - fix
    - title: 📚 Documentation
      labels:
        - documentation
    - title: 🔒 Security
      labels:
        - security
    - title: Other Changes
      labels:
        - "*"

```
