---
layout: training-module
title: "Module 5, Section 4: Hands-On Labs"
description: "Practical exercises for building and optimizing GitHub Actions workflows"
permalink: /intermediate/module-5/labs/
module_number: 5
section_number: 4
phase: intermediate
prev_section:
  url: /intermediate/module-5/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /intermediate/module-5/quiz/
  title: "Knowledge Check"
toc: true
module_title: "GitHub Actions & Automation"
total_sections: 7
module_index: /intermediate/module-5/
sections:
  - title: "Context & Overview"
    url: "/intermediate/module-5/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "20 min"
  - title: "Core Concepts"
    url: "/intermediate/module-5/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/intermediate/module-5/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "50 min"
  - title: "Hands-On Labs"
    url: "/intermediate/module-5/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/intermediate/module-5/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "25 min"
  - title: "Resources"
    url: "/intermediate/module-5/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/intermediate/module-5/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "30 min"
---
{% raw %}

## Lab 1: Build a Complete CI/CD Pipeline
**Objective:** Create a full CI/CD pipeline for a web application

### Setup

```bash
# Create project
mkdir actions-lab && cd actions-lab
npm init -y
npm install express jest
# Create app
cat > index.js << 'EOF'
const express = require('express');
const app = express();
app.get('/', (req, res) => {
  res.json({ message: 'Hello World', version: process.env.VERSION || '1.0.0' });
});
app.get('/health', (req, res) => {
  res.json({ status: 'healthy' });
});
module.exports = app;
if (require.main === module) {
  const port = process.env.PORT || 3000;
  app.listen(port, () => console.log(`Server running on port ${port}`));
}
EOF
# Create test
cat > index.test.js << 'EOF'
const app = require('./index');
describe('API', () => {
  test('returns hello message', async () => {
    // Simple test
    expect(true).toBe(true);
  });
});
EOF
# Update package.json scripts
npm pkg set scripts.test="jest"
npm pkg set scripts.start="node index.js"

```

### Tasks
1. **Create a CI workflow** (`.github/workflows/ci.yml`):
   - Trigger on push and pull_request to main
   - Run lint (add eslint if desired)
   - Run tests
   - Build Docker image (don't push yet)
2. **Create a CD workflow** (`.github/workflows/cd.yml`):
   - Trigger on push to main only
   - Call the CI workflow first
   - Deploy to staging environment
   - After approval, deploy to production
   - Add manual trigger option
3. **Create release workflow** (`.github/workflows/release.yml`):
   - Trigger on tag push `v*.*.*`
   - Build Docker image
   - Tag with version and `latest`
   - Create GitHub release with auto-generated notes

### Validation
- [ ] CI runs on every PR
- [ ] CD deploys to staging automatically
- [ ] Production requires approval
- [ ] Releases create proper Docker tags
---

## Lab 2: Self-Hosted Runner Setup
**Objective:** Configure and use self-hosted runners

### Tasks
1. **Register a self-hosted runner:**
   - Go to repository Settings → Actions → Runners
   - Click "New self-hosted runner"
   - Follow setup instructions for your OS
   
2. **Create a workflow using self-hosted runner:**

```yaml
name: Self-Hosted Test
on:
  workflow_dispatch:
jobs:
  test:
    runs-on: self-hosted
    steps:
      - uses: actions/checkout@v4
      - name: Show runner info
        run: |
          echo "Runner OS: ${{ runner.os }}"
          echo "Runner arch: ${{ runner.arch }}"
          echo "Runner name: ${{ runner.name }}"
          hostname
          pwd

```

3. **Add runner labels and use them:**

```yaml
runs-on: [self-hosted, linux, gpu]

```

4. **Configure runner as a service (Linux):**

```bash
sudo ./svc.sh install
sudo ./svc.sh start

```

## Lab 3: Workflow Optimization
**Objective:** Optimize workflow performance and costs

### Starting Workflow (Inefficient)

```yaml
name: Inefficient CI
on: [push]
jobs:
  job1:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm run lint
      
  job2:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm test
      
  job3:
    runs-on: ubuntu-latest
    needs: [job1, job2]
    steps:
      - uses: actions/checkout@v4
      - run: npm install
      - run: npm run build

```

### Tasks
1. **Add path filtering** - Only run when source changes
2. **Implement caching** - Cache npm dependencies
3. **Use concurrency** - Cancel in-progress runs on new push
4. **Add timeouts** - Prevent hung jobs
5. **Optimize job structure** - Reduce redundant steps

### Optimized Workflow Should Include

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
# Path filters
on:
  push:
    paths:
      - 'src/**'
      - 'package*.json'
      
# Timeout
jobs:
  build:
    timeout-minutes: 10

```

### Measure

| Metric | Before | After |
|--------|--------|-------|
| Workflow time | ___ | ___ |
| Billable minutes | ___ | ___ |
| Time saved | - | ___ |
{% endraw %}
---
<div class="section-navigation">
  <a href="{{ page.prev_section.url }}" class="nav-button nav-prev">
    <span class="nav-label">← Previous</span>
    <span class="nav-title">{{ page.prev_section.title }}</span>
  </a>
  <a href="{{ page.next_section.url }}" class="nav-button nav-next">
    <span class="nav-label">Next →</span>
    <span class="nav-title">{{ page.next_section.title }}</span>
  </a>
</div>
