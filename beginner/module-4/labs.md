---
layout: training-module
title: "Hands-On Labs"
permalink: /beginner/module-4/labs/
module_number: 4
module_title: "SCM Process"
section_number: 4
total_sections: 7
phase: beginner
estimated_time: "45 min"
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
  url: /beginner/module-4/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /beginner/module-4/quiz/
  title: "Knowledge Check"
---

# Hands-On Labs

## Lab 1: Implement a Complete Branch Protection Strategy

**Objective:** Configure branch protection for a multi-environment project

**Setup:**

```bash
# Create or use existing repository
gh repo create scm-lab --public --clone
cd scm-lab

# Create branch structure
git checkout -b develop
echo "# Development" > README.md
git add . && git commit -m "Initial develop branch"
git push -u origin develop

git checkout -b staging
echo "# Staging" >> README.md
git add . && git commit -m "Staging branch setup"
git push -u origin staging

git checkout main

```

**Tasks:**

1. **Configure `main` branch (Production)**
   - Require 2 approvals
   - Require `ci/build`, `ci/test`, `security/scan` status checks
   - Require signed commits
   - Require linear history
   - No force pushes
   - Restrict pushes to release team only

2. **Configure `staging` branch**
   - Require 1 approval
   - Require `ci/build`, `ci/test` status checks
   - Allow repository admins to bypass

3. **Configure `develop` branch**
   - Require pull requests (no direct pushes)
   - Require `ci/lint` status check
   - Allow force pushes from maintainers only

4. **Test your configuration**
   - Try to push directly to main (should fail)
   - Create a PR from feature branch to develop
   - Verify required checks appear

**Validation:**

```bash
# Should fail
git checkout main
echo "test" > test.txt
git add . && git commit -m "Direct push"
git push origin main

# Output: remote: error: GH006: Protected branch update failed

```

## Lab 2: Design and Implement CODEOWNERS

**Objective:** Create a CODEOWNERS structure for a full-stack application

**Scenario:** Your application has:
- Frontend (React) in `/src/frontend/`
- Backend (Python) in `/src/backend/`
- Infrastructure (Terraform) in `/infrastructure/`
- Shared types in `/src/shared/`
- CI/CD in `/.github/`
- Documentation in `/docs/`
- Security modules in `/src/*/security/`

**Teams:**

- `@acme/frontend-team`
- `@acme/backend-team`
- `@acme/platform-team`
- `@acme/security-team`
- `@acme/tech-leads`

**Tasks:**

1. **Create the repository structure:**

```bash
mkdir -p src/{frontend,backend,shared}
mkdir -p src/frontend/security src/backend/security
mkdir -p infrastructure docs .github/workflows

```

2. **Create CODEOWNERS file:**

```
# Your task: Create .github/CODEOWNERS that:
# - Has @acme/tech-leads as default owners
# - Frontend team owns /src/frontend/ and all .tsx/.css files
# - Backend team owns /src/backend/ and all .py files
# - Platform team owns /infrastructure/ and /.github/
# - Security team must review ANY security directory
# - Shared code requires frontend AND backend team review
# - All Dockerfiles require platform AND security team

```

3. **Test with sample files:**

```bash
# Create test files
echo "component" > src/frontend/App.tsx
echo "api" > src/backend/main.py
echo "module" > infrastructure/main.tf
echo "types" > src/shared/types.ts
echo "auth" > src/backend/security/auth.py

```

4. **Create a PR and verify owners are assigned correctly**

**Expected Owners:**

| File | Expected Reviewers |
|------|-------------------|
| `src/frontend/App.tsx` | @acme/frontend-team |
| `src/backend/main.py` | @acme/backend-team |
| `infrastructure/main.tf` | @acme/platform-team |
| `src/shared/types.ts` | @acme/frontend-team @acme/backend-team |
| `src/backend/security/auth.py` | @acme/security-team |

## Lab 3: Release Management Workflow

**Objective:** Implement a complete release workflow with automation

**Tasks:**

1. **Create release configuration:**

```yaml
# .github/release.yml
# Configure categories for your changelog

```

2. **Create release workflow:**

```yaml
# .github/workflows/release.yml
name: Release

on:
  push:
    tags:
      - 'v*.*.*'

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Build artifacts
        run: |
          # Build commands here
          mkdir dist
          echo "Build output" > dist/app.txt
          tar -czf dist/app-linux.tar.gz dist/app.txt
          
      - name: Create Release
        uses: softprops/action-gh-release@v1
        with:
          generate_release_notes: true
          files: |
            dist/app-linux.tar.gz

```

3. **Implement version bumping:**

```bash
# Create release script
#!/bin/bash
# scripts/release.sh

VERSION=$1
if [ -z "$VERSION" ]; then
  echo "Usage: ./release.sh v1.2.3"
  exit 1
fi

# Update version in package.json or similar
# Create and push tag
git tag -a "$VERSION" -m "Release $VERSION"
git push origin "$VERSION"

```

4. **Create releases for versions:**
   - v1.0.0 - Initial release
   - v1.0.1 - Bug fix release
   - v1.1.0 - Feature release
   - v2.0.0-beta.1 - Pre-release

**Validation:**

- Releases appear on repository Releases page
- Auto-generated notes correctly categorize changes
- Build artifacts attached to releases
