---
layout: training-module
title: "Resources"
permalink: /advanced/module-11/resources/
module_number: 11
module_title: "Infrastructure as Code & Operations"
section_number: 6
total_sections: 7
phase: advanced
estimated_time: "10 min"
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
  url: /advanced/module-11/quiz/
  title: "Knowledge Check"
next_section:
  url: /advanced/module-11/scenarios/
  title: "CSM Scenarios"
---

## Official Documentation

### GitHub Terraform Provider

| Resource | Description |
|----------|-------------|
| [Provider Registry](https://registry.terraform.io/providers/integrations/github/latest) | Official provider documentation |
| [GitHub Provider Docs](https://registry.terraform.io/providers/integrations/github/latest/docs) | Resource and data source reference |
| [Provider GitHub Repo](https://github.com/integrations/terraform-provider-github) | Source code and issue tracker |

### Terraform

| Resource | Description |
|----------|-------------|
| [Terraform Documentation](https://developer.hashicorp.com/terraform/docs) | Official Terraform docs |
| [Terraform Best Practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices) | HashiCorp recommended patterns |
| [State Management](https://developer.hashicorp.com/terraform/language/state) | Understanding Terraform state |
| [Module Development](https://developer.hashicorp.com/terraform/language/modules/develop) | Creating reusable modules |

### Pulumi (Alternative)

| Resource | Description |
|----------|-------------|
| [Pulumi GitHub Provider](https://www.pulumi.com/registry/packages/github/) | GitHub management with programming languages |
| [Pulumi vs Terraform](https://www.pulumi.com/docs/concepts/vs/terraform/) | Comparison guide |

## Reference Architectures

### Example Repositories

| Repository | Description |
|------------|-------------|
| [terraform-github-organization](https://github.com/mineiros-io/terraform-github-organization) | Community module for org management |
| [github-terraform-demo](https://github.com/integrations/terraform-provider-github/tree/main/examples) | Official provider examples |

### Architecture Patterns

```
Pattern 1: Monolithic Configuration
└── github-config/
    └── terraform/
        └── main.tf  (all resources in one file)

Pattern 2: Resource-Based Organization
└── github-config/
    └── terraform/
        ├── repositories.tf
        ├── teams.tf
        └── branch-protection.tf

Pattern 3: Team-Based Organization (Recommended for scale)
└── github-config/
    └── terraform/
        ├── teams/
        │   ├── platform/
        │   └── product/
        └── modules/
            └── standard-repo/

Pattern 4: Self-Service (Enterprise scale)
└── github-platform/
    ├── modules/           # Shared modules
    ├── core/              # Platform team managed
    └── requests/          # Developer self-service

```

## Tools and Utilities

### Terraform Tools

| Tool | Purpose |
|------|---------|
| [tflint](https://github.com/terraform-linters/tflint) | Terraform linter |
| [terraform-docs](https://terraform-docs.io/) | Auto-generate module documentation |
| [checkov](https://www.checkov.io/) | Security scanning for Terraform |
| [infracost](https://www.infracost.io/) | Cost estimation (for cloud resources) |
| [terragrunt](https://terragrunt.gruntwork.io/) | Terraform wrapper for DRY configs |

### GitHub Tools

| Tool | Purpose |
|------|---------|
| [GitHub CLI (gh)](https://cli.github.com/) | Command-line GitHub access |
| [github-scripts](https://github.com/actions/github-script) | GitHub API in Actions |
| [octokit](https://github.com/octokit) | GitHub API client libraries |

## GitOps Resources

### Learning Materials

| Resource | Type |
|----------|------|
| [GitOps Principles](https://opengitops.dev/) | OpenGitOps foundation |
| [Weaveworks GitOps](https://www.weave.works/technologies/gitops/) | GitOps concept originators |
| [GitOps for GitHub](https://github.blog/2020-12-17-commits-are-snapshots-not-diffs/) | Git mental model |

### CI/CD Integration

| Platform | Integration Guide |
|----------|------------------|
| GitHub Actions | [Terraform Actions](https://github.com/hashicorp/setup-terraform) |
| GitLab CI | [Terraform GitLab Template](https://docs.gitlab.com/ee/user/infrastructure/iac/) |
| Azure DevOps | [Terraform Extension](https://marketplace.visualstudio.com/items?itemName=ms-devlabs.custom-terraform-tasks) |

## Security Resources

### Authentication Best Practices

| Topic | Resource |
|-------|----------|
| GitHub Apps | [Creating GitHub Apps](https://docs.github.com/en/apps/creating-github-apps/about-creating-github-apps/about-creating-github-apps) |
| OIDC for Actions | [Configuring OIDC](https://docs.github.com/en/actions/deployment/security-hardening-your-deployments/configuring-openid-connect-in-amazon-web-services) |
| Token Permissions | [GitHub Token Scopes](https://docs.github.com/en/apps/oauth-apps/building-oauth-apps/scopes-for-oauth-apps) |

### Compliance

| Standard | GitHub Relevance |
|----------|------------------|
| SOC 2 | [GitHub SOC 2 Report](https://github.com/security) |
| FedRAMP | [GitHub Government](https://government.github.com/) |
| GDPR | [GitHub Privacy](https://docs.github.com/en/site-policy/privacy-policies/github-privacy-statement) |

## Video Resources

### Conference Talks

- **HashiConf**: Search for "GitHub Terraform" presentations
- **GitHub Universe**: Enterprise administration sessions
- **DevOps Enterprise Summit**: GitOps adoption case studies

### Tutorials

| Platform | Search Terms |
|----------|--------------|
| YouTube | "Terraform GitHub Provider tutorial" |
| Pluralsight | "Managing GitHub with Terraform" |
| LinkedIn Learning | "Infrastructure as Code GitHub" |

## Community Resources

### Forums and Support

| Resource | Type |
|----------|------|
| [Terraform GitHub Provider Issues](https://github.com/integrations/terraform-provider-github/issues) | Bug reports, feature requests |
| [HashiCorp Discuss](https://discuss.hashicorp.com/c/terraform-providers/tf-github/) | Community Q&A |
| [GitHub Community](https://github.community/) | GitHub platform questions |

### Slack/Discord

- HashiCorp Community Slack
- Platform Engineering Slack communities
- DevOps-related Discord servers

## Quick Reference Card

### Common Terraform Commands

```bash
# Initialize
terraform init

# Format code
terraform fmt -recursive

# Validate configuration
terraform validate

# Plan changes
terraform plan -out=tfplan

# Apply changes
terraform apply tfplan

# Import existing resource
terraform import github_repository.name org/repo

# Show state
terraform state list
terraform state show github_repository.name

# Destroy (careful!)
terraform destroy

```

### GitHub Provider Resources

| Resource | Import Syntax |
|----------|--------------|
| Repository | `github_repository.name org/repo-name` |
| Team | `github_team.name team-id` |
| Branch Protection | `github_branch_protection.name org/repo:branch` |
| Team Membership | `github_team_membership.name team-id:username` |
| Organization Settings | `github_organization_settings.name org-name` |

---

**Next:** Practice customer conversations in the CSM Scenarios section.
