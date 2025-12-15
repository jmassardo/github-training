---
layout: training-module
title: "Module 8, Section 3: Guided Walkthrough"
description: "Step-by-step implementation of CI/CD pipelines, environments, and deployments"
permalink: /intermediate/module-8/walkthrough/
module_number: 8
section_number: 3
phase: intermediate
prev_section:
  url: /intermediate/module-8/concepts/
  title: "Core Concepts"
next_section:
  url: /intermediate/module-8/labs/
  title: "Hands-On Labs"
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

## Walkthrough 1: Complete CI/CD Pipeline
Create a full pipeline from commit to production.

### Step 1: CI Pipeline
`.github/workflows/ci.yml`:

```yaml
name: CI
on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm test -- --coverage
      - uses: actions/upload-artifact@v4
        with:
          name: coverage
          path: coverage/
  build:
    needs: [lint, test]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with:
          name: build
          path: dist/
  security-scan:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Snyk
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}

```

### Step 2: CD Pipeline
`.github/workflows/cd.yml`:

```yaml
name: CD
on:
  workflow_run:
    workflows: ["CI"]
    branches: [main]
    types: [completed]
concurrency:
  group: deployment
  cancel-in-progress: false
jobs:
  # Only run if CI succeeded
  check-ci:
    runs-on: ubuntu-latest
    if: ${{ github.event.workflow_run.conclusion == 'success' }}
    outputs:
      should_deploy: ${{ steps.check.outputs.deploy }}
    steps:
      - id: check
        run: echo "deploy=true" >> $GITHUB_OUTPUT
  deploy-staging:
    needs: check-ci
    runs-on: ubuntu-latest
    environment:
      name: staging
      url: https://staging.example.com
    steps:
      - uses: actions/checkout@v4
      
      - name: Download build artifact
        uses: dawidd6/action-download-artifact@v3
        with:
          workflow: ci.yml
          name: build
          path: dist/
          
      - name: Deploy to staging
        run: |
          echo "Deploying to staging..."
          # Add actual deployment commands
          
      - name: Run smoke tests
        run: |
          curl -f https://staging.example.com/health || exit 1
  integration-tests:
    needs: deploy-staging
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run integration tests
        run: npm run test:integration
        env:
          TEST_URL: https://staging.example.com
  deploy-production:
    needs: integration-tests
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://example.com
    steps:
      - uses: actions/checkout@v4
      
      - name: Download build artifact
        uses: dawidd6/action-download-artifact@v3
        with:
          workflow: ci.yml
          name: build
          path: dist/
          
      - name: Deploy to production
        run: |
          echo "Deploying to production..."
          # Add actual deployment commands
          
      - name: Verify deployment
        run: |
          sleep 30
          curl -f https://example.com/health || exit 1
          
      - name: Notify success
        if: success()
        uses: slackapi/slack-github-action@v1
        with:
          payload: |
            {"text": "✅ Production deployment successful: ${{ github.sha }}"}
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK }}

```

---

## Walkthrough 2: Configure GitHub Environments

### Step 1: Create Environments
1. Go to repository **Settings** → **Environments**
2. Click **New environment**
3. Create environments: `development`, `staging`, `production`

### Step 2: Configure Staging Environment

```
Environment: staging
├── Environment secrets:
│   └── STAGING_API_KEY: ****
├── Environment variables:
│   ├── API_URL: https://api.staging.example.com
│   └── LOG_LEVEL: debug
├── Deployment branches:
│   └── All branches
└── Protection rules:
    └── None (auto-deploy)

```

### Step 3: Configure Production Environment

```
Environment: production
├── Environment secrets:
│   └── PROD_API_KEY: ****
├── Environment variables:
│   ├── API_URL: https://api.example.com
│   └── LOG_LEVEL: warn
├── Deployment branches:
│   └── Selected branches: main
├── Required reviewers:
│   └── @release-team (require 2)
├── Wait timer: 15 minutes
└── Custom deployment protection rules:
    └── Check deployment health (external)

```

### Step 4: Use Environment in Workflow

```yaml
jobs:
  deploy:
    environment:
      name: production
      url: ${{ steps.deploy.outputs.url }}
    steps:
      - name: Deploy
        id: deploy
        run: |
          ./deploy.sh
          echo "url=https://example.com" >> $GITHUB_OUTPUT
        env:
          API_KEY: ${{ secrets.PROD_API_KEY }}
          API_URL: ${{ vars.API_URL }}

```

## Walkthrough 3: Deploy to Azure

### Step 1: Set Up OIDC in Azure

```bash
# Create App Registration
az ad app create --display-name "github-actions-deploy"
# Create Service Principal
az ad sp create --id <app-id>
# Create Federated Credential
az ad app federated-credential create \
  --id <app-id> \
  --parameters '{
    "name": "github-main",
    "issuer": "https://token.actions.githubusercontent.com",
    "subject": "repo:org/repo:ref:refs/heads/main",
    "audiences": ["api://AzureADTokenExchange"]
  }'
# Grant permissions
az role assignment create \
  --assignee <sp-id> \
  --role "Contributor" \
  --scope /subscriptions/<subscription-id>/resourceGroups/<rg-name>

```

### Step 2: Create Azure Deployment Workflow

```yaml
name: Deploy to Azure
on:
  push:
    branches: [main]
permissions:
  id-token: write
  contents: read
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: production
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Azure Login
        uses: azure/login@v2
        with:
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
      
      - name: Deploy to Azure Web App
        uses: azure/webapps-deploy@v3
        with:
          app-name: my-web-app
          slot-name: staging
          package: dist/
          
      - name: Swap staging to production
        run: |
          az webapp deployment slot swap \
            --name my-web-app \
            --resource-group my-rg \
            --slot staging \
            --target-slot production

```

---

## Walkthrough 4: Kubernetes Deployment

### Step 1: Create Kubernetes Manifests
`k8s/deployment.yml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app
          image: ghcr.io/org/my-app:TAG
          ports:
            - containerPort: 3000
          readinessProbe:
            httpGet:
              path: /health
              port: 3000
            initialDelaySeconds: 5
            periodSeconds: 10
          livenessProbe:
            httpGet:
              path: /health
              port: 3000
            initialDelaySeconds: 15
            periodSeconds: 20
          resources:
            requests:
              memory: "128Mi"
              cpu: "100m"
            limits:
              memory: "256Mi"
              cpu: "500m"

```

### Step 2: Create Deployment Workflow

```yaml
name: Deploy to Kubernetes
on:
  push:
    tags: ['v*.*.*']
env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}
jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    outputs:
      image_tag: ${{ steps.meta.outputs.tags }}
    steps:
      - uses: actions/checkout@v4
      
      - name: Log in to GHCR
        uses: docker/login-action@v3
        with:
          registry: ${{ env.REGISTRY }}
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}
          
      - name: Extract metadata
        id: meta
        uses: docker/metadata-action@v5
        with:
          images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
          
      - name: Build and push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment: production
    permissions:
      id-token: write
      contents: read
      
    steps:
      - uses: actions/checkout@v4
      
      - name: Azure login
        uses: azure/login@v2
        with:
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
          
      - name: Get AKS credentials
        run: |
          az aks get-credentials \
            --resource-group my-rg \
            --name my-aks-cluster
            
      - name: Deploy to Kubernetes
        run: |
          # Replace image tag in manifest
          sed -i "s|IMAGE_TAG|${{ github.ref_name }}|g" k8s/deployment.yml
          
          kubectl apply -f k8s/
          kubectl rollout status deployment/my-app --timeout=300s
          
      - name: Verify deployment
        run: |
          kubectl get pods -l app=my-app
          kubectl get services

```

## Walkthrough 5: Implementing Rollback

### Step 1: Manual Rollback Workflow

```yaml
name: Rollback Production
on:
  workflow_dispatch:
    inputs:
      version:
        description: 'Version to rollback to (e.g., v1.2.3)'
        required: true
        type: string
      reason:
        description: 'Reason for rollback'
        required: true
        type: string
jobs:
  rollback:
    runs-on: ubuntu-latest
    environment: production
    
    steps:
      - name: Verify version exists
        run: |
          # Check if the specified version exists
          if ! gh release view ${{ inputs.version }} &>/dev/null; then
            echo "Version ${{ inputs.version }} not found"
            exit 1
          fi
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          
      - name: Create rollback issue
        uses: actions/github-script@v7
        with:
          script: |
            await github.rest.issues.create({
              owner: context.repo.owner,
              repo: context.repo.repo,
              title: `Rollback to ${context.payload.inputs.version}`,
              body: `
                ## Rollback Details
                - **Target Version:** ${context.payload.inputs.version}
                - **Reason:** ${context.payload.inputs.reason}
                - **Initiated by:** @${context.actor}
                - **Time:** ${new Date().toISOString()}
              `,
              labels: ['rollback', 'incident']
            });
            
      - name: Perform rollback
        run: |
          echo "Rolling back to ${{ inputs.version }}"
          # Kubernetes rollback
          kubectl set image deployment/my-app \
            my-app=ghcr.io/org/my-app:${{ inputs.version }}
          kubectl rollout status deployment/my-app
          
      - name: Notify team
        uses: slackapi/slack-github-action@v1
        with:
          payload: |
            {
              "text": "🔄 Production rollback completed",
              "blocks": [
                {
                  "type": "section",
                  "text": {
                    "type": "mrkdwn",
                    "text": "*Production Rollback*\n• Version: ${{ inputs.version }}\n• Reason: ${{ inputs.reason }}\n• By: ${{ github.actor }}"
                  }
                }
              ]
            }
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK }}

```

### Step 2: Automatic Rollback on Failure

```yaml
deploy:
  runs-on: ubuntu-latest
  environment: production
  steps:
    - name: Save current version
      id: current
      run: |
        CURRENT=$(kubectl get deployment my-app -o jsonpath='{.spec.template.spec.containers[0].image}')
        echo "version=$CURRENT" >> $GITHUB_OUTPUT
        
    - name: Deploy new version
      id: deploy
      run: |
        kubectl apply -f k8s/
        kubectl rollout status deployment/my-app --timeout=300s
      continue-on-error: true
        
    - name: Health check
      id: health
      if: steps.deploy.outcome == 'success'
      run: |
        sleep 30
        for i in {1..5}; do
          if curl -sf https://api.example.com/health; then
            echo "Health check passed"
            exit 0
          fi
          sleep 10
        done
        echo "Health check failed"
        exit 1
      continue-on-error: true
      
    - name: Rollback on failure
      if: steps.deploy.outcome == 'failure' || steps.health.outcome == 'failure'
      run: |
        echo "Deployment failed, rolling back to ${{ steps.current.outputs.version }}"
        kubectl rollout undo deployment/my-app
        kubectl rollout status deployment/my-app
        
    - name: Fail workflow if rollback occurred
      if: steps.deploy.outcome == 'failure' || steps.health.outcome == 'failure'
      run: exit 1

```

{% endraw %}
