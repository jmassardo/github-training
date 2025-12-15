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

## Walkthrough 1: Publishing npm Packages
**Step 1: Configure package.json**

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

**Step 2: Create .npmrc**

```
@your-org:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=${NODE_AUTH_TOKEN}

```

**Step 3: Create Publish Workflow**
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

**Step 4: Consuming the Package**
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

---

## Walkthrough 2: Publishing Container Images
**Step 1: Create Dockerfile**

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

**Step 2: Create Container Publish Workflow**
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

**Step 3: Verify and Use the Image**

```bash
# Pull the image
docker pull ghcr.io/your-org/your-repo:latest
# Verify provenance
gh attestation verify ghcr.io/your-org/your-repo:latest \
  --owner your-org
# Run the container
docker run -p 3000:3000 ghcr.io/your-org/your-repo:latest

```

## Walkthrough 3: Publishing Maven Packages
**Step 1: Configure pom.xml**

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

**Step 2: Create Maven Publish Workflow**

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

**Step 3: Consuming in Another Project**

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
**Step 1: Configure Buildx for Multi-arch**

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

## Walkthrough 5: Package Retention and Cleanup
**Step 1: Configure Package Retention**

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

{% endraw %}
