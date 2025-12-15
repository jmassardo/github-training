---
layout: training-module
title: "Module 8, Section 2: Core Concepts"
description: "CI/CD pipeline stages, GitHub Environments, deployment strategies, and cloud integration"
permalink: /intermediate/module-8/concepts/
module_number: 8
section_number: 2
phase: intermediate
prev_section:
  url: /intermediate/module-8/overview/
  title: "Context & Overview"
next_section:
  url: /intermediate/module-8/walkthrough/
  title: "Guided Walkthrough"
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

## 2.1 CI/CD Pipeline Stages

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph CI["CI Pipeline"]
    direction LR
    B["🔨 Build<br/>Compile"] --> L["📝 Lint<br/>Style checks"]
    L --> T["🧪 Test<br/>Unit tests"]
    T --> S["🔒 Scan<br/>Security analysis"]
    S --> P["📦 Package<br/>Artifact creation"]
  end
  
  subgraph CD["CD Pipeline"]
    direction LR
    D["🚀 Dev<br/>Auto deploy"] --> ST["🎭 Staging<br/>Auto deploy"]
    ST --> PR["🏭 Prod<br/>Approval required"]
    PR --> V["✅ Verify<br/>Health checks"]
    V --> M["📊 Monitor<br/>Alerts & logs"]
  end
  
  CI --> CD
</div>
</div>

---

## 2.2 GitHub Environments
Environments represent deployment targets with protection rules:

```yaml
jobs:
  deploy-staging:
    environment: staging
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to staging
        run: ./deploy.sh staging
  deploy-production:
    needs: deploy-staging
    environment: production
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to production
        run: ./deploy.sh production

```

### Environment Features

| Feature | Description |
|---------|-------------|
| **Secrets** | Environment-specific secrets |
| **Variables** | Environment-specific configuration |
| **Protection rules** | Required reviewers, wait timer |
| **Deployment branches** | Restrict which branches can deploy |
| **URL** | Environment URL for status display |

### Environment Protection Rules

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Config["Production Environment Configuration"]
    A["🔐 Required reviewers"] --> A1["@release-team (any 2)"]
    B["⏱️ Wait timer"] --> B1["15 minutes"]
    C["🌿 Deployment branches"] --> C1["Selected branches:<br/>main, release/*"]
    D["📋 Custom rules"] --> D1["External API approval required"]
    E["🛡️ Admin bypass"] --> E1["false"]
  end
</div>
</div>

## 2.3 Deployment Strategies

### Rolling Deployment
Gradually replace old instances with new:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph T0["Time 0"]
    A1["v1"] ~~~ A2["v1"] ~~~ A3["v1"] ~~~ A4["v1"] ~~~ A5["v1"]
  end
  subgraph T1["Time 1 - 20% new"]
    B1["v2"]:::new ~~~ B2["v1"] ~~~ B3["v1"] ~~~ B4["v1"] ~~~ B5["v1"]
  end
  subgraph T2["Time 2 - 40% new"]
    C1["v2"]:::new ~~~ C2["v2"]:::new ~~~ C3["v1"] ~~~ C4["v1"] ~~~ C5["v1"]
  end
  subgraph T3["Time 3 - 60% new"]
    D1["v2"]:::new ~~~ D2["v2"]:::new ~~~ D3["v2"]:::new ~~~ D4["v1"] ~~~ D5["v1"]
  end
  subgraph T5["Time 5 - 100% new"]
    F1["v2"]:::new ~~~ F2["v2"]:::new ~~~ F3["v2"]:::new ~~~ F4["v2"]:::new ~~~ F5["v2"]:::new
  end
  
  T0 --> T1 --> T2 --> T3 --> T5
  
  classDef new fill:#22c55e,color:#fff
</div>
</div>

**Pros:** Zero downtime, gradual rollout  
**Cons:** Mixed versions during deployment, slow rollback

### Blue-Green Deployment
Maintain two identical environments:

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Before["Before Switch"]
    LB1["⚖️ Load Balancer"]
    LB1 -->|"100%"| BLUE1["🔵 Blue v1<br/>(LIVE)"]:::live
    LB1 -.->|"0%"| GREEN1["🟢 Green v2<br/>(idle)"]:::idle
  end
  
  subgraph After["After Switch"]
    LB2["⚖️ Load Balancer"]
    LB2 -.->|"0%"| BLUE2["🔵 Blue v1<br/>(idle)"]:::idle
    LB2 -->|"100%"| GREEN2["🟢 Green v2<br/>(LIVE)"]:::live
  end
  
  Before -->|"🔄 Switch"| After
  
  classDef live fill:#22c55e,color:#fff
  classDef idle fill:#6b7280,color:#fff
</div>
</div>

**Pros:** Instant rollback, no mixed versions  
**Cons:** Double infrastructure cost

### Canary Deployment
Route a percentage of traffic to new version:

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  LB["⚖️ Load Balancer"]
  LB -->|"95% traffic"| V1["📦 v1<br/>(stable)"]:::stable
  LB -->|"5% traffic"| V2["🐤 v2<br/>(canary)"]:::canary
  
  subgraph Progress["Traffic Progression"]
    direction LR
    P1["5%"] --> P2["25%"] --> P3["50%"] --> P4["75%"] --> P5["100%"]
  end
  
  classDef stable fill:#3b82f6,color:#fff
  classDef canary fill:#f59e0b,color:#fff
</div>
</div>

**Pros:** Minimal blast radius, real traffic testing  
**Cons:** Complex routing, monitoring required
---

## 2.4 Deployment Status and History
GitHub tracks deployments automatically:

| Environment | Status | Branch | Actor | Time |
|-------------|--------|--------|-------|------|
| production | ✅ Active | main | @jdoe | 2 hours ago |
| production | ⏸️ Inactive | main | @jsmith | 1 day ago |
| staging | ✅ Active | main | @jdoe | 3 hours ago |
| production | ⏸️ Inactive | main | @jdoe | 3 days ago |

## 2.5 Cloud Platform Integration

### OIDC Authentication
Passwordless authentication to cloud providers:

```yaml
permissions:
  id-token: write   # Required for OIDC
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      # Azure
      - uses: azure/login@v2
        with:
          client-id: ${{ secrets.AZURE_CLIENT_ID }}
          tenant-id: ${{ secrets.AZURE_TENANT_ID }}
          subscription-id: ${{ secrets.AZURE_SUBSCRIPTION_ID }}
      
      # AWS
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789:role/deploy
          aws-region: us-east-1
      
      # GCP
      - uses: google-github-actions/auth@v2
        with:
          workload_identity_provider: projects/123/locations/global/workloadIdentityPools/pool/providers/provider
          service_account: deploy@project.iam.gserviceaccount.com

```

{% endraw %}
