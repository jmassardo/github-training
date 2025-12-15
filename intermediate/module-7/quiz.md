---
layout: training-module
title: "Module 7, Section 5: Knowledge Check"
description: "Test your understanding of GitHub Packages and supply chain security"
permalink: /intermediate/module-7/quiz/
module_number: 7
section_number: 5
phase: intermediate
prev_section:
  url: /intermediate/module-7/labs/
  title: "Hands-On Labs"
next_section:
  url: /intermediate/module-7/resources/
  title: "Resources"
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

## Knowledge Check Questions
**Question 1:** What's the difference between `packages: read` and `packages: write` permissions in GitHub Actions?
<details markdown="1">
<summary>View Answer</summary>
**`packages: read`:**

- Download/pull packages from GitHub Packages
- Required for consuming private packages
- Sufficient for most CI jobs that just need dependencies
**`packages: write`:**

- Upload/push packages to GitHub Packages
- Delete package versions
- Required for publishing workflows
- Includes read permission
**Usage example:**

```yaml
jobs:
  test:
    permissions:
      packages: read     # Just needs to install dependencies
      contents: read
      
  publish:
    permissions:
      packages: write    # Needs to publish package
      contents: read

```

**Best practice:** Use minimum required permissions (principle of least privilege).
</details>
---
**Question 2:** How do you ensure container images are only pulled from trusted sources?
<details markdown="1">
<summary>View Answer</summary>
**Multiple layers of trust:**

1. **Signature verification:**

```bash
# Verify with cosign
cosign verify ghcr.io/org/image:tag \
  --certificate-identity-regexp="https://github.com/org/.*" \
  --certificate-oidc-issuer="https://token.actions.githubusercontent.com"

```

2. **Attestation verification:**

```bash
# Verify build provenance
gh attestation verify ghcr.io/org/image:tag --owner org

```

3. **Repository linking:**
- Link packages to source repositories
- Visibility follows repository permissions
4. **Organization policies:**
- Restrict which registries can be used
- Require signed images
- Block unverified images
5. **Kubernetes admission control:**

```yaml
# Kyverno policy example
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: require-ghcr
spec:
  rules:
    - name: require-ghcr-images
      match:
        resources:
          kinds:
            - Pod
      validate:
        message: "Images must be from ghcr.io/our-org"
        pattern:
          spec:
            containers:
              - image: "ghcr.io/our-org/*"

```

</details>
**Question 3:** A customer wants to publish the same npm package to both GitHub Packages and npmjs.com. How would you configure this?
<details markdown="1">
<summary>View Answer</summary>
**Dual publishing configuration:**

```yaml
name: Publish to Multiple Registries
on:
  release:
    types: [published]
jobs:
  publish-github:
    runs-on: ubuntu-latest
    permissions:
      packages: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          registry-url: 'https://npm.pkg.github.com'
      - run: npm ci
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
  publish-npm:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          registry-url: 'https://registry.npmjs.org'
      - run: npm ci
      - run: npm publish --access public
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}

```

**Package.json consideration:**

```json
{
  "name": "@org/package",  // Scoped for GitHub
  "publishConfig": {
    "access": "public"     // For npmjs.com
  }
}

```

**Alternative approach:** Publish primarily to npmjs.com and use GitHub Packages as a cache/proxy.
</details>
---
**Question 4:** What container tagging strategy would you recommend for a production deployment workflow?
<details markdown="1">
<summary>View Answer</summary>
**Recommended multi-tag strategy:**

```yaml
- name: Extract metadata
  id: meta
  uses: docker/metadata-action@v5
  with:
    images: ghcr.io/${{ github.repository }}
    tags: |
      # Semantic version tags (on tag push)
      type=semver,pattern={{version}}
      type=semver,pattern={{major}}.{{minor}}
      type=semver,pattern={{major}},enable=${{ !startsWith(github.ref, 'refs/tags/v0.') }}
      
      # Branch-based tags
      type=ref,event=branch
      
      # SHA-based (immutable reference)
      type=sha,prefix=sha-
      
      # Latest only on main branch tags
      type=raw,value=latest,enable=${{ github.ref == format('refs/heads/{0}', 'main') }}

```

**Results in tags like:**

- `ghcr.io/org/app:1.2.3` - Specific version
- `ghcr.io/org/app:1.2` - Minor version (auto-updates)
- `ghcr.io/org/app:1` - Major version (auto-updates)
- `ghcr.io/org/app:sha-abc1234` - Immutable
- `ghcr.io/org/app:latest` - Latest stable
**For production:**

- **Development:** Use `:main` or `:sha-*` tags
- **Staging:** Use `:sha-*` or pre-release tags
- **Production:** Use semantic version tags (`:1.2.3`)
**Never use `:latest` in production** - it's mutable and unpredictable.
</details>
**Question 5:** How do you handle private package authentication in a monorepo with multiple package ecosystems?
<details markdown="1">
<summary>View Answer</summary>
**Monorepo authentication setup:**

```yaml
jobs:
  setup:
    runs-on: ubuntu-latest
    permissions:
      packages: write
      contents: read
    
    steps:
      - uses: actions/checkout@v4
      
      # npm packages
      - name: Configure npm
        run: |
          echo "@org:registry=https://npm.pkg.github.com" >> .npmrc
          echo "//npm.pkg.github.com/:_authToken=${{ secrets.GITHUB_TOKEN }}" >> .npmrc
      
      # Maven packages
      - name: Configure Maven
        run: |
          mkdir -p ~/.m2
          cat > ~/.m2/settings.xml << 'EOF'
          <settings>
            <servers>
              <server>
                <id>github</id>
                <username>${env.GITHUB_ACTOR}</username>
                <password>${env.GITHUB_TOKEN}</password>
              </server>
            </servers>
          </settings>
          EOF
      
      # NuGet packages
      - name: Configure NuGet
        run: |
          dotnet nuget add source \
            --username ${{ github.actor }} \
            --password ${{ secrets.GITHUB_TOKEN }} \
            --store-password-in-clear-text \
            --name github \
            "https://nuget.pkg.github.com/ORG/index.json"
      
      # Container registry
      - name: Login to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

```

**Tip:** Create a composite action for reuse across workflows.
</details>
{% endraw %}
