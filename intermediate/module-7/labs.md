---
layout: training-module
title: "Module 7, Section 4: Hands-On Labs"
description: "Practice publishing npm packages, containers, and setting up private package sharing"
permalink: /intermediate/module-7/labs/
module_number: 7
section_number: 4
phase: intermediate
prev_section:
  url: /intermediate/module-7/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /intermediate/module-7/quiz/
  title: "Knowledge Check"
toc: true
module_title: "Dependency & Package Management"
total_sections: 7
module_index: /intermediate/module-7/
sections:
  - title: "Context & Overview"
    url: "/intermediate/module-7/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "20 min"
  - title: "Core Concepts"
    url: "/intermediate/module-7/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/intermediate/module-7/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "50 min"
  - title: "Hands-On Labs"
    url: "/intermediate/module-7/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/intermediate/module-7/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "25 min"
  - title: "Resources"
    url: "/intermediate/module-7/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/intermediate/module-7/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "30 min"
---
{% raw %}

## Lab 1: npm Package Publishing Pipeline
**Objective:** Create a complete npm package with automated publishing
**Tasks:**
1. **Create a new npm package:**

```bash
mkdir my-utility && cd my-utility
npm init -y

```

2. **Add package content:**

```javascript
// src/index.js
export function greet(name) {
  return `Hello, ${name}!`;
}
export function formatDate(date) {
  return new Intl.DateTimeFormat('en-US').format(date);
}

```

3. **Configure for GitHub Packages:**
   - Update package.json with scope
   - Add publishConfig
   - Create .npmrc
4. **Create publish workflow:**
   - Trigger on release
   - Run tests before publish
   - Publish to GitHub Packages
5. **Create version bump workflow:**

```yaml
name: Version Bump
on:
  workflow_dispatch:
    inputs:
      bump:
        description: 'Version bump type'
        required: true
        type: choice
        options:
          - patch
          - minor
          - major

```

**Validation:**
- [ ] Package publishes on release
- [ ] Package is downloadable via npm
- [ ] Version bump creates PR with updated version
---

## Lab 2: Container Registry Workflow
**Objective:** Build a complete container workflow with multi-stage builds and security scanning
**Tasks:**
1. **Create a Node.js application:**

```javascript
// index.js
const express = require('express');
const app = express();
app.get('/health', (req, res) => res.json({ status: 'healthy' }));
app.get('/', (req, res) => res.json({ message: 'Hello World' }));
app.listen(3000);

```

2. **Create optimized Dockerfile:**
   - Multi-stage build
   - Non-root user
   - Health check
   - Proper layer caching
3. **Create container workflow:**
   - Build on PR (no push)
   - Push on main and tags
   - Generate SBOM
   - Scan for vulnerabilities
4. **Add vulnerability scanning:**

```yaml
- name: Scan for vulnerabilities
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
    format: 'sarif'
    output: 'trivy-results.sarif'
    
- name: Upload scan results
  uses: github/codeql-action/upload-sarif@v3
  with:
    sarif_file: 'trivy-results.sarif'

```

**Validation:**
- [ ] Container builds on PR
- [ ] Container pushes on main
- [ ] Multiple tags generated (sha, latest, semver)
- [ ] Vulnerabilities reported to Security tab

## Lab 3: Private Package Sharing
**Objective:** Set up cross-repository package consumption
**Tasks:**
1. **Create a shared library repository:**
   - Create `shared-utils` package
   - Publish to GitHub Packages
2. **Create a consumer repository:**
   - Depend on `shared-utils`
   - Configure authentication
3. **Set up organization-level authentication:**

```yaml
# In consuming repo
- name: Setup npm for GitHub Packages
  run: |
    echo "@your-org:registry=https://npm.pkg.github.com" >> .npmrc
    echo "//npm.pkg.github.com/:_authToken=${{ secrets.PACKAGES_TOKEN }}" >> .npmrc

```

4. **Test private package consumption:**
   - Verify install works in CI
   - Verify build succeeds
   - Document authentication setup
**Validation:**
- [ ] Private package published
- [ ] Consumer can install package in CI
- [ ] Documentation complete
{% endraw %}
