---
layout: training-module
title: "Module 7, Section 7: Real-World CSM Scenarios"
description: "Practice handling customer questions about registry consolidation, supply chain security, and package sharing"
permalink: /intermediate/module-7/scenarios/
module_number: 7
section_number: 7
phase: intermediate
prev_section:
  url: /intermediate/module-7/resources/
  title: "Resources"
next_section:
  url: /intermediate/module-8/
  title: "Module 8: CI/CD Pipelines & Deployment"
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

## Scenario 1: Registry Consolidation
**Situation:**
A customer uses Artifactory for Maven, npm registry for JavaScript, and Docker Hub for containers. They want to consolidate to reduce costs and complexity.
**Customer Question:**
"We're paying for three separate registries. Can GitHub Packages replace all of them?"
**Your Response:**
"GitHub Packages can replace all three for most use cases. Let me break down the migration:
**Feature Comparison:**

| Feature | Artifactory | GitHub Packages |
|---------|-------------|-----------------|
| npm registry | ✅ | ✅ |
| Maven registry | ✅ | ✅ |
| Container registry | ✅ | ✅ (GHCR) |
| Virtual repos | ✅ | ✅ (proxy configs) |
| License scanning | ✅ | Via Dependabot |
| Build info | ✅ | Via attestations |
**Migration Plan:**

1. **Phase 1: Containers (Week 1-2)**
   - Lowest risk, easiest migration
   - Update CI to push to GHCR
   - Update deployments to pull from GHCR
   
2. **Phase 2: npm (Week 3-4)**
   - Update .npmrc files
   - Publish new versions to GitHub
   - Keep old versions in npm for backward compatibility
3. **Phase 3: Maven (Week 5-6)**
   - Update pom.xml distribution management
   - Configure Maven settings.xml
   - Test with internal consumers
**Cost Savings:**

- GitHub Packages included with GitHub Enterprise
- No additional per-artifact storage fees
- Single credential management
**What you might miss:**

- Advanced caching/proxying features
- Virtual repository aggregation
- Detailed analytics
Would you like me to help plan a pilot migration?"
---

## Scenario 2: Supply Chain Security
**Situation:**
After a supply chain attack in the industry, a customer's security team is concerned about package integrity.
**Customer Question:**
"How can we ensure the packages we're using haven't been tampered with?"
**Your Response:**
"Excellent security focus! Here's a comprehensive supply chain security strategy:
**1. Package Provenance (Your Packages):**

```yaml
# Add attestation to your container builds
- name: Generate provenance
  uses: actions/attest-build-provenance@v1
  with:
    subject-name: ghcr.io/${{ github.repository }}
    subject-digest: ${{ steps.push.outputs.digest }}
    push-to-registry: true

```

**2. Verify Packages You Consume:**

```bash
# Verify container provenance
gh attestation verify ghcr.io/vendor/image:tag \
  --owner vendor
# npm package integrity (automatic via lock files)
npm ci --ignore-scripts  # Install with integrity check, no scripts

```

**3. Dependency Controls:**

```yaml
# dependabot.yml - only allow verified sources
version: 2
updates:
  - package-ecosystem: npm
    directory: "/"
    schedule:
      interval: daily
    # Dependabot verifies package integrity automatically

```

**4. Lock File Enforcement:**

```yaml
# CI check for lock file changes
- name: Check for lock file changes
  run: |
    npm ci
    git diff --exit-code package-lock.json

```

**5. SBOM Generation:**

```yaml
- name: Generate SBOM
  uses: anchore/sbom-action@v0
  with:
    image: ghcr.io/${{ github.repository }}:${{ github.sha }}
    artifact-name: sbom.spdx.json

```

**6. Private Registry Proxy:**

- Configure GitHub Packages to proxy public registries
- All packages go through your controlled endpoint
- Enables auditing and blocking
**Defense in Depth:**

- ✅ Dependabot alerts for vulnerabilities
- ✅ Lock files for reproducible builds
- ✅ Attestations for provenance
- ✅ Signature verification for containers
- ✅ SBOM for transparency
Want me to help implement any of these controls?"

## Scenario 3: Multi-Team Package Sharing
**Situation:**
An organization has 50 development teams. They want to share internal libraries but struggle with discoverability and access control.
**Customer Question:**
"Our teams keep reinventing the wheel because they can't find existing internal packages. How do we improve this?"
**Your Response:**
"This is a common challenge with internal packages. Here's a comprehensive solution:
**1. Organization-Level Package Discovery:**

```
github.com/orgs/YOUR-ORG/packages
All packages visible in one place:
├── @org/shared-auth
├── @org/logging-utils
├── @org/ui-components
├── ghcr.io/org/base-images
└── ...

```

**2. Internal Package Registry (InnerSource):**

- Create a `packages` or `registry` repository
- Catalog all internal packages
- Include documentation and examples

```markdown
# Internal Package Registry
## JavaScript Libraries

| Package | Description | Team | Version |
|---------|-------------|------|---------|
| @org/auth | Authentication utilities | Platform | 2.1.0 |
| @org/logging | Structured logging | Platform | 1.5.0 |
## Container Base Images

| Image | Purpose | Team |
|-------|---------|------|
| ghcr.io/org/node-base | Node.js base image | Platform |
| ghcr.io/org/python-base | Python base image | Platform |

```

**3. Template Repositories:**

- Create starter templates that include internal packages
- `gh repo create --template org/node-template`
**4. Access Control Strategy:**

```
Organization Packages:
├── Public to org - All teams can read
├── Team-specific - Only that team
└── Core libraries - Everyone reads, platform team writes

```

**5. Package Announcements:**

- Use GitHub Discussions in registry repo
- Announce new packages and major updates
- Gather feedback from consumers
**6. Automated Documentation:**

```yaml
# Generate and publish docs automatically
- name: Generate docs
  run: npm run docs
- name: Deploy to GitHub Pages
  uses: peaceiris/actions-gh-pages@v3
  with:
    github_token: ${{ secrets.GITHUB_TOKEN }}
    publish_dir: ./docs

```

**Adoption Metrics:**

- Track package downloads
- Survey teams on discovery experience
- Measure duplicate code reduction
Would you like help setting up the registry repository?"
---

## Module Summary

### Key Takeaways
1. **GitHub Packages Supports Multiple Ecosystems**
   - npm, Maven, NuGet, RubyGems, Containers
   - Unified authentication with GITHUB_TOKEN
   - Integrated with repository permissions
2. **GHCR is Purpose-Built for Containers**
   - ghcr.io/owner/image naming
   - Multi-arch support
   - Dependency scanning integration
3. **Versioning Strategy Matters**
   - Semantic versioning for libraries
   - Multi-tag strategy for containers
   - Never use :latest in production
4. **Supply Chain Security is Essential**
   - Generate and verify attestations
   - Use lock files consistently
   - Scan dependencies continuously
5. **Automation is Key**
   - Publish on release events
   - Auto-cleanup old versions
   - Integrate with CI/CD

### What's Next
In **Module 8: CI/CD Pipelines & Deployment**, you'll learn:

- Advanced deployment strategies
- Environment management
- Deployment protection rules
- Rollback procedures
The packages you've learned to manage here become deployment artifacts in your CI/CD pipelines.
**Checklist: Module 7 Complete**
- [ ] Published an npm package to GitHub Packages
- [ ] Built and pushed a container to GHCR
- [ ] Configured multi-architecture builds
- [ ] Set up package cleanup automation
- [ ] Generated package attestations
- [ ] Completed all three labs
- [ ] Reviewed real-world scenarios
{% endraw %}
