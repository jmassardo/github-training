---
layout: training-module
title: "GHES vs GHEC"
permalink: /advanced/module-9/ghes-vs-ghec/
module_number: 9
module_title: "Enterprise Administration"
section_number: 8
total_sections: 10
phase: advanced
estimated_time: "25 min"
module_index: /advanced/module-9/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-9/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/advanced/module-9/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/advanced/module-9/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/advanced/module-9/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/advanced/module-9/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/advanced/module-9/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/advanced/module-9/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
  - title: "GHES vs GHEC"
    url: "/advanced/module-9/ghes-vs-ghec/"
    short_title: "GHES vs GHEC"
    icon: "⚖️"
  - title: "GHES Administration"
    url: "/advanced/module-9/ghes-admin/"
    short_title: "GHES Admin"
    icon: "🖥️"
  - title: "Migrating to GHEC"
    url: "/advanced/module-9/migrations/"
    short_title: "Migrations"
    icon: "🔄"
toc: true
prev_section:
  url: /advanced/module-9/scenarios/
  title: "CSM Scenarios"
next_section:
  url: /advanced/module-9/ghes-admin/
  title: "GHES Administration"
---

# GitHub Enterprise Server vs GitHub Enterprise Cloud

## Understanding the Two Deployment Models

GitHub offers two enterprise deployment models, and as a CSM you'll frequently help customers decide which one fits their needs — or guide them through running both in a hybrid configuration.

> 💡 **Think of it like email:** GHEC is like using Microsoft 365 (cloud-hosted, Microsoft manages the infrastructure). GHES is like running your own Exchange Server on-premises (you manage the hardware, updates, and availability).

---

## At a Glance

| Aspect | GitHub Enterprise Cloud (GHEC) | GitHub Enterprise Server (GHES) |
|--------|-------------------------------|--------------------------------|
| **Hosting** | GitHub-managed (cloud) | Customer-managed (on-premises or private cloud) |
| **Infrastructure** | GitHub handles everything | Customer manages servers, storage, networking |
| **Updates** | Continuous — always on latest | Customer-controlled upgrade cycles |
| **URL** | `github.com` | Custom domain (e.g., `github.yourcompany.com`) |
| **Availability** | 99.9% SLA | Customer-managed HA/DR |
| **Data Residency** | GitHub data centers (US/EU) | Customer's own data centers |
| **Compliance** | SOC 2, FedRAMP (with Data Residency) | Full customer control over data location |
| **Scaling** | Automatic | Manual capacity planning |

---

## Feature Comparison

### Core Platform Features

| Feature | GHEC | GHES |
|---------|------|------|
| Repositories (public & private) | ✅ | ✅ |
| Issues, Projects, Pull Requests | ✅ | ✅ |
| GitHub Actions | ✅ (GitHub-hosted + self-hosted runners) | ✅ (self-hosted runners only) |
| GitHub Advanced Security | ✅ | ✅ |
| GitHub Packages | ✅ | ✅ |
| GitHub Pages | ✅ | ✅ |
| Dependabot | ✅ | ✅ (requires GitHub Connect) |
| Secret Scanning | ✅ | ✅ |
| Code Scanning (CodeQL) | ✅ | ✅ |
| Copilot (IDE features: completions, chat) | ✅ | ✅ (runs on developer workstation, requires internet) |
| Copilot (platform features: Code Review, Coding Agent, Autofix) | ✅ | ❌ |

### Identity & Access Management

| Feature | GHEC | GHES |
|---------|------|------|
| SAML SSO | ✅ | ✅ |
| SCIM Provisioning | ✅ | ✅ (limited providers) |
| Enterprise Managed Users (EMU) | ✅ | ❌ |
| Built-in Authentication | ❌ (requires IdP) | ✅ |
| LDAP | ❌ | ✅ |
| CAS | ❌ | ✅ |

### Enterprise Management

| Feature | GHEC | GHES |
|---------|------|------|
| Enterprise Account | ✅ | ✅ (single instance) |
| Multiple Organizations | ✅ | ✅ |
| Audit Log Streaming | ✅ | ✅ |
| IP Allow Lists | ✅ | N/A (network-level controls) |
| Data Residency | ✅ (EU available) | ✅ (customer-managed) |
| GitHub Connect | N/A (github.com is the destination) | ✅ (configured on GHES, connects outbound to github.com) |

### Features Unique to Each Platform

<div class="callout callout-info" markdown="1">
<div class="callout-title">☁️ GHEC-Only Features</div>

- **Enterprise Managed Users (EMU)** — Full IdP-controlled identity lifecycle
- **GitHub-hosted Runners** — No infrastructure to manage for CI/CD
- **Codespaces** — Cloud development environments
- **Copilot Spaces** — Curated context collections for Copilot
- **Copilot Code Review** — AI-powered pull request reviews on the platform
- **Copilot Coding Agent** — Autonomous issue-to-PR agent
- **Copilot Autofix** — Automatic security vulnerability remediation
- **IP Allow Lists** — Network-level access restrictions
- **Data Residency (EU)** — GitHub-managed EU data hosting
- **Automatic scaling** — No capacity planning needed
</div>

<div class="callout callout-tip" markdown="1">
<div class="callout-title">🖥️ GHES-Only Features</div>

- **LDAP authentication** — Direct LDAP/Active Directory integration without SAML
- **CAS authentication** — Central Authentication Service support
- **Built-in authentication** — No external IdP required
- **Full network isolation** — Air-gapped deployment possible
- **Custom TLS certificates** — Full control over SSL/TLS configuration
- **Direct database access** — For advanced troubleshooting and custom reporting
</div>

---

## Decision Framework: When to Recommend Each

### Recommend GHEC When

- Customer wants **minimal infrastructure management** burden
- Team needs **GitHub-hosted runners** for CI/CD
- Organization wants **Codespaces** for cloud development
- Customer is comfortable with data stored in **GitHub's infrastructure**
- **Rapid onboarding** is a priority (no servers to provision)
- Customer wants to be **always on the latest features**
- **EMU** is required for strict identity control

### Recommend GHES When

- **Regulatory or compliance requirements** mandate data stays on-premises
- Customer operates in an **air-gapped or restricted network** environment
- Organization needs **LDAP** integration without SAML
- Customer requires **complete infrastructure control** (servers, storage, networking)
- Existing investment in **on-premises data centers** and operations teams
- Industry-specific regulations (financial services, defense, government)

### Recommend Hybrid (GHES with GitHub Connect) When

- Customer has **some teams that require on-premises** and others that can use cloud
- Organization is in a **phased migration** from GHES to GHEC
- Teams need access to **GitHub.com Actions marketplace** from GHES
- Developers want **unified contribution graphs** across both platforms
- Customer wants **Dependabot alerts on GHES** (requires Connect for advisory database sync)

---

## GitHub Connect: Bridging GHES to GitHub.com

GitHub Connect is a GHES feature that creates an outbound connection from your Enterprise Server instance to github.com. It enables GHES to leverage select cloud services without requiring a full migration.

### GitHub Connect Features

| Feature | What It Does | Why It Matters |
|---------|-------------|----------------|
| **Unified Search** | Search github.com code from GHES | Developers find public/open-source code without leaving GHES |
| **Unified Contributions** | GHES contributions appear on GitHub.com profiles | Developer recognition and engagement |
| **Server Statistics** | Send anonymized usage data to GitHub.com | Enables support and capacity planning insights |
| **Dependabot Alerts** | Sync the GitHub Advisory Database from github.com to GHES | Vulnerability alerts on GHES without manual updates |
| **GitHub Actions Sync** | Download actions from github.com for use on GHES | Access the full Actions marketplace |
| **Automatic User License Sync** | Sync license consumption to the enterprise account on github.com | Simplified billing and license management |

### How GitHub Connect Works

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph OnPrem["Customer Data Center"]
    GHES["🖥️ GitHub Enterprise Server"]
    GHES_DB["💾 Data & Repos"]
    GHES --> GHES_DB
  end
  
  subgraph Cloud["GitHub.com"]
    GHcom["☁️ GitHub.com"]
    Advisory["📋 Advisory Database"]
    Actions["⚡ Actions Marketplace"]
    Contribs["📊 Contribution Graph"]
  end
  
  GHES -->|"GitHub Connect<br/>(Outbound HTTPS only)"| GHcom
  GHES -.->|"Dependabot Sync"| Advisory
  GHES -.->|"Actions Sync"| Actions
  GHES -.->|"Contribution Sync"| Contribs
</div>
</div>

<div class="callout callout-warning" markdown="1">
<div class="callout-title">🔒 Security Note</div>

GitHub Connect uses **outbound HTTPS connections only** from GHES to GitHub.com. No inbound connections are required. Data shared is limited to the specific features enabled, and customers control exactly which features are active.
</div>

---

## Licensing & Pricing Considerations

### License Structure

| Component | GHEC | GHES | Both (GHEC + GHES) |
|-----------|------|------|---------------------|
| **Base License** | Per-user/month | Per-user/year | Combined per-user pricing |
| **GitHub Actions** | Included minutes + overage | Self-hosted (customer-managed compute) | Varies |
| **Advanced Security** | Per-committer add-on | Per-committer add-on | Per-committer add-on |
| **Copilot** | Per-user add-on (full platform features) | Per-user add-on (IDE features only) | Per-user add-on |
| **Support** | Premium included | Premium included | Premium included |

### Key Licensing Points for CSMs

- Customers running **both GHEC and GHES** get a bundled per-user rate
- **GitHub Actions** minutes on GHEC are included; GHES Actions use self-hosted runners (no minute-based billing, but customer pays for compute)
- **GHAS** is licensed per active committer regardless of platform
- **Copilot** is licensed per user regardless of platform, but platform-integrated features (Code Review, Coding Agent, Autofix) are only available on GHEC — developers on GHES still get IDE-based completions and chat
- Encourage customers to use **license sync via GitHub Connect** to avoid duplicate billing

---

## Common Customer Questions

### "Can we migrate from GHES to GHEC?"

Yes. GitHub Enterprise Importer (GEI) supports migrating repositories, including history, pull requests, and issues. See the [Migrating to GHEC]({{ site.baseurl }}/advanced/module-9/migrations/) section for full details.

### "Will we lose any functionality moving to GHEC?"

Most functionality is available on both platforms. The main differences are around authentication (LDAP/CAS are GHES-only), infrastructure control, and platform-integrated Copilot features (Code Review, Coding Agent, and Autofix are GHEC-only). GHEC also gains features like EMU, Codespaces, and GitHub-hosted runners. Review the feature comparison table above with your customer.

### "Can we run GHES in a cloud provider instead of on-premises?"

Absolutely. GHES is supported on **AWS**, **Azure**, **GCP**, and **VMware/Hyper-V**. Many customers run GHES on cloud infrastructure for the flexibility of IaaS while maintaining full control over their GitHub instance.

### "What about data residency in the EU?"

GHEC offers a **Data Residency** option for EU-based storage. For customers who need data to remain within their own infrastructure, GHES provides complete control over data location.

---

## CSM Talking Points

**For customers evaluating GitHub Enterprise:**

> "The biggest question isn't which platform is better — it's which deployment model fits your organization's requirements. GHEC gives you the fastest time to value with zero infrastructure overhead, plus full access to platform-integrated Copilot features like Code Review and Coding Agent. GHES gives you complete control over your infrastructure and data. And GitHub Connect lets GHES customers tap into github.com services like the Actions marketplace and advisory database."

**For customers considering migration:**

> "Many of our enterprise customers start with GHES and evolve to GHEC as cloud adoption matures. GitHub Connect lets GHES instances pull in services from github.com during a transition period, and GitHub Enterprise Importer handles the heavy lifting of moving repositories."

**For regulated industries:**

> "GHES gives you the ability to host GitHub within your own compliance boundary. For customers who can use cloud services, GHEC's Data Residency option and SOC 2/FedRAMP compliance meet the requirements of most regulatory frameworks."

---

## Additional Resources

- 📖 [GitHub Enterprise overview](https://docs.github.com/en/get-started/learning-about-github/githubs-products#github-enterprise)
- 📖 [About GitHub Enterprise Server](https://docs.github.com/en/enterprise-server/admin/overview/about-github-enterprise-server)
- 📖 [About GitHub Enterprise Cloud](https://docs.github.com/en/enterprise-cloud@latest/admin/overview/about-github-enterprise-cloud)
- 📖 [About GitHub Connect](https://docs.github.com/en/enterprise-server/admin/configuration/configuring-github-connect/about-github-connect)
- 📖 [GitHub Enterprise pricing](https://github.com/pricing)
