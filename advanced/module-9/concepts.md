---
layout: training-module
title: "Core Concepts"
permalink: /advanced/module-9/concepts/
module_number: 9
module_title: "Enterprise Administration"
section_number: 2
total_sections: 10
phase: advanced
estimated_time: "45 min"
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
  url: /advanced/module-9/overview/
  title: "Context & Overview"
next_section:
  url: /advanced/module-9/walkthrough/
  title: "Guided Walkthrough"
---

<div class="callout callout-info" markdown="1">
<div class="callout-title">🏢 Official Documentation</div>
For comprehensive reference, see <a href="https://docs.github.com/en/enterprise-cloud@latest/admin">GitHub Enterprise Administration</a> and <a href="https://docs.github.com/en/organizations">Organization Management</a>.
</div>

# Core Concepts Deep Dive

## 2.1 Enterprise Account Structure

GitHub Enterprise Cloud provides a hierarchical structure for managing organizations:

**Enterprise Account** → **Organizations** → **Teams** → **Repositories**

### Enterprise-Level Controls

| Control | Description | When to Use |
|---------|-------------|-------------|
| **Enterprise Policies** | Set defaults for all organizations | Enforce consistent security baselines |
| **Enterprise Owners** | Full administrative access | Limited to trusted administrators |
| **Enterprise Support** | Premium support access | Enterprise-tier support tickets |
| **Audit Log Streaming** | Real-time audit data export | SIEM integration, compliance |

### Organization Management

```yaml
# Example organization structure for a large enterprise
enterprise_structure:
  enterprise_name: "Acme Corporation"
  organizations:
    - name: "acme-platform"
      purpose: "Shared platform and infrastructure"
      visibility: "internal"
      teams:
        - platform-core
        - sre-team
        - security-team
    
    - name: "acme-products"
      purpose: "Product development teams"
      visibility: "internal"
      teams:
        - product-alpha
        - product-beta
        - mobile-team
    
    - name: "acme-opensource"
      purpose: "Public open source projects"
      visibility: "public"
      teams:
        - oss-maintainers
        - community-team

```

## 2.2 Identity Management: SAML SSO

SAML (Security Assertion Markup Language) provides single sign-on authentication:

### How SAML SSO Works

<div class="mermaid-container">
<div class="mermaid">
sequenceDiagram
    participant User
    participant GitHub
    participant IdP as Identity Provider<br/>(Okta, Azure AD, etc.)
    
    User->>GitHub: 1. Access GitHub
    GitHub->>User: 2. Redirect to IdP
    User->>IdP: 3. Authenticate
    IdP->>GitHub: 4. SAML Assertion
    GitHub->>User: 5. Access Granted
</div>
</div>

### Supported Identity Providers

| IdP | Documentation | Notes |
|-----|--------------|-------|
| **Azure AD / Entra ID** | [GitHub Docs](https://docs.github.com/en/enterprise-cloud@latest/admin/identity-and-access-management/using-saml-for-enterprise-iam) | Most common enterprise IdP |
| **Okta** | [Okta Integration](https://www.okta.com/integrations/github-enterprise-cloud/) | Pre-built integration available |
| **PingFederate** | GitHub SAML Guide | Common in financial services |
| **OneLogin** | GitHub SAML Guide | Good for mid-market |
| **Generic SAML** | GitHub SAML Guide | Any SAML 2.0 provider |

### SAML Configuration Checklist

```markdown
## SAML SSO Configuration Checklist

### Identity Provider Setup
- [ ] Create GitHub Enterprise application in IdP
- [ ] Configure SAML assertion attributes:
  - [ ] NameID (required): User identifier
  - [ ] email: User email address
  - [ ] name: Display name (optional)
  - [ ] groups: Group memberships (optional)
- [ ] Generate IdP metadata/certificate

### GitHub Enterprise Configuration  
- [ ] Enable SAML SSO at enterprise level
- [ ] Upload IdP metadata or configure manually:
  - [ ] Sign-on URL
  - [ ] Issuer
  - [ ] Public certificate
- [ ] Test SSO with single user
- [ ] Enable for all organization members

### Validation
- [ ] Verify SSO authentication works
- [ ] Test user provisioning
- [ ] Validate session timeout behavior
- [ ] Document recovery/bypass procedures

```

## 2.3 SCIM User Provisioning

SCIM (System for Cross-domain Identity Management) automates user lifecycle:

### SCIM Capabilities

| Operation | Description | Benefit |
|-----------|-------------|---------|
| **Provision** | Create users when added to IdP | Automated onboarding |
| **Update** | Sync attribute changes | Keep data current |
| **Deprovision** | Suspend/remove when removed from IdP | Security compliance |
| **Group Sync** | Map IdP groups to GitHub teams | Automated team membership |

### SCIM Implementation Flow

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph IdP["IDENTITY PROVIDER"]
    U["👥 Users"]
    G["📁 Groups"]
    AR["📋 Assignment Rules"]
  end
  
  IdP -->|"SCIM API<br/>(POST, PATCH, DELETE)"| GH
  
  subgraph GH["GITHUB ENTERPRISE"]
    subgraph SCIM["SCIM Endpoint"]
      S1["Create/Update/Delete Users"]
      S2["Sync Group → Team Mappings"]
      S3["Maintain Audit Trail"]
    end
    
    OU["👥 Org Users"]
    T["👥 Teams"]
    P["🔐 Permissions"]
  end
</div>
</div>

## 2.4 Enterprise Managed Users (EMU)

EMU provides the highest level of identity control:

### EMU vs. Standard Enterprise

| Feature | Standard Enterprise | EMU Enterprise |
|---------|--------------------| ---------------|
| **User Identity** | Personal GitHub accounts | IdP-controlled accounts |
| **Username Format** | User-chosen | `shortcode_username` |
| **Account Lifecycle** | User-controlled | Fully IdP-controlled |
| **External Collaboration** | Users can contribute anywhere | Limited to enterprise |
| **Personal Repos** | Yes | No |
| **Account Recovery** | User password reset | IdP only |

### When to Recommend EMU

**Recommend EMU when:**

- Strict compliance requirements (financial services, healthcare)
- Need complete control over user identity lifecycle
- Want to prevent data leakage to personal accounts
- Organization has mature IdP infrastructure

**Recommend Standard Enterprise when:**

- Developers contribute to open source projects
- Acquisitions bring users with existing GitHub identities
- Organization wants user flexibility
- Gradual migration from existing GitHub.com accounts

## 2.5 Audit Log & Compliance

Enterprise audit logs capture comprehensive activity data:

### Audit Log Categories

```yaml
audit_log_categories:
  authentication:
    - oauth_authorization.create
    - oauth_authorization.destroy
    - user_session.create
    - user_session.destroy
  
  repository_actions:
    - repo.create
    - repo.destroy
    - repo.access
    - repo.visibility_change
    - protected_branch.update
  
  organization_actions:
    - org.add_member
    - org.remove_member
    - org.update_member
    - team.create
    - team.add_member
  
  enterprise_actions:
    - enterprise.config.enable
    - enterprise.config.disable
    - business.sso_response
    - audit_log_streaming.enable

```

### Audit Log Streaming

Stream audit logs to external systems for analysis:

| Destination | Use Case | Setup Complexity |
|-------------|----------|------------------|
| **Amazon S3** | Long-term storage, Athena queries | Low |
| **Azure Blob Storage** | Azure ecosystem integration | Low |
| **Google Cloud Storage** | GCP ecosystem integration | Low |
| **Splunk** | Real-time security monitoring | Medium |
| **Datadog** | DevOps monitoring integration | Medium |

### Sample Audit Log Query (GraphQL)

```graphql
query GetAuditLogs($enterprise: String!, $first: Int!) {
  enterprise(slug: $enterprise) {
    auditLog(first: $first) {
      edges {
        node {
          action
          actor {
            ... on User {
              login
            }
          }
          createdAt
          operationType
          ... on RepositoryAuditEntry {
            repository {
              name
            }
          }
        }
      }
    }
  }
}

```

## 2.6 GitHub Connect (Hybrid Deployments)

For customers running GitHub Enterprise Server alongside GitHub Enterprise Cloud:

### GitHub Connect Features

| Feature | Description | Benefit |
|---------|-------------|---------|
| **Unified Search** | Search across GHES and GHEC | Find code anywhere |
| **Unified Contributions** | Contribution graph from GHES | Developer recognition |
| **Server Statistics** | Usage analytics | Capacity planning |
| **Dependabot Updates** | Advisory database sync | Security coverage |
| **GitHub Actions** | Action sync from GitHub.com | Access marketplace actions |
