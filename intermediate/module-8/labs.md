---
layout: training-module
title: "Module 8, Section 4: Hands-On Labs"
description: "Build deployment pipelines, implement canary deployments, and multi-cloud strategies"
permalink: /intermediate/module-8/labs/
module_number: 8
section_number: 4
phase: intermediate
prev_section:
  url: /intermediate/module-8/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /intermediate/module-8/quiz/
  title: "Knowledge Check"
toc: true
module_title: "CI/CD Pipelines & Deployment"
total_sections: 7
module_index: /intermediate/module-8/
sections:
  - title: "Context & Overview"
    url: "/intermediate/module-8/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "25 min"
  - title: "Core Concepts"
    url: "/intermediate/module-8/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/intermediate/module-8/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "60 min"
  - title: "Hands-On Labs"
    url: "/intermediate/module-8/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/intermediate/module-8/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "30 min"
  - title: "Resources"
    url: "/intermediate/module-8/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/intermediate/module-8/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "35 min"
---
{% raw %}

## Lab 1: Build a Complete Deployment Pipeline
**Objective:** Create an end-to-end pipeline with environments, gates, and monitoring

### Tasks:
**1. Create a sample application:**

```bash
mkdir deployment-lab && cd deployment-lab
npm init -y
cat > index.js << 'EOF'
const express = require('express');
const app = express();
const VERSION = process.env.VERSION || '1.0.0';
app.get('/health', (req, res) => res.json({ status: 'healthy', version: VERSION }));
app.get('/', (req, res) => res.json({ message: 'Hello World', version: VERSION }));
app.listen(3000, () => console.log(`Server v${VERSION} running on port 3000`));
EOF
npm install express

```

**2. Create three environments:**
   - `development` - No protection, auto-deploy
   - `staging` - No protection, auto-deploy after dev
   - `production` - Require approval, 10-minute wait
**3. Implement deployment workflow:**
   - Build and test
   - Deploy to dev → staging → production
   - Health checks between stages
   - Automatic rollback on failure
**4. Add deployment notifications:**
   - Slack webhook for deployment status
   - GitHub Issue creation on failure

### Validation:
- [ ] All three environments configured
- [ ] Pipeline progresses dev → staging → production
- [ ] Production requires manual approval
- [ ] Health checks work
- [ ] Notifications sent
---

## Lab 2: Canary Deployment with Kubernetes
**Objective:** Implement progressive delivery with traffic splitting

### Tasks:
**1. Set up Kubernetes manifests for canary:**

```yaml
# stable-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-stable
spec:
  replicas: 9
  # ...
# canary-deployment.yml  
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-canary
spec:
  replicas: 1  # 10% traffic
  # ...

```

**2. Create canary workflow:**
   - Deploy canary with 10% traffic
   - Monitor for 10 minutes
   - Promote to 50% if healthy
   - Promote to 100% if still healthy
   - Auto-rollback on errors
**3. Implement monitoring checks:**
   - Error rate check
   - Latency check
   - Custom metrics

### Validation:
- [ ] Canary deploys with limited traffic
- [ ] Traffic gradually increases
- [ ] Rollback works on failure

## Lab 3: Multi-Cloud Deployment
**Objective:** Deploy the same application to multiple cloud providers

### Tasks:
**1. Configure OIDC for all three clouds:**
   - Azure
   - AWS
   - GCP
**2. Create unified deployment workflow:**

```yaml
strategy:
  matrix:
    cloud: [azure, aws, gcp]
    
steps:
  - name: Deploy to ${{ matrix.cloud }}
    # Cloud-specific deployment

```

**3. Implement health verification for each:**
   - Check deployment status
   - Verify application health
   - Report status to GitHub

### Validation:
- [ ] OIDC configured for all clouds
- [ ] Single workflow deploys to all
- [ ] Each deployment verified independently

---

## Lab 4: Deploy a Static Site with GitHub Pages
**Objective:** Set up automated deployment to GitHub Pages using Actions

GitHub Pages is a free static site hosting service built into GitHub. This lab demonstrates deploying a documentation site or static web app.

### Tasks:

**1. Create a static site:**

```bash
mkdir pages-lab && cd pages-lab
git init

# Create a simple site
cat > index.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My GitHub Pages Site</title>
    <style>
        body { font-family: -apple-system, sans-serif; max-width: 800px; margin: 2rem auto; padding: 0 1rem; }
        h1 { color: #0969da; }
        .version { background: #ddf4ff; padding: 0.5rem 1rem; border-radius: 6px; }
    </style>
</head>
<body>
    <h1>🚀 Deployed with GitHub Actions</h1>
    <p class="version">Version: <span id="version">1.0.0</span></p>
    <p>Build time: <span id="time"></span></p>
    <script>document.getElementById('time').textContent = new Date().toISOString();</script>
</body>
</html>
EOF

# Create a 404 page
cat > 404.html << 'EOF'
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Page Not Found</title>
</head>
<body>
    <h1>404 - Page Not Found</h1>
    <p><a href="/">Return home</a></p>
</body>
</html>
EOF
```

**2. Create GitHub Pages deployment workflow:**

```yaml
# .github/workflows/deploy-pages.yml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Setup Pages
        uses: actions/configure-pages@v4
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

**3. Configure GitHub Pages in repository settings:**
   - Go to Settings → Pages
   - Source: Select "GitHub Actions"
   - Wait for first deployment

**4. (Optional) Add a build step for static site generators:**

For Jekyll, Hugo, or other static site generators:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Build site
        run: npm run build
      
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: './dist'  # Your build output folder
```

**5. Add custom domain (optional):**

```bash
# Create CNAME file for custom domain
echo "docs.example.com" > CNAME
```

Then configure DNS:

- For apex domain: A records pointing to GitHub's IPs
- For subdomain: CNAME record pointing to `<username>.github.io`

### Validation:
- [ ] Site deploys successfully on push to main
- [ ] Site accessible at `https://<username>.github.io/<repo>/`
- [ ] 404 page works for invalid URLs
- [ ] Deployment shows in repository Environments
- [ ] (Optional) Custom domain configured and working

### CSM Talking Points

| Scenario | GitHub Pages Value |
|----------|-------------------|
| **Documentation** | "Host project docs alongside code. Every push updates docs automatically." |
| **Marketing/Landing** | "Free hosting for product pages, no separate infrastructure needed." |
| **Previews** | "Preview branches deploy to separate URLs for review before merge." |
| **Internal Tools** | "Enterprise can use Pages for internal dashboards and tools." |

---

{% endraw %}
