---
layout: training-module
title: "Core Concepts"
permalink: /advanced/module-11/concepts/
module_number: 11
module_title: "Infrastructure as Code & Operations"
section_number: 2
total_sections: 7
phase: advanced
estimated_time: "30 min"
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
  url: /advanced/module-11/overview/
  title: "Context & Overview"
next_section:
  url: /advanced/module-11/walkthrough/
  title: "Guided Walkthrough"
---

## GitHub Terraform Provider Deep Dive

The GitHub Terraform Provider enables declarative management of GitHub resources including repositories, teams, branch protection rules, and organization settings.

### Provider Architecture

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph TF["Terraform Workflow"]
    TFFiles["📄 .tf Files<br/>(Desired State)"] --> Core["⚙️ Terraform<br/>Core"]
    Core --> API["🔌 GitHub<br/>API"]
    Core --> State["💾 State File<br/>(Current)"]
  end
</div>
</div>

### Provider Configuration

```hcl
# Configure the GitHub Provider
terraform {
  required_providers {
    github = {
      source  = "integrations/github"
      version = "~> 6.0"
    }
  }
}

provider "github" {
  owner = "my-organization"  # GitHub organization
  # Token from environment variable GITHUB_TOKEN
}

```

**Authentication Options:**

| Method | Use Case | Configuration |
|--------|----------|---------------|
| Personal Access Token | Development, simple setups | `GITHUB_TOKEN` env var |
| GitHub App | Production, fine-grained permissions | App ID + Private Key |
| OIDC | CI/CD pipelines (Actions) | Workload identity |

### Key Resources

#### Repository Management

```hcl
resource "github_repository" "application" {
  name        = "my-application"
  description = "Production application"
  visibility  = "private"
  
  # Repository features
  has_issues      = true
  has_discussions = true
  has_projects    = true
  has_wiki        = false
  
  # Merge settings
  allow_merge_commit     = false
  allow_squash_merge     = true
  allow_rebase_merge     = true
  delete_branch_on_merge = true
  squash_merge_commit_title   = "PR_TITLE"
  squash_merge_commit_message = "PR_BODY"
  
  # Security
  vulnerability_alerts = true
  
  # Template (optional)
  template {
    owner      = "my-organization"
    repository = "repo-template"
  }
}

```

#### Branch Protection Rules

```hcl
resource "github_branch_protection" "main" {
  repository_id = github_repository.application.node_id
  pattern       = "main"
  
  # Require PR reviews
  required_pull_request_reviews {
    required_approving_review_count = 2
    dismiss_stale_reviews           = true
    require_code_owner_reviews      = true
    require_last_push_approval      = true
  }
  
  # Require status checks
  required_status_checks {
    strict   = true  # Require branch to be up to date
    contexts = ["ci/build", "ci/test", "security/scan"]
  }
  
  # Additional protections
  enforce_admins                  = true
  require_signed_commits          = true
  required_linear_history         = true
  require_conversation_resolution = true
  
  # Restrict who can push
  restrict_pushes {
    push_allowances = [
      "my-organization/release-engineers"
    ]
  }
}

```

#### Team Management

```hcl
resource "github_team" "platform" {
  name        = "platform-engineering"
  description = "Platform Engineering Team"
  privacy     = "closed"
}

resource "github_team_membership" "platform_members" {
  for_each = toset(["user1", "user2", "user3"])
  
  team_id  = github_team.platform.id
  username = each.value
  role     = "member"
}

resource "github_team_repository" "platform_repos" {
  for_each = toset([
    github_repository.application.name,
    github_repository.infrastructure.name
  ])
  
  team_id    = github_team.platform.id
  repository = each.value
  permission = "push"
}

```

## State Management

Terraform state tracks the mapping between your configuration and real GitHub resources.

### Remote State Storage

**Never store state locally in production.** Use remote backends:

```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "github/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}

```

**Alternative backends:**

- **Azure Blob Storage** - For Azure-centric organizations
- **Google Cloud Storage** - For GCP users
- **Terraform Cloud** - Managed solution with collaboration features
- **HashiCorp Consul** - Self-hosted option

### State Operations

```bash
# View current state
terraform state list

# Show specific resource
terraform state show github_repository.application

# Import existing resource into state
terraform import github_repository.application my-organization/my-application

# Remove resource from state (without destroying)
terraform state rm github_repository.old_repo

# Move resource in state (refactoring)
terraform state mv github_repository.old github_repository.new

```

### Importing Existing Resources

For organizations with existing GitHub resources:

```bash
# Import existing repository
terraform import github_repository.existing my-org/existing-repo

# Import team
terraform import github_team.existing 1234567

# Import branch protection
terraform import github_branch_protection.main my-org/repo:main

```

## GitOps Implementation Patterns

### Pattern 1: Centralized Configuration Repository

```
github-config/
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── repositories/
│   │   ├── team-alpha.tf
│   │   ├── team-beta.tf
│   │   └── shared.tf
│   └── teams/
│       ├── engineering.tf
│       └── security.tf
├── .github/
│   └── workflows/
│       └── terraform.yml
└── README.md

```

### Pattern 2: Self-Service with Pull Requests

```yaml
# .github/workflows/terraform.yml
name: Terraform

on:
  pull_request:
    paths:
      - 'terraform/**'
  push:
    branches:
      - main
    paths:
      - 'terraform/**'

jobs:
  plan:
    runs-on: ubuntu-latest
    if: github.event_name == 'pull_request'
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        
      - name: Terraform Init
        run: terraform init
        working-directory: terraform
        
      - name: Terraform Plan
        run: terraform plan -out=tfplan
        working-directory: terraform
        env:
          GITHUB_TOKEN: ${{ secrets.GH_ADMIN_TOKEN }}
          
      - name: Post Plan to PR
        uses: actions/github-script@v7
        with:
          script: |
            // Post plan output as PR comment

  apply:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        
      - name: Terraform Init
        run: terraform init
        working-directory: terraform
        
      - name: Terraform Apply
        run: terraform apply -auto-approve
        working-directory: terraform
        env:
          GITHUB_TOKEN: ${{ secrets.GH_ADMIN_TOKEN }}

```

### Pattern 3: Drift Detection

```yaml
# .github/workflows/drift-detection.yml
name: Drift Detection

on:
  schedule:
    - cron: '0 */6 * * *'  # Every 6 hours
  workflow_dispatch:

jobs:
  detect-drift:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        
      - name: Terraform Init
        run: terraform init
        working-directory: terraform
        
      - name: Terraform Plan
        id: plan
        run: |
          terraform plan -detailed-exitcode -out=tfplan 2>&1 | tee plan.txt
          echo "exitcode=$?" >> $GITHUB_OUTPUT
        working-directory: terraform
        continue-on-error: true
        env:
          GITHUB_TOKEN: ${{ secrets.GH_ADMIN_TOKEN }}
          
      - name: Alert on Drift
        if: steps.plan.outputs.exitcode == '2'
        run: |
          # Send alert via Slack, email, or create issue
          gh issue create \
            --title "Configuration Drift Detected" \
            --body "$(cat terraform/plan.txt)"
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}

```

## Modular Terraform Design

### Using Modules for Consistency

```hcl
# modules/standard-repository/main.tf
variable "name" {
  type = string
}

variable "team" {
  type = string
}

variable "visibility" {
  type    = string
  default = "private"
}

resource "github_repository" "this" {
  name        = var.name
  visibility  = var.visibility
  
  # Standard settings
  has_issues             = true
  delete_branch_on_merge = true
  vulnerability_alerts   = true
  allow_squash_merge     = true
  allow_merge_commit     = false
}

resource "github_branch_protection" "main" {
  repository_id = github_repository.this.node_id
  pattern       = "main"
  
  required_pull_request_reviews {
    required_approving_review_count = 1
    dismiss_stale_reviews           = true
  }
  
  required_status_checks {
    strict = true
  }
}

resource "github_team_repository" "team_access" {
  team_id    = var.team
  repository = github_repository.this.name
  permission = "push"
}

```

```hcl
# repositories/team-alpha.tf
module "service_api" {
  source = "../modules/standard-repository"
  
  name   = "service-api"
  team   = github_team.alpha.id
}

module "service_web" {
  source = "../modules/standard-repository"
  
  name   = "service-web"
  team   = github_team.alpha.id
}

```

## Security Considerations

### Token Management

| Approach | Security Level | Use Case |
|----------|---------------|----------|
| PAT in env var | Low | Local development only |
| GitHub App | High | Production automation |
| OIDC | Highest | GitHub Actions pipelines |

### GitHub App for Terraform

```hcl
provider "github" {
  owner = "my-organization"
  app_auth {
    id              = var.github_app_id
    installation_id = var.github_app_installation_id
    pem_file        = var.github_app_pem_file
  }
}

```

### Least Privilege Permissions

Configure GitHub App with minimal required permissions:

- `administration: write` - Repository settings
- `members: write` - Team management
- `organization_administration: write` - Org settings

---

**Next:** Walk through a complete implementation in the Guided Walkthrough section.
