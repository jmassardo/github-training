---
layout: training-module
title: "Guided Walkthrough"
permalink: /advanced/module-11/walkthrough/
module_number: 11
module_title: "Infrastructure as Code & Operations"
section_number: 3
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
  url: /advanced/module-11/concepts/
  title: "Core Concepts"
next_section:
  url: /advanced/module-11/labs/
  title: "Hands-On Labs"
---

## Walkthrough: Building a GitHub IaC Solution

This walkthrough guides you through setting up Infrastructure as Code for GitHub from scratch, including project structure, authentication, and a complete GitOps workflow.

### Prerequisites

- Terraform installed (v1.5+)
- GitHub organization with admin access
- GitHub Personal Access Token or GitHub App

## Step 1: Project Setup

### Create Project Structure

```bash
mkdir github-infrastructure
cd github-infrastructure

# Create directory structure
mkdir -p terraform/{modules,repositories,teams}
mkdir -p .github/workflows

```

**Final structure:**

```
github-infrastructure/
├── terraform/
│   ├── main.tf
│   ├── variables.tf
│   ├── outputs.tf
│   ├── versions.tf
│   ├── modules/
│   │   └── standard-repository/
│   ├── repositories/
│   │   └── projects.tf
│   └── teams/
│       └── engineering.tf
├── .github/
│   └── workflows/
│       ├── terraform-plan.yml
│       └── terraform-apply.yml
└── README.md

```

### Initialize Terraform Configuration

Create `terraform/versions.tf`:

```hcl
terraform {
  required_version = ">= 1.5.0"
  
  required_providers {
    github = {
      source  = "integrations/github"
      version = "~> 6.0"
    }
  }
  
  # Remote state (configure for your backend)
  backend "s3" {
    bucket         = "your-terraform-state-bucket"
    key            = "github/terraform.tfstate"
    region         = "us-east-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"
  }
}

```

Create `terraform/variables.tf`:

```hcl
variable "github_organization" {
  description = "GitHub organization name"
  type        = string
}

variable "github_token" {
  description = "GitHub token for authentication"
  type        = string
  sensitive   = true
  default     = null  # Use GITHUB_TOKEN env var
}

variable "default_branch" {
  description = "Default branch name for repositories"
  type        = string
  default     = "main"
}

variable "required_reviewers" {
  description = "Number of required PR reviewers"
  type        = number
  default     = 1
}

```

Create `terraform/main.tf`:

```hcl
provider "github" {
  owner = var.github_organization
  token = var.github_token
}

# Import team configurations
module "teams" {
  source = "./teams"
}

# Import repository configurations
module "repositories" {
  source = "./repositories"
  
  depends_on = [module.teams]
}

```

## Step 2: Create Reusable Module

Create `terraform/modules/standard-repository/main.tf`:

```hcl
variable "name" {
  description = "Repository name"
  type        = string
}

variable "description" {
  description = "Repository description"
  type        = string
  default     = ""
}

variable "visibility" {
  description = "Repository visibility: public, private, or internal"
  type        = string
  default     = "private"
}

variable "team_id" {
  description = "Team ID for repository access"
  type        = string
  default     = null
}

variable "team_permission" {
  description = "Team permission level"
  type        = string
  default     = "push"
}

variable "template_repository" {
  description = "Template repository name (optional)"
  type        = string
  default     = null
}

variable "template_owner" {
  description = "Template repository owner"
  type        = string
  default     = null
}

variable "topics" {
  description = "Repository topics"
  type        = list(string)
  default     = []
}

# Create repository
resource "github_repository" "this" {
  name        = var.name
  description = var.description
  visibility  = var.visibility
  
  # Features
  has_issues      = true
  has_discussions = false
  has_projects    = false
  has_wiki        = false
  
  # Merge settings
  allow_merge_commit     = false
  allow_squash_merge     = true
  allow_rebase_merge     = true
  delete_branch_on_merge = true
  
  squash_merge_commit_title   = "PR_TITLE"
  squash_merge_commit_message = "PR_BODY"
  
  # Security
  vulnerability_alerts                    = true
  security_and_analysis {
    secret_scanning {
      status = "enabled"
    }
    secret_scanning_push_protection {
      status = "enabled"
    }
  }
  
  # Template (if specified)
  dynamic "template" {
    for_each = var.template_repository != null ? [1] : []
    content {
      owner      = var.template_owner
      repository = var.template_repository
    }
  }
  
  topics = var.topics
  
  lifecycle {
    prevent_destroy = true  # Prevent accidental deletion
  }
}

# Branch protection for main branch
resource "github_branch_protection" "main" {
  repository_id = github_repository.this.node_id
  pattern       = "main"
  
  # Require PR reviews
  required_pull_request_reviews {
    required_approving_review_count = 1
    dismiss_stale_reviews           = true
    require_code_owner_reviews      = true
    require_last_push_approval      = true
  }
  
  # Require status checks to pass
  required_status_checks {
    strict = true
  }
  
  # Enforce for everyone including admins
  enforce_admins = true
  
  # Require linear history (no merge commits)
  required_linear_history = true
  
  # Require conversation resolution
  require_conversation_resolution = true
}

# Team access (if team_id provided)
resource "github_team_repository" "team_access" {
  count = var.team_id != null ? 1 : 0
  
  team_id    = var.team_id
  repository = github_repository.this.name
  permission = var.team_permission
}

# Outputs
output "repository_name" {
  value = github_repository.this.name
}

output "repository_url" {
  value = github_repository.this.html_url
}

output "repository_node_id" {
  value = github_repository.this.node_id
}

```

## Step 3: Define Teams

Create `terraform/teams/main.tf`:

```hcl
# Engineering teams
resource "github_team" "platform" {
  name        = "platform-engineering"
  description = "Platform Engineering Team"
  privacy     = "closed"
}

resource "github_team" "backend" {
  name        = "backend-engineers"
  description = "Backend Engineering Team"
  privacy     = "closed"
}

resource "github_team" "frontend" {
  name        = "frontend-engineers"
  description = "Frontend Engineering Team"
  privacy     = "closed"
}

# Team hierarchy
resource "github_team" "all_engineers" {
  name        = "all-engineers"
  description = "All Engineering Staff"
  privacy     = "closed"
}

# Outputs for use in other modules
output "platform_team_id" {
  value = github_team.platform.id
}

output "backend_team_id" {
  value = github_team.backend.id
}

output "frontend_team_id" {
  value = github_team.frontend.id
}

```

## Step 4: Define Repositories

Create `terraform/repositories/main.tf`:

```hcl
variable "platform_team_id" {
  type = string
}

variable "backend_team_id" {
  type = string
}

variable "frontend_team_id" {
  type = string
}

# Platform repositories
module "infrastructure" {
  source = "../modules/standard-repository"
  
  name        = "infrastructure"
  description = "Infrastructure as Code repository"
  team_id     = var.platform_team_id
  topics      = ["infrastructure", "terraform", "iac"]
}

module "platform_tools" {
  source = "../modules/standard-repository"
  
  name        = "platform-tools"
  description = "Internal platform tooling"
  team_id     = var.platform_team_id
  topics      = ["platform", "internal-tools"]
}

# Backend repositories
module "api_gateway" {
  source = "../modules/standard-repository"
  
  name        = "api-gateway"
  description = "API Gateway service"
  team_id     = var.backend_team_id
  topics      = ["api", "backend", "service"]
}

module "user_service" {
  source = "../modules/standard-repository"
  
  name        = "user-service"
  description = "User management service"
  team_id     = var.backend_team_id
  topics      = ["microservice", "backend"]
}

# Frontend repositories
module "web_app" {
  source = "../modules/standard-repository"
  
  name        = "web-app"
  description = "Main web application"
  team_id     = var.frontend_team_id
  topics      = ["frontend", "react", "web"]
}

```

## Step 5: GitOps Workflow

Create `.github/workflows/terraform-plan.yml`:

```yaml
name: Terraform Plan

on:
  pull_request:
    paths:
      - 'terraform/**'
      - '.github/workflows/terraform-*.yml'

permissions:
  contents: read
  pull-requests: write

jobs:
  plan:
    name: Terraform Plan
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        with:
          terraform_version: 1.6.0
          
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_ROLE_ARN }}
          aws-region: us-east-1
          
      - name: Terraform Init
        id: init
        run: terraform init
        working-directory: terraform
        
      - name: Terraform Format Check
        id: fmt
        run: terraform fmt -check -recursive
        working-directory: terraform
        continue-on-error: true
        
      - name: Terraform Validate
        id: validate
        run: terraform validate
        working-directory: terraform
        
      - name: Terraform Plan
        id: plan
        run: terraform plan -no-color -out=tfplan
        working-directory: terraform
        env:
          GITHUB_TOKEN: ${{ secrets.GH_ADMIN_TOKEN }}
        continue-on-error: true
        
      - name: Post Plan to PR
        uses: actions/github-script@v7
        with:
          script: |
            const output = `#### Terraform Format 🖌 \`${{ steps.fmt.outcome }}\`
            #### Terraform Init ⚙️ \`${{ steps.init.outcome }}\`
            #### Terraform Validate 🤖 \`${{ steps.validate.outcome }}\`
            #### Terraform Plan 📖 \`${{ steps.plan.outcome }}\`
            
            <details><summary>Show Plan</summary>
            
            \`\`\`terraform
            ${{ steps.plan.outputs.stdout }}
            \`\`\`
            
            </details>
            
            *Pushed by: @${{ github.actor }}, Action: \`${{ github.event_name }}\`*`;
            
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: output
            })

```

Create `.github/workflows/terraform-apply.yml`:

```yaml
name: Terraform Apply

on:
  push:
    branches:
      - main
    paths:
      - 'terraform/**'

permissions:
  contents: read

jobs:
  apply:
    name: Terraform Apply
    runs-on: ubuntu-latest
    environment: production
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        with:
          terraform_version: 1.6.0
          
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_ROLE_ARN }}
          aws-region: us-east-1
          
      - name: Terraform Init
        run: terraform init
        working-directory: terraform
        
      - name: Terraform Apply
        run: terraform apply -auto-approve
        working-directory: terraform
        env:
          GITHUB_TOKEN: ${{ secrets.GH_ADMIN_TOKEN }}

```

## Step 6: Initialize and Test

```bash
# Set environment variables
export GITHUB_TOKEN="ghp_your_token_here"
export TF_VAR_github_organization="your-org"

# Initialize Terraform
cd terraform
terraform init

# Format and validate
terraform fmt -recursive
terraform validate

# Plan changes
terraform plan

# Apply (when ready)
terraform apply

```

## Walkthrough Summary

You've now built:
1. ✅ Modular Terraform project structure
2. ✅ Reusable repository module with security defaults
3. ✅ Team configuration with hierarchy
4. ✅ GitOps workflow with PR-based changes
5. ✅ Automated plan and apply via GitHub Actions

---

**Next:** Practice these concepts hands-on in the Labs section.
