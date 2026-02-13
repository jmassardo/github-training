---
layout: training-module
title: "Knowledge Check"
permalink: /advanced/module-9/quiz/
module_number: 9
module_title: "Enterprise Administration"
section_number: 5
total_sections: 10
phase: advanced
estimated_time: "15 min"
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
  url: /advanced/module-9/labs/
  title: "Hands-On Labs"
next_section:
  url: /advanced/module-9/resources/
  title: "Resources"
---

# Knowledge Check

## Quiz Questions

**Question 1:** What is the primary difference between SAML SSO and SCIM?

A) SAML provides authentication, SCIM provides user provisioning  
B) SAML is for organizations, SCIM is for enterprises  
C) SAML is deprecated, SCIM is the replacement  
D) SAML works with Azure AD, SCIM works with Okta

---

**Question 2:** When should you recommend Enterprise Managed Users (EMU)?

A) When the customer wants developers to contribute to open source  
B) When the customer has strict compliance requirements and needs full identity control  
C) When the customer is migrating from GitHub Enterprise Server  
D) When the customer has fewer than 100 developers

---

**Question 3:** What does GitHub Connect enable?

A) Connection between two GitHub.com organizations  
B) Connection between GitHub Enterprise Server and GitHub Enterprise Cloud  
C) Connection between GitHub and Azure DevOps  
D) Connection between GitHub and Jenkins

---

**Question 4:** What is the correct SCIM endpoint format for GitHub Enterprise?

A) `https://github.com/scim/v2/enterprises/{enterprise}`  
B) `https://api.github.com/scim/v2/enterprises/{enterprise}`  
C) `https://api.github.com/v3/enterprises/{enterprise}/scim`  
D) `https://scim.github.com/enterprises/{enterprise}`

---

**Question 5:** Which audit log streaming destination is NOT natively supported?

A) Amazon S3  
B) Azure Event Hubs  
C) Elasticsearch  
D) Google Cloud Storage

---

**Question 6:** What happens to a user's personal repositories when using EMU?

A) They are migrated to the enterprise  
B) They remain accessible on the personal account  
C) EMU users cannot have personal repositories  
D) They are converted to enterprise repositories

---

**Question 7:** Which tool is used for GitHub Enterprise migrations?

A) GitHub Migrator  
B) GitHub Enterprise Importer (GEI)  
C) GitHub Transfer Tool  
D) GitHub Migration Assistant

---

**Question 8:** What is required before enabling SCIM provisioning?

A) GitHub Actions must be enabled  
B) SAML SSO must be configured  
C) All users must have 2FA enabled  
D) Enterprise must have 10+ organizations

---

## Answer Key

<details>
<summary>Click to reveal answers</summary>

1. **A** - SAML handles authentication (who you are), SCIM handles provisioning (create/update/delete users)

2. **B** - EMU is for organizations needing complete identity control and compliance

3. **B** - GitHub Connect bridges GHES and GHEC for hybrid deployments

4. **B** - The SCIM endpoint is on api.github.com

5. **C** - Elasticsearch is not a native streaming destination (can use S3 + Lambda)

6. **C** - EMU users have enterprise-controlled accounts with no personal repos

7. **B** - GitHub Enterprise Importer (gh-gei) is the official migration tool

8. **B** - SCIM requires SAML SSO to be configured first

</details>

---

## Self-Assessment

After completing this quiz, assess your understanding:

- **8/8 correct**: Excellent! You have a strong grasp of enterprise administration concepts.
- **6-7 correct**: Good understanding. Review the concepts you missed.
- **4-5 correct**: Fair understanding. Consider reviewing the Core Concepts section.
- **Below 4**: Review the entire module before proceeding.
