---
layout: training-module
title: "Module 7, Section 3: Guided Walkthrough"
description: "Step-by-step guides for publishing npm packages, containers, Maven packages, and more"
permalink: /intermediate/module-7/walkthrough/
module_number: 7
section_number: 3
phase: intermediate
prev_section:
  url: /intermediate/module-7/concepts/
  title: "Core Concepts"
next_section:
  url: /intermediate/module-7/labs/
  title: "Hands-On Labs"
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

## Introduction

In this walkthrough, you'll learn how to publish and consume packages using GitHub Packages. We'll cover the most common package ecosystems: npm for JavaScript/TypeScript, container images with GHCR, and Maven for Java. Each walkthrough builds on real-world patterns you'll encounter when helping customers set up their package management workflows.

> **📚 Official Documentation:** [GitHub Packages Documentation](https://docs.github.com/en/packages)

---

## Walkthrough 1: Publishing npm Packages

npm is the most popular package manager for JavaScript and TypeScript projects. GitHub Packages provides a fully-featured npm registry that integrates seamlessly with your repositories, allowing you to publish private packages scoped to your organization.

### Why Use GitHub Packages for npm?

- **Unified permissions** — Package access follows repository permissions
- **Private packages included** — No separate npm Pro subscription needed
- **CI/CD integration** — `GITHUB_TOKEN` works automatically in Actions
- **Audit trail** — All package publishes are logged

> **📚 Learn more:** [Working with the npm registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry)

### Step 1: Configure package.json

The `publishConfig` setting tells npm to publish to GitHub Packages instead of the public npm registry. The package name must be scoped to your organization (e.g., `@your-org/package-name`).

```json
{
  "name": "@your-org/my-package",
  "version": "1.0.0",
  "description": "My awesome package",
  "main": "dist/index.js",
  "types": "dist/index.d.ts",
  "files": [
    "dist"
  ],
  "repository": {
    "type": "git",
    "url": "https://github.com/your-org/my-package.git"
  },
  "publishConfig": {
    "registry": "https://npm.pkg.github.com"
  }
}

```

### Step 2: Create .npmrc

The `.npmrc` file configures npm to use GitHub Packages for your organization's scoped packages. The `NODE_AUTH_TOKEN` environment variable will be set automatically by the `setup-node` action in CI, or you can set it locally with a Personal Access Token.

```
@your-org:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=${NODE_AUTH_TOKEN}

```

### Step 3: Create Publish Workflow

This workflow automatically publishes your package when you create a GitHub Release. The `permissions` block explicitly grants write access to packages—this is required because the default `GITHUB_TOKEN` permissions may be restricted at the organization level.

**Key workflow features:**
- **Triggered by releases** — Publishing only happens when you intentionally create a release
- **Scoped authentication** — The `setup-node` action configures authentication automatically
- **Build before publish** — Ensures TypeScript compilation (or other build steps) complete first

`.github/workflows/publish.yml`:

```yaml
name: Publish Package
on:
  release:
    types: [published]
jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
      
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          registry-url: 'https://npm.pkg.github.com'
          scope: '@your-org'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Build
        run: npm run build
        
      - name: Publish
        run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```

### Step 4: Consuming the Package

Once published, other projects in your organization can install the package. Consumers need an `.npmrc` file to tell npm where to find your organization's packages. For CI/CD, the `GITHUB_TOKEN` provides read access; for local development, developers need a Personal Access Token with `read:packages` scope.

In another project:

```json
// package.json
{
  "dependencies": {
    "@your-org/my-package": "^1.0.0"
  }
}

```

```
# .npmrc
@your-org:registry=https://npm.pkg.github.com

```

<div class="callout callout-tip">
<div class="callout-title">💡 CSM Tip</div>

When customers ask about migrating from npmjs.com, explain that GitHub Packages can work alongside the public registry. Scoped packages go to GitHub Packages while public dependencies still come from npmjs.com automatically.
</div>

---

## Walkthrough 2: Publishing Container Images

GitHub Container Registry (GHCR) provides a home for your Docker and OCI container images. It's tightly integrated with GitHub Actions and supports advanced features like multi-architecture builds and signed attestations.

### Why GHCR Over Docker Hub?

- **Generous limits** — No rate limiting for authenticated pulls
- **Fine-grained permissions** — Control access at the package level
- **Attestations** — Cryptographic proof of build provenance
- **Cost** — Included with GitHub, no separate Docker subscription

> **📚 Learn more:** [Working with the Container registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)

### Step 1: Create Dockerfile

This example uses a multi-stage build to create a small, production-ready image. Multi-stage builds keep your final image lean by excluding build tools and development dependencies.

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build
FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules
COPY package*.json ./
EXPOSE 3000
CMD ["node", "dist/index.js"]

```

### Step 2: Create Container Publish Workflow

This workflow demonstrates a production-ready container publishing pipeline. Let's break down the key components:

- **Docker Buildx** — Enables advanced build features like caching and multi-platform builds
- **Metadata action** — Automatically generates semantic version tags from git tags
- **Build attestation** — Creates cryptographic proof that GitHub Actions built this image

The workflow builds on every push and PR (for validation), but only pushes images on non-PR events.

`.github/workflows/docker-publish.yml`:

```yaml
name: Build and Push Container
on:
  push:
    branches: [main]
    tags: ['v*.*.*']
  pull_request:
    branches: [main]
env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}
jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
      attestations: write
      id-token: write
      
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      - name: Log in to Container Registry
        if: github.event_name != 'pull_request'
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - name: Extract metadata (tags, labels)
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          tags: |
            type=ref,event=branch
            type=ref,event=pr
            type=semver,pattern={{version}}
            type=semver,pattern={{major}}.{{minor}}
            type=semver,pattern={{major}}
            type=sha
      - name: Build and push
        id: push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
          cache-from: type=gha
          cache-to: type=gha,mode=max
      - name: Generate artifact attestation
        if: github.event_name != 'pull_request'
        uses: actions/attest-build-provenance@v1
        with:
          subject-name: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          subject-digest: ${{ steps.push.outputs.digest }}
          push-to-registry: true

```

### Step 3: Verify and Use the Image

After the workflow runs, you can verify the image was published correctly and check its provenance attestation. The `gh attestation verify` command confirms the image was built by your GitHub Actions workflow—not tampered with or built elsewhere.

```bash
# Pull the image
docker pull ghcr.io/your-org/your-repo:latest
# Verify provenance
gh attestation verify ghcr.io/your-org/your-repo:latest \
  --owner your-org
# Run the container
docker run -p 3000:3000 ghcr.io/your-org/your-repo:latest

```

> **📚 Learn more:** [Verifying artifact attestations](https://docs.github.com/en/actions/security-guides/using-artifact-attestations-to-establish-provenance-for-builds)

---

## Walkthrough 3: Publishing Maven Packages

For Java teams, GitHub Packages provides a Maven repository that integrates with your existing build tools. This is particularly valuable for organizations that want to share internal libraries without setting up Nexus or Artifactory.

> **📚 Learn more:** [Working with the Apache Maven registry](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry)

### Step 1: Configure pom.xml

The `distributionManagement` section tells Maven where to publish your package. Note that the URL must include your organization and repository name—Maven packages in GitHub Packages are always tied to a specific repository.

```xml
<project>
  <groupId>com.yourorg</groupId>
  <artifactId>my-library</artifactId>
  <version>1.0.0</version>
  
  <distributionManagement>
    <repository>
      <id>github</id>
      <name>GitHub Packages</name>
      <url>https://maven.pkg.github.com/YOUR-ORG/YOUR-REPO</url>
    </repository>
  </distributionManagement>
</project>

```

### Step 2: Create Maven Publish Workflow

The `setup-java` action handles Maven authentication automatically when you specify `server-id: github`. This configures Maven's `settings.xml` to use the `GITHUB_TOKEN` for authentication.

```yaml
name: Publish Maven Package
on:
  release:
    types: [published]
jobs:
  publish:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
      
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up JDK 17
        uses: actions/setup-java@v4
        with:
          java-version: '17'
          distribution: 'temurin'
          server-id: github
          
      - name: Publish package
        run: mvn --batch-mode deploy
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```

### Step 3: Consuming in Another Project

To consume packages from GitHub Packages, other projects need to configure a repository in their `pom.xml`. The wildcard (`*`) in the URL allows access to packages from any repository in the organization.

**Important:** Consumers also need authentication configured in their `~/.m2/settings.xml` with a Personal Access Token that has `read:packages` scope.

```xml
<!-- pom.xml -->
<repositories>
  <repository>
    <id>github</id>
    <url>https://maven.pkg.github.com/YOUR-ORG/*</url>
  </repository>
</repositories>
<dependencies>
  <dependency>
    <groupId>com.yourorg</groupId>
    <artifactId>my-library</artifactId>
    <version>1.0.0</version>
  </dependency>
</dependencies>

```

---

## Walkthrough 4: Multi-Architecture Container Builds

Modern deployments often need to support multiple CPU architectures—especially with the rise of ARM-based servers (AWS Graviton, Apple Silicon, Raspberry Pi). Docker Buildx with QEMU emulation lets you build images for multiple architectures from a single workflow.

### When Do Customers Need This?

- **Cost optimization** — ARM instances are often cheaper (AWS Graviton)
- **Developer experience** — Native images for Apple Silicon Macs
- **Edge computing** — Raspberry Pi and IoT deployments
- **Kubernetes** — Mixed architecture clusters

> **📚 Learn more:** [Building multi-platform images](https://docs.docker.com/build/building/multi-platform/)

### Step 1: Configure Buildx for Multi-arch

The key additions here are the QEMU action (which provides CPU emulation) and the `platforms` parameter in the build action. This workflow builds the same Dockerfile for three architectures and creates a single multi-arch manifest.

```yaml
name: Multi-arch Build
on:
  push:
    tags: ['v*.*.*']
jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
      
    steps:
      - uses: actions/checkout@v4
      - name: Set up QEMU
        uses: docker/setup-qemu-action@v3
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      - name: Login to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          platforms: linux/amd64,linux/arm64,linux/arm/v7
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:${{ github.ref_name }}
            ghcr.io/${{ github.repository }}:latest
          cache-from: type=gha
          cache-to: type=gha,mode=max

```

<div class="callout callout-warning">
<div class="callout-title">⚠️ Build Time Consideration</div>

Multi-architecture builds take significantly longer than single-arch builds because each architecture must be built separately (and ARM builds use emulation on x86 runners). Consider using matrix builds with native ARM runners for faster builds in high-volume scenarios.
</div>

---

## Walkthrough 5: Package Retention and Cleanup

Over time, container registries and package repositories accumulate old versions that consume storage and clutter the UI. GitHub provides tools to automate cleanup while preserving important versions.

### Storage Considerations

- **GitHub Free/Pro** — 500MB packages storage included
- **GitHub Team** — 2GB packages storage included  
- **GitHub Enterprise** — 50GB packages storage included
- **Additional storage** — Available for purchase

> **📚 Learn more:** [Managing package access and visibility](https://docs.github.com/en/packages/learn-github-packages/configuring-a-packages-access-control-and-visibility)

### Step 1: Configure Package Retention

This workflow runs weekly and cleans up old package versions while preserving recent releases. The `delete-package-versions` action provides flexible filtering to keep semantic versions while removing PR builds and old untagged images.

```yaml
name: Cleanup Old Packages
on:
  schedule:
    - cron: '0 0 * * 0'  # Weekly on Sunday
  workflow_dispatch:
jobs:
  cleanup:
    runs-on: ubuntu-latest
    permissions:
      packages: write
      
    steps:
      - name: Delete old container versions
        uses: actions/delete-package-versions@v5
        with:
          package-name: 'my-app'
          package-type: 'container'
          min-versions-to-keep: 10
          delete-only-untagged-versions: true
          
      - name: Delete old npm versions
        uses: actions/delete-package-versions@v5
        with:
          package-name: '@org/my-package'
          package-type: 'npm'
          min-versions-to-keep: 5
          ignore-versions: '^\\d+\\.\\d+\\.\\d+$'  # Keep semantic versions

```

<div class="callout callout-tip">
<div class="callout-title">💡 Best Practice</div>

Always use `min-versions-to-keep` to ensure you never accidentally delete all versions of a package. Consider keeping more versions for production packages and fewer for development/PR builds.
</div>

---

## Summary

In this walkthrough, you learned how to:

| Walkthrough | Key Takeaways |
|-------------|---------------|
| **npm Packages** | Configure scoped packages, authenticate with `GITHUB_TOKEN`, set up publish workflows |
| **Container Images** | Use Docker Buildx, generate semantic tags, create build attestations |
| **Maven Packages** | Configure `pom.xml` distribution, authenticate with `setup-java` action |
| **Multi-arch Builds** | Use QEMU emulation, build for multiple platforms simultaneously |
| **Package Cleanup** | Automate retention policies, preserve important versions |

### Additional Resources

- [GitHub Packages Documentation](https://docs.github.com/en/packages)
- [Container Registry Guide](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-container-registry)
- [npm Registry Guide](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-npm-registry)
- [Maven Registry Guide](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-apache-maven-registry)
- [Artifact Attestations](https://docs.github.com/en/actions/security-guides/using-artifact-attestations-to-establish-provenance-for-builds)

{% endraw %}
