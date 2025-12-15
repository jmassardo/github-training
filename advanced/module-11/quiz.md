---
layout: training-module
title: "Knowledge Check"
permalink: /advanced/module-11/quiz/
module_number: 11
module_title: "Infrastructure as Code & Operations"
section_number: 5
total_sections: 7
phase: advanced
estimated_time: "15 min"
module_index: /advanced/module-11/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-11/overview/"
    short_title: "Overview"
    icon: "📋"
  - title: "Core Concepts"
    url: "/advanced/module-11/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/advanced/module-11/walkthrough/"
    short_title: "Walkthrough"
    icon: "🔄"
  - title: "Hands-On Labs"
    url: "/advanced/module-11/labs/"
    short_title: "Labs"
    icon: "🧪"
  - title: "Knowledge Check"
    url: "/advanced/module-11/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/advanced/module-11/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/advanced/module-11/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
prev_section:
  url: /advanced/module-11/labs/
  title: "Hands-On Labs"
next_section:
  url: /advanced/module-11/resources/
  title: "Resources"
---

## Knowledge Check

Test your understanding of Infrastructure as Code concepts for GitHub administration.

---

### Question 1

**What is the primary benefit of using Terraform state for GitHub management?**

<details>
<summary>Show Answer</summary>

**Terraform state tracks the mapping between your configuration and real GitHub resources.** This enables:
- Detecting drift between desired and actual configuration
- Planning changes before applying them
- Managing dependencies between resources
- Knowing which resources Terraform manages vs. which exist independently

Without state, Terraform wouldn't know if a repository already exists or what its current settings are.
</details>

---

### Question 2

**A customer has 500 existing repositories they want to manage with Terraform. What's the recommended approach?**

A) Delete all repositories and recreate them with Terraform  
B) Use `terraform import` to bring existing resources into state  
C) Create new Terraform resources with the same names  
D) Terraform can only manage new resources

<details>
<summary>Show Answer</summary>

**B) Use `terraform import` to bring existing resources into state**

The import process:
1. Define the resource in Terraform configuration
2. Run `terraform import github_repository.name org/repo-name`
3. Update configuration to match imported state
4. Verify with `terraform plan` showing no changes

This preserves existing repositories while enabling Terraform management going forward.
</details>

---

### Question 3

**Which authentication method is recommended for production Terraform automation?**

A) Personal Access Token stored in code  
B) Personal Access Token in environment variable  
C) GitHub App with installation token  
D) OAuth user token

<details>
<summary>Show Answer</summary>

**C) GitHub App with installation token**

GitHub Apps provide:
- **Fine-grained permissions** - Only grant access needed
- **Organization-scoped** - Not tied to individual user
- **Audit logging** - Actions attributed to app, not user
- **No token expiration concerns** - Installation tokens auto-refresh
- **Rate limit benefits** - Higher limits than PATs

For GitHub Actions, OIDC (OpenID Connect) is even better as it eliminates stored secrets entirely.
</details>

---

### Question 4

**What does "drift" mean in the context of GitOps for GitHub?**

<details>
<summary>Show Answer</summary>

**Drift occurs when the actual GitHub configuration differs from the desired state defined in code.**

Examples of drift:
- Someone manually changes branch protection rules via UI
- A repository setting is modified outside of Terraform
- Team membership is updated directly in GitHub

Drift detection compares the Terraform state against the live GitHub API to identify these differences. GitOps practices aim to eliminate drift by:
- Making all changes through code (pull requests)
- Running regular drift detection checks
- Alerting or auto-remediating when drift is found
</details>

---

### Question 5

**A customer asks: "Why should we use Infrastructure as Code instead of the GitHub UI or API scripts?"**

What are the key advantages of IaC you should highlight?

<details>
<summary>Show Answer</summary>

**Key IaC advantages over UI/scripts:**

| Aspect | UI/Scripts | Infrastructure as Code |
|--------|-----------|------------------------|
| **Consistency** | Manual, error-prone | Guaranteed identical config |
| **Change History** | Limited audit log | Full Git history with diffs |
| **Review Process** | None | Pull request review |
| **Rollback** | Manual reconstruction | Git revert |
| **Documentation** | Separate, often outdated | Code IS documentation |
| **Reproducibility** | Tribal knowledge | Anyone can run `terraform apply` |
| **Scale** | Linear effort increase | Same effort for 10 or 1000 repos |
| **Testing** | Manual verification | Automated validation |
</details>

---

### Question 6

**Which Terraform command shows what changes would be made without applying them?**

A) `terraform show`  
B) `terraform plan`  
C) `terraform validate`  
D) `terraform preview`

<details>
<summary>Show Answer</summary>

**B) `terraform plan`**

The Terraform workflow:
1. `terraform init` - Initialize providers and backend
2. `terraform validate` - Check syntax and configuration validity
3. `terraform plan` - **Show proposed changes without applying**
4. `terraform apply` - Execute the changes

`terraform plan -out=tfplan` saves the plan to a file that can be applied exactly as reviewed, ensuring the apply matches what was approved.
</details>

---

### Question 7

**What is the purpose of a Terraform module?**

<details>
<summary>Show Answer</summary>

**A Terraform module is a reusable container for multiple resources that are used together.**

Benefits:
- **Consistency** - Same configuration applied everywhere
- **Abstraction** - Hide complexity behind simple interface
- **Maintainability** - Update in one place, apply everywhere
- **Standards enforcement** - Security settings baked in

Example: A "standard-repository" module that always creates repos with:
- Branch protection enabled
- Vulnerability alerts on
- Consistent merge settings
- Required code review

Teams use the module without knowing (or being able to change) these details.
</details>

---

### Question 8

**A customer's security team wants to ensure no one can bypass branch protection rules. How can IaC help?**

<details>
<summary>Show Answer</summary>

**IaC provides multiple layers of protection:**

1. **Drift Detection** - Scheduled jobs detect if branch protection is modified/removed
2. **Auto-Remediation** - Terraform apply restores required settings
3. **Audit Trail** - Git history shows all legitimate changes
4. **Change Control** - All modifications require PR approval
5. **Enforce Admins** - Terraform sets `enforce_admins = true` preventing even admins from bypassing

```hcl
resource "github_branch_protection" "main" {
  # ...
  enforce_admins = true  # Even admins must follow rules
}

```

Combined with alerting, any unauthorized change is detected and flagged within minutes.
</details>

---

### Question 9

**What's the recommended approach for managing Terraform state in a team environment?**

A) Store state file in the Git repository  
B) Each team member maintains their own state  
C) Use a remote backend with state locking  
D) Email state files between team members

<details>
<summary>Show Answer</summary>

**C) Use a remote backend with state locking**

Remote state provides:
- **Shared access** - Team members see same state
- **Locking** - Prevents concurrent modifications
- **Encryption** - State often contains sensitive data
- **Versioning** - Recovery from state corruption
- **Backup** - Cloud storage durability

Common backends:
- AWS S3 with DynamoDB locking
- Azure Blob Storage
- Google Cloud Storage
- Terraform Cloud/Enterprise
- HashiCorp Consul

**Never commit state to Git** - it may contain secrets and causes merge conflicts.
</details>

---

### Question 10

**Match the GitOps principle to its benefit:**

| Principle | Benefit |
|-----------|---------|
| Declarative | ? |
| Versioned | ? |
| Automated | ? |
| Observable | ? |

<details>
<summary>Show Answer</summary>

| Principle | Benefit |
|-----------|---------|
| **Declarative** | Describe desired end state, not steps to get there |
| **Versioned** | Complete history of all changes in Git |
| **Automated** | CI/CD applies changes without manual intervention |
| **Observable** | Drift detection alerts when reality differs from code |

Together, these principles create a system where:
- Configuration is always documented (code)
- Changes are always reviewed (PRs)
- Deployment is always consistent (automation)
- Problems are always detected (observability)
</details>

---

## Score Yourself

| Score | Assessment |
|-------|------------|
| 9-10 | Excellent! Ready to help customers implement IaC |
| 7-8 | Good understanding, review weak areas |
| 5-6 | Revisit Core Concepts and Walkthrough |
| <5 | Complete the module again before proceeding |

---

**Next:** Explore additional Resources for deeper learning.
