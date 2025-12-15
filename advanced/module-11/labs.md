---
layout: training-module
title: "Hands-On Labs"
permalink: /advanced/module-11/labs/
module_number: 11
module_title: "Infrastructure as Code & Operations"
section_number: 4
total_sections: 7
phase: advanced
estimated_time: "45 min"
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
  url: /advanced/module-11/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /advanced/module-11/quiz/
  title: "Knowledge Check"
---

## Lab Overview

These hands-on labs will help you practice Infrastructure as Code concepts for GitHub administration. Complete them in order, as each builds on the previous.

| Lab | Topic | Time | Difficulty |
|-----|-------|------|------------|
| Lab 1 | Import Existing Repository | 15 min | Beginner |
| Lab 2 | Create Repository Module | 15 min | Intermediate |
| Lab 3 | Implement Drift Detection | 10 min | Intermediate |
| Lab 4 | Build Self-Service Workflow | 15 min | Advanced |

---

## Lab 1: Import an Existing Repository

**Objective:** Import an existing GitHub repository into Terraform state and manage it declaratively.

### Setup

1. Create a new directory for this lab:

```bash
mkdir lab1-import && cd lab1-import

```

2. Create `main.tf`:

```hcl
terraform {
  required_providers {
    github = {
      source  = "integrations/github"
      version = "~> 6.0"
    }
  }
}

provider "github" {
  owner = "YOUR_ORG_OR_USERNAME"
}

# We'll import into this resource
resource "github_repository" "imported" {
  name = "EXISTING_REPO_NAME"
  
  # Start with minimal config - we'll update after import
}

```

### Steps

**Step 1:** Set your GitHub token:

```bash
export GITHUB_TOKEN="ghp_your_token"

```

**Step 2:** Initialize Terraform:

```bash
terraform init

```

**Step 3:** Import the existing repository:

```bash
terraform import github_repository.imported YOUR_ORG/EXISTING_REPO_NAME

```

**Step 4:** View the imported state:

```bash
terraform state show github_repository.imported

```

**Step 5:** Update your configuration to match the imported state. Copy the relevant attributes from the state output into your `main.tf`.

**Step 6:** Run plan to verify no changes:

```bash
terraform plan

```

### Verification

✅ `terraform plan` shows "No changes" or only desired changes  
✅ You can see the repository in `terraform state list`  
✅ You understand which attributes were imported

### Challenge

Try importing a branch protection rule for the same repository:

```bash
terraform import github_branch_protection.main YOUR_ORG/REPO_NAME:main

```

---

## Lab 2: Create a Reusable Repository Module

**Objective:** Build a reusable Terraform module that creates repositories with consistent security settings.

### Setup

1. Create project structure:

```bash
mkdir -p lab2-module/modules/secure-repo
cd lab2-module

```

### Steps

**Step 1:** Create the module in `modules/secure-repo/main.tf`:

```hcl
variable "name" {
  type        = string
  description = "Repository name"
}

variable "description" {
  type        = string
  description = "Repository description"
  default     = ""
}

variable "visibility" {
  type        = string
  description = "Repository visibility"
  default     = "private"
  
  validation {
    condition     = contains(["public", "private", "internal"], var.visibility)
    error_message = "Visibility must be public, private, or internal."
  }
}

variable "required_approvals" {
  type        = number
  description = "Number of required PR approvals"
  default     = 1
}

resource "github_repository" "this" {
  name        = var.name
  description = var.description
  visibility  = var.visibility
  
  # Security defaults
  vulnerability_alerts   = true
  delete_branch_on_merge = true
  allow_squash_merge     = true
  allow_merge_commit     = false
  
  has_issues   = true
  has_wiki     = false
  has_projects = false
}

resource "github_branch_protection" "main" {
  repository_id = github_repository.this.node_id
  pattern       = "main"
  
  required_pull_request_reviews {
    required_approving_review_count = var.required_approvals
    dismiss_stale_reviews           = true
    require_last_push_approval      = true
  }
  
  enforce_admins          = true
  required_linear_history = true
}

output "repository_url" {
  value = github_repository.this.html_url
}

```

**Step 2:** Create the root module in `main.tf`:

```hcl
terraform {
  required_providers {
    github = {
      source  = "integrations/github"
      version = "~> 6.0"
    }
  }
}

provider "github" {
  owner = "YOUR_ORG"
}

module "service_alpha" {
  source = "./modules/secure-repo"
  
  name        = "service-alpha-lab"
  description = "Lab test repository"
  visibility  = "private"
}

module "service_beta" {
  source = "./modules/secure-repo"
  
  name               = "service-beta-lab"
  description        = "Lab test repository with 2 reviewers"
  required_approvals = 2
}

output "repos_created" {
  value = {
    alpha = module.service_alpha.repository_url
    beta  = module.service_beta.repository_url
  }
}

```

**Step 3:** Initialize and apply:

```bash
terraform init
terraform plan
terraform apply

```

### Verification

✅ Two repositories created with consistent settings  
✅ Branch protection rules applied automatically  
✅ Module accepts different parameters for customization

### Challenge

Extend the module to:

- Add CODEOWNERS file automatically
- Configure GitHub Actions permissions
- Set up required status checks

---

## Lab 3: Implement Drift Detection

**Objective:** Create a GitHub Actions workflow that detects when GitHub configuration drifts from the Terraform state.

### Setup

Use the project from Lab 2 or create a new one.

### Steps

**Step 1:** Create `.github/workflows/drift-detection.yml`:

```yaml
name: Configuration Drift Detection

on:
  schedule:
    - cron: '0 8 * * *'  # Daily at 8 AM
  workflow_dispatch:      # Manual trigger

permissions:
  contents: read
  issues: write

jobs:
  detect-drift:
    name: Detect Configuration Drift
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        with:
          terraform_version: 1.6.0
          
      - name: Terraform Init
        run: terraform init
        
      - name: Terraform Plan (Drift Check)
        id: plan
        run: |
          set +e
          terraform plan -detailed-exitcode -no-color > plan_output.txt 2>&1
          EXIT_CODE=$?
          echo "exit_code=$EXIT_CODE" >> $GITHUB_OUTPUT
          
          if [ $EXIT_CODE -eq 0 ]; then
            echo "status=no-drift" >> $GITHUB_OUTPUT
          elif [ $EXIT_CODE -eq 2 ]; then
            echo "status=drift-detected" >> $GITHUB_OUTPUT
          else
            echo "status=error" >> $GITHUB_OUTPUT
          fi
        env:
          GITHUB_TOKEN: ${{ secrets.GH_ADMIN_TOKEN }}
          
      - name: Create Issue on Drift
        if: steps.plan.outputs.status == 'drift-detected'
        uses: actions/github-script@v7
        with:
          script: |
            const fs = require('fs');
            const planOutput = fs.readFileSync('plan_output.txt', 'utf8');
            
            await github.rest.issues.create({
              owner: context.repo.owner,
              repo: context.repo.repo,
              title: '🚨 Configuration Drift Detected',
              body: `## Drift Detection Report
              
              The scheduled drift detection found differences between the desired state (Terraform) and the actual GitHub configuration.
              
              ### Terraform Plan Output
              
              \`\`\`
              ${planOutput.substring(0, 60000)}
              \`\`\`
              
              ### Action Required
              
              1. Review the changes above
              2. If changes are intentional, update Terraform configuration
              3. If changes are unauthorized, investigate and remediate
              
              ---
              *Generated by drift detection workflow*`,
              labels: ['drift', 'infrastructure', 'automated']
            });
            
      - name: Log No Drift
        if: steps.plan.outputs.status == 'no-drift'
        run: echo "✅ No configuration drift detected"

```

**Step 2:** Test the workflow:
1. Push to your repository
2. Manually trigger via Actions tab
3. Make a manual change to one of your test repos
4. Run again to see drift detection in action

### Verification

✅ Workflow runs on schedule and manual trigger  
✅ Creates issue when drift is detected  
✅ Issue contains plan output showing what changed

---

## Lab 4: Build Self-Service Repository Request

**Objective:** Create a workflow that allows developers to request new repositories via pull request.

### Setup

Create a new directory structure:

```bash
mkdir -p lab4-selfservice/requests
cd lab4-selfservice

```

### Steps

**Step 1:** Create `request-template.yaml`:

```yaml
# Repository Request Template
# Copy this file to requests/YOUR_REPO_NAME.yaml and fill in details

name: ""              # Required: repository name (lowercase, hyphens)
description: ""       # Required: brief description
team: ""              # Required: team slug that will own this repo
visibility: "private" # Optional: private (default), internal, or public
template: ""          # Optional: template repository name
topics: []            # Optional: list of topics

```

**Step 2:** Create `main.tf`:

```hcl
terraform {
  required_providers {
    github = {
      source  = "integrations/github"
      version = "~> 6.0"
    }
  }
}

provider "github" {
  owner = "YOUR_ORG"
}

locals {
  # Read all YAML request files
  request_files = fileset(path.module, "requests/*.yaml")
  
  requests = {
    for file in local.request_files :
    trimsuffix(basename(file), ".yaml") => yamldecode(file("${path.module}/${file}"))
  }
}

module "requested_repos" {
  source   = "./modules/secure-repo"
  for_each = local.requests
  
  name        = each.value.name
  description = each.value.description
  visibility  = lookup(each.value, "visibility", "private")
}

```

**Step 3:** Create `.github/workflows/repo-request.yml`:

```yaml
name: Repository Request

on:
  pull_request:
    paths:
      - 'requests/*.yaml'

permissions:
  contents: read
  pull-requests: write

jobs:
  validate:
    name: Validate Request
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        
      - name: Validate YAML
        run: |
          for file in requests/*.yaml; do
            echo "Validating $file"
            # Check required fields
            if ! grep -q "^name:" "$file"; then
              echo "ERROR: Missing 'name' in $file"
              exit 1
            fi
            if ! grep -q "^description:" "$file"; then
              echo "ERROR: Missing 'description' in $file"
              exit 1
            fi
            if ! grep -q "^team:" "$file"; then
              echo "ERROR: Missing 'team' in $file"
              exit 1
            fi
          done
          echo "✅ All request files valid"
          
      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        
      - name: Terraform Plan
        id: plan
        run: |
          terraform init
          terraform plan -no-color
        env:
          GITHUB_TOKEN: ${{ secrets.GH_ADMIN_TOKEN }}
          
      - name: Comment Plan
        uses: actions/github-script@v7
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: `### Repository Request Validation ✅
              
              The following repositories will be created when this PR is merged:
              
              \`\`\`
              ${{ steps.plan.outputs.stdout }}
              \`\`\`
              
              **Reviewers:** Please verify the request follows naming conventions and the team assignment is correct.`
            })

```

### Verification

✅ Developers can request repos by creating YAML file  
✅ PR workflow validates request format  
✅ Terraform plan shows what will be created  
✅ Merge triggers repository creation

---

## Lab Summary

You've practiced:

| Lab | Skills Gained |
|-----|---------------|
| Lab 1 | Importing existing resources, state management |
| Lab 2 | Module development, code reuse, consistency |
| Lab 3 | GitOps drift detection, alerting |
| Lab 4 | Self-service workflows, PR-based provisioning |

---

**Next:** Test your knowledge in the Quiz section.
