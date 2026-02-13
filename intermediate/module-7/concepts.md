---
layout: training-module
title: "Module 7, Section 2: Core Concepts"
description: "Learn about package ecosystems, authentication, versioning, and supply chain security"
permalink: /intermediate/module-7/concepts/
module_number: 7
section_number: 2
phase: intermediate
prev_section:
  url: /intermediate/module-7/overview/
  title: "Context & Overview"
next_section:
  url: /intermediate/module-7/walkthrough/
  title: "Guided Walkthrough"
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

<div class="callout callout-info" markdown="1">
<div class="callout-title">📦 Official Documentation</div>
For comprehensive reference, see <a href="https://docs.github.com/en/packages">GitHub Packages Documentation</a> and the <a href="https://docs.github.com/en/packages/working-with-a-github-packages-registry">Package Registry Guides</a>.
</div>

## 2.1 Supported Package Ecosystems

| Ecosystem | Registry URL | Package Format |
|-----------|-------------|----------------|
| npm | npm.pkg.github.com | @owner/package |
| Maven | maven.pkg.github.com | com.owner.package |
| Gradle | maven.pkg.github.com | com.owner.package |
| NuGet | nuget.pkg.github.com | Owner.Package |
| RubyGems | rubygems.pkg.github.com | owner-package |
| Containers | ghcr.io | ghcr.io/owner/image |

---

## 2.2 Authentication

### Personal Access Token (Classic)

```bash
# Create PAT with packages permissions
# read:packages - Download packages
# write:packages - Upload packages
# delete:packages - Delete packages
# npm authentication
npm login --registry=https://npm.pkg.github.com
Username: YOUR_GITHUB_USERNAME
Password: YOUR_PAT
Email: your@email.com
# Or use .npmrc
echo "//npm.pkg.github.com/:_authToken=YOUR_PAT" >> ~/.npmrc

```

### GITHUB_TOKEN in Actions

```yaml
jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      packages: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      
      - name: Login to GitHub Packages
        run: echo "${{ secrets.GITHUB_TOKEN }}" | npm login --registry=https://npm.pkg.github.com --username ${{ github.actor }}

```

## 2.3 GitHub Container Registry (GHCR)
GHCR is GitHub's container registry:

```
ghcr.io/OWNER/IMAGE:TAG
Examples:
ghcr.io/acme/api-server:latest
ghcr.io/acme/api-server:v1.2.3
ghcr.io/acme/api-server:sha-abc1234

```

### Key Features

| Feature | Description |
|---------|-------------|
| Public/Private | Control visibility per package |
| Organization scoping | Inherit org permissions |
| Repository linking | Link to source repository |
| Immutable tags | Prevent tag overwriting |
| Vulnerability scanning | Dependabot for containers |
| Multi-arch support | AMD64, ARM64, etc. |

---

## 2.4 Package Visibility

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph VIS["📦 Package Visibility"]
        subgraph PUBLIC["🌐 Public"]
            P1["Anyone can pull"]
            P2["No authentication required"]
            P3["Good for: Open source libraries"]
        end
        
        subgraph PRIVATE_REPO["🔒 Private (Repository)"]
            R1["Linked to repository permissions"]
            R2["Repo read = package read"]
            R3["Good for: Internal libraries"]
        end
        
        subgraph PRIVATE_ORG["🏢 Private (Organization)"]
            O1["Explicit permission grants"]
            O2["Team-based access control"]
            O3["Good for: Shared org packages"]
        end
    end
</div>
</div>

## 2.5 Package Versioning Strategies

### Semantic Versioning

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    SemVer["MAJOR.MINOR.PATCH\nExample: 1.2.3"] --> Major["MAJOR\nBreaking changes"]
    SemVer --> Minor["MINOR\nNew features,\nbackward compatible"]
    SemVer --> Patch["PATCH\nBug fixes,\nno API changes"]
    PreRelease["Pre-release versions"] --> Alpha["1.2.3-alpha.1"]
    PreRelease --> Beta["1.2.3-beta.2"]
    PreRelease --> RC["1.2.3-rc.1"]
    BuildMeta["Build metadata"] --> B1["1.2.3+build.123"]
    BuildMeta --> B2["1.2.3+20240115"]
</div>
</div>

### Container Tagging Strategies

```
# Semantic versioning
ghcr.io/org/app:1.2.3
ghcr.io/org/app:1.2
ghcr.io/org/app:1
# Git-based
ghcr.io/org/app:main
ghcr.io/org/app:sha-abc1234
ghcr.io/org/app:pr-42
# Environment-based
ghcr.io/org/app:latest
ghcr.io/org/app:staging
ghcr.io/org/app:production

```

---

## 2.6 Supply Chain Security

Modern software relies heavily on open source dependencies, making supply chain security critical. GitHub provides tools to verify the integrity and provenance of packages.

### Software Bill of Materials (SBOM)

An **SBOM** is a comprehensive inventory of all components in your software—think of it as an "ingredients list" for your application. SBOMs help organizations:

- **Track dependencies** - Know exactly what's in your software
- **Respond to vulnerabilities** - Quickly identify affected projects when a CVE is announced
- **Meet compliance requirements** - Many regulations now require SBOM documentation
- **Audit third-party software** - Verify what vendors include in their products

GitHub can automatically generate SBOMs in industry-standard formats like CycloneDX and SPDX:

```json
{
  "bomFormat": "CycloneDX",
  "specVersion": "1.4",
  "components": [
    {
      "type": "library",
      "name": "lodash",
      "version": "4.17.21",
      "purl": "pkg:npm/lodash@4.17.21"
    }
  ]
}

```

### Package Provenance

**Provenance** is cryptographic proof of where a package came from and how it was built. This addresses supply chain attacks where malicious actors publish compromised packages. With provenance, consumers can verify:

- **Source repository** - The package came from the expected repository
- **Build process** - It was built by a trusted CI system (not a developer's laptop)
- **Commit hash** - The exact source code version used
- **Signing** - The package hasn't been tampered with since building

GitHub Actions can automatically generate and sign provenance attestations using Sigstore:

```
Package: my-library@1.2.3
Built by: GitHub Actions
Repository: github.com/org/my-library
Commit: abc1234
Workflow: release.yml
Runner: github-hosted
Signed: Yes (Sigstore)

```

{% endraw %}
