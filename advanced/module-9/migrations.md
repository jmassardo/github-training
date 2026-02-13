---
layout: training-module
title: "Migrating to GHEC"
permalink: /advanced/module-9/migrations/
module_number: 9
module_title: "Enterprise Administration"
section_number: 10
total_sections: 10
phase: advanced
estimated_time: "30 min"
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
  url: /advanced/module-9/ghes-admin/
  title: "GHES Administration"
next_section:
  url: /advanced/module-10/
  title: "Module 10: GitHub APIs & Apps"
---

# Migrating to GitHub Enterprise Cloud

## Why Migration Matters for CSMs

Platform migration is one of the highest-stakes projects your customers will undertake. As a CSM, you'll often guide customers through migrations from GHES, Azure DevOps, GitLab, Bitbucket, or other platforms to GitHub Enterprise Cloud. Understanding migration tools, planning strategies, and common pitfalls will help you set realistic expectations and ensure a smooth transition.

> 💡 **Think of it like moving offices:** You wouldn't just throw everything in a truck and hope for the best. You'd inventory what you have, decide what moves and what doesn't, plan the move in phases, and verify everything arrived safely. Platform migration works the same way.

---

## Migration Sources & Tools

### GitHub Enterprise Importer (GEI)

**GitHub Enterprise Importer** is the recommended tool for enterprise-scale migrations. It's designed for high-fidelity, automated migration of repositories and their metadata.

| Source Platform | Supported | Tool |
|----------------|-----------|------|
| **GitHub Enterprise Server** | ✅ Full support | GEI |
| **Azure DevOps** | ✅ Full support | GEI |
| **Bitbucket Server** | ✅ Full support | GEI |
| **GitLab** | ⚠️ Partial (repos + history) | `gei` CLI + manual steps |
| **Bitbucket Cloud** | ⚠️ Limited | GEI (repo-level) |
| **Other Git hosts** | ⚠️ Git history only | `git clone --mirror` + `git push` |
| **SVN, TFVC, Mercurial** | ⚠️ Requires conversion | Third-party tools + git import |

### What GEI Migrates

| Data Type | GHES → GHEC | ADO → GHEC | Bitbucket Server → GHEC |
|-----------|:-----------:|:----------:|:-----------------------:|
| **Git history** (commits, branches, tags) | ✅ | ✅ | ✅ |
| **Pull Requests / PRs** | ✅ | ✅ (from ADO PRs) | ✅ |
| **PR review comments** | ✅ | ✅ | ✅ |
| **Issues** | ✅ | ❌ (use separate tool) | ❌ |
| **Wikis** | ✅ | ❌ | ❌ |
| **Releases** | ✅ | N/A | N/A |
| **Branch protection rules** | ✅ | N/A | N/A |
| **GitHub Actions workflows** | ✅ (files only) | N/A | N/A |
| **CI/CD pipelines** | N/A | ❌ (manual conversion) | ❌ (manual conversion) |
| **Work items / Issues** | ✅ | ❌ (use GitHub Issues Importer) | ❌ |
| **Boards / Projects** | ❌ | ❌ | ❌ |

<div class="callout callout-warning" markdown="1">
<div class="callout-title">⚠️ What Doesn't Migrate Automatically</div>

- **CI/CD pipelines** — Azure Pipelines YAML, Jenkins pipelines, and GitLab CI configs must be manually converted to GitHub Actions workflows
- **Project boards** — Azure Boards, Jira boards, and GitLab boards need manual recreation in GitHub Projects
- **Secrets and variables** — CI/CD secrets must be manually re-created in the target repository/organization
- **Webhooks** — Must be reconfigured to point to new repository URLs
- **Third-party integrations** — Apps and integrations need reconfiguration
</div>

---

## Migration Planning

### The Migration Framework

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  A["📋 Phase 1: Discovery & Assessment<br/>(2-4 weeks)"] --> B["🏗️ Phase 2: Preparation<br/>(2-4 weeks)"]
  B --> C["🧪 Phase 3: Pilot Migration<br/>(1-2 weeks)"]
  C --> D["🚀 Phase 4: Production Migration<br/>(2-8 weeks)"]
  D --> E["✅ Phase 5: Validation & Cutover<br/>(1-2 weeks)"]
  E --> F["📊 Phase 6: Post-Migration<br/>(ongoing)"]
</div>
</div>

### Phase 1: Discovery & Assessment

**Goal:** Understand what you're migrating and identify blockers early.

| Activity | Details |
|----------|---------|
| **Inventory repositories** | Count, sizes, activity levels, languages |
| **Identify dependencies** | CI/CD pipelines, webhooks, integrations, packages |
| **Map user identities** | Source usernames → GitHub usernames (SAML/EMU mapping) |
| **Assess compliance needs** | Data residency, retention policies, audit requirements |
| **Identify large/complex repos** | Repos > 5 GB, repos with Git LFS, monorepos |
| **Document current workflows** | Branching strategies, review processes, deployment flows |

#### Discovery Questions to Ask Customers

- "How many repositories are in scope for migration?"
- "What's the size of your largest repository?"
- "Are you using Git LFS for any repositories?"
- "What CI/CD platform are you currently using?"
- "How are user identities managed today? (LDAP, SAML, local accounts)"
- "Are there any regulatory requirements around data retention during migration?"
- "Do you have any repositories that must remain on the current platform?"

### Phase 2: Preparation

**Goal:** Set up the target environment and prepare migration infrastructure.

| Activity | Details |
|----------|---------|
| **Set up GHEC organization structure** | Create orgs, teams, configure SSO/SCIM |
| **Configure identity mapping** | Map source users to GHEC identities |
| **Install and configure GEI** | Set up migration tooling, test connectivity |
| **Create migration runbook** | Step-by-step procedure with rollback plans |
| **Communicate with stakeholders** | Set expectations for timeline and any downtime |
| **Prepare CI/CD conversion** | Start converting pipelines to GitHub Actions |

### Phase 3: Pilot Migration

**Goal:** Validate the migration process with a small, representative set of repositories.

| Activity | Details |
|----------|---------|
| **Select pilot repos** | 5-10 repos representing different sizes and complexity |
| **Run trial migration** | Execute GEI migration and validate output |
| **Verify data integrity** | Compare commit counts, PR history, branch structure |
| **Test CI/CD** | Ensure converted workflows function correctly |
| **Gather feedback** | Have pilot teams use the migrated repos for 1-2 weeks |
| **Refine process** | Update runbook based on pilot learnings |

### Phase 4: Production Migration

**Goal:** Migrate remaining repositories in planned batches.

| Strategy | Description | Best For |
|----------|-------------|----------|
| **Big bang** | Migrate everything in one maintenance window | Small orgs (< 50 repos) |
| **Phased by team** | Migrate team-by-team over weeks | Medium orgs, team-level CI/CD |
| **Phased by criticality** | Low-risk repos first, critical repos last | Risk-averse enterprises |
| **Parallel operation** | Run both platforms during transition | Large enterprises needing zero downtime |

### Phase 5: Validation & Cutover

**Goal:** Verify everything migrated correctly and switch over.

| Activity | Details |
|----------|---------|
| **Data validation** | Automated scripts to compare source vs. target |
| **CI/CD validation** | Run all pipelines in the new environment |
| **User acceptance testing** | Development teams verify their workflows |
| **DNS/URL updates** | Update bookmarks, documentation, tooling references |
| **Decommission source** | Archive or disable source repositories |
| **Update integrations** | Point webhooks, apps, and third-party tools to new repos |

### Phase 6: Post-Migration

**Goal:** Stabilize and optimize the new environment.

| Activity | Details |
|----------|---------|
| **Monitor adoption** | Track active users, commit activity, PR velocity |
| **Address issues** | Quickly resolve any migration-related problems |
| **Optimize workflows** | Take advantage of GHEC-specific features |
| **Clean up** | Remove temporary migration infrastructure |
| **Retrospective** | Document lessons learned for future migrations |

---

## Using GitHub Enterprise Importer (GEI)

### Installing the GEI CLI

```bash
# Install via GitHub CLI extension
gh extension install github/gh-gei

# Verify installation
gh gei --version
```

### Basic Migration Commands

#### GHES to GHEC Migration

```bash
# Step 1: Generate a migration script for all repos in an org
gh gei generate-script \
  --github-source-org SOURCE_ORG \
  --github-target-org TARGET_ORG \
  --ghes-api-url https://ghes.yourcompany.com/api/v3 \
  --output migrate.sh

# Step 2: Review and customize the generated script
cat migrate.sh

# Step 3: Execute the migration
# Set environment variables for authentication
export GH_PAT=ghp_your_target_pat
export GH_SOURCE_PAT=ghp_your_source_pat

# Run migration for a single repository
gh gei migrate-repo \
  --github-source-org SOURCE_ORG \
  --source-repo my-repo \
  --github-target-org TARGET_ORG \
  --target-repo my-repo \
  --ghes-api-url https://ghes.yourcompany.com/api/v3
```

#### Azure DevOps to GHEC Migration

```bash
# Generate migration script for all repos in an ADO org
gh gei generate-script \
  --ado-source-org ADO_ORG \
  --github-target-org TARGET_ORG \
  --output migrate-ado.sh

# Migrate a single ADO repository
export ADO_PAT=your_ado_pat
export GH_PAT=ghp_your_github_pat

gh gei migrate-repo \
  --ado-source-org ADO_ORG \
  --ado-team-project MY_PROJECT \
  --source-repo my-repo \
  --github-target-org TARGET_ORG \
  --target-repo my-repo
```

### Migration Monitoring

```bash
# Check migration status
gh gei wait-for-migration --migration-id MIGRATION_ID

# List recent migrations
gh gei list-migrations --github-target-org TARGET_ORG
```

---

## CI/CD Pipeline Conversion

### Azure Pipelines → GitHub Actions

This is often the most time-consuming part of an ADO migration. Here's a high-level mapping:

| Azure Pipelines Concept | GitHub Actions Equivalent |
|------------------------|--------------------------|
| `azure-pipelines.yml` | `.github/workflows/*.yml` |
| Pipeline | Workflow |
| Stage | Job (with `needs` for ordering) |
| Job | Job |
| Step | Step |
| Task | Action |
| Variable Group | Environment secrets / Organization secrets |
| Service Connection | OIDC or repository secrets |
| Agent Pool | Runner group |
| Artifact | Artifact (via `actions/upload-artifact`) |
| Environment | Environment |

<div class="callout callout-tip" markdown="1">
<div class="callout-title">💡 CSM Tip</div>

GitHub provides the **GitHub Actions Importer** tool (`gh actions-importer`) that can automatically convert Azure Pipelines, Jenkins, GitLab CI, Travis CI, and CircleCI pipelines to GitHub Actions workflows. It won't produce perfect results for every pipeline, but it handles 60-80% of the conversion work and gives teams a solid starting point.

```bash
# Install GitHub Actions Importer
gh extension install github/gh-actions-importer

# Audit existing pipelines (generates a report)
gh actions-importer audit azure-devops \
  --output-dir audit-results

# Convert a specific pipeline
gh actions-importer convert azure-devops \
  --output-dir converted-workflows \
  --pipeline-id PIPELINE_ID
```
</div>

---

## Identity Mapping

### The Challenge

When migrating repositories, you need to map user identities from the source platform to GitHub accounts. This is critical for preserving:

- **PR authorship** — Who created and reviewed pull requests
- **Commit attribution** — Git commit author mapping
- **Issue assignments** — Who was assigned to issues
- **@mentions** — References to users in comments

### Identity Mapping Strategies

| Strategy | Description | Best For |
|----------|-------------|----------|
| **Mannequin reclaim** | GEI creates placeholder "mannequin" accounts; admins reclaim them post-migration | Most migrations |
| **Pre-mapped identities** | Provide a CSV mapping source users to GitHub users before migration | EMU environments |
| **Post-migration cleanup** | Migrate first, then manually resolve unmapped users | Small migrations |

### Mannequin Reclamation

After a GEI migration, unmapped users appear as "mannequins" (placeholder accounts). Organization owners can reclaim them:

1. Navigate to **Organization Settings** → **Import/Export** → **Mannequins**
2. Search for mannequin accounts
3. Map each mannequin to the correct GitHub user
4. The user receives an invitation to accept the attribution

```bash
# Generate mannequin reclamation CSV
gh gei mannequin-csv \
  --github-target-org TARGET_ORG \
  --output mannequins.csv

# Reclaim mannequins using a mapping file
gh gei reclaim-mannequin \
  --github-target-org TARGET_ORG \
  --csv mannequin-mapping.csv
```

---

## Common Migration Challenges

### Large Repositories

| Challenge | Threshold | Mitigation |
|-----------|-----------|------------|
| **Repository size** | > 5 GB | Use GEI's streaming mode; consider splitting the repo |
| **Git LFS objects** | Large LFS storage | Migrate LFS objects separately; verify LFS quota on GHEC |
| **Long history** | > 100,000 commits | GEI handles this, but allocate extra migration time |
| **Many branches** | > 500 branches | Clean up stale branches before migration |

### Organizational Resistance

| Concern | Response |
|---------|----------|
| "We'll lose our commit history" | "GEI preserves full Git history including all commits, branches, and tags" |
| "Our CI/CD pipelines will break" | "We have a phased approach: migrate code first, convert pipelines with GitHub Actions Importer, validate in parallel" |
| "We can't afford downtime" | "We can run parallel operations during transition — both platforms active simultaneously" |
| "User onboarding will be disruptive" | "With SAML SSO, users authenticate with their existing credentials. The identity mapping preserves their contribution history" |
| "What if something goes wrong?" | "Every migration starts with a pilot phase. We validate data integrity before proceeding, and the source platform remains available as a rollback" |

### Compliance & Data Concerns

| Concern | Solution |
|---------|----------|
| **Data retention requirements** | Archive source platform data before decommissioning; GHEC audit logs provide compliance trail |
| **Regulatory approvals** | Engage compliance team early; document data flow and storage locations |
| **Sensitive data in repos** | Scan for secrets/PII before migration using secret scanning; clean up before moving |
| **Cross-border data transfer** | Consider GHEC Data Residency (EU) for applicable regulations |

---

## Migration Timeline Estimates

| Migration Scope | Typical Timeline | Key Variables |
|----------------|-----------------|---------------|
| **Small** (< 50 repos, single source) | 4-6 weeks | Pilot + 1-2 migration batches |
| **Medium** (50-500 repos, single source) | 8-12 weeks | Phased team migration |
| **Large** (500+ repos, single source) | 12-20 weeks | Multiple migration waves |
| **Complex** (multiple sources, CI/CD conversion) | 16-24+ weeks | Parallel workstreams |

<div class="callout callout-info" markdown="1">
<div class="callout-title">📋 Timeline Factors</div>

The biggest variable in migration timelines isn't the data migration itself (GEI can migrate hundreds of repos in hours). It's the **surrounding work**: CI/CD conversion, integration reconfiguration, user training, and organizational change management. Plan accordingly.
</div>

---

## Post-Migration Checklist

```markdown
## Post-Migration Validation Checklist

### Data Integrity
- [ ] Commit counts match between source and target
- [ ] All branches and tags are present
- [ ] Pull request history and comments are preserved
- [ ] Issues and labels migrated correctly (GHES source)
- [ ] Wiki content migrated (GHES source)
- [ ] Releases and release assets present (GHES source)

### Configuration
- [ ] Branch protection rules configured
- [ ] CODEOWNERS files in place
- [ ] Repository visibility set correctly (public/private/internal)
- [ ] Team access permissions configured
- [ ] Repository settings (merge strategies, features) configured

### CI/CD
- [ ] GitHub Actions workflows validated and passing
- [ ] Secrets and variables configured in repositories/organizations
- [ ] Self-hosted runners registered (if applicable)
- [ ] Deployment environments configured with protection rules
- [ ] OIDC configured for cloud provider authentication

### Integrations
- [ ] Webhooks reconfigured
- [ ] Third-party apps installed and authorized
- [ ] IDE plugins updated to point to new repositories
- [ ] Documentation updated with new repository URLs

### Identity
- [ ] All mannequin accounts reclaimed
- [ ] SAML SSO configured and tested
- [ ] SCIM provisioning active
- [ ] Team memberships match expectations

### Cleanup
- [ ] Source repositories archived or marked read-only
- [ ] Migration tooling and temporary credentials removed
- [ ] Stakeholders notified of migration completion
- [ ] Retrospective conducted and documented
```

---

## CSM Talking Points

**For prospective migration customers:**

> "Migration is a project we've helped hundreds of organizations through successfully. GitHub Enterprise Importer handles the heavy lifting of moving your repositories, pull requests, and history. The key to success is solid planning — especially around CI/CD conversion and identity mapping."

**For customers hesitant to start:**

> "The best approach is a pilot migration. We'll migrate 5-10 representative repositories, validate everything, and gather feedback from your teams. This gives you confidence in the process before committing to a full migration."

**For customers worried about timelines:**

> "The actual data migration happens fast — GEI can move hundreds of repos in a matter of hours. The longer timeline accounts for planning, CI/CD conversion, team training, and validation. We break it into phases so you see steady progress."

---

## Additional Resources

- 📖 [About GitHub Enterprise Importer](https://docs.github.com/en/migrations/using-github-enterprise-importer/understanding-github-enterprise-importer/about-github-enterprise-importer)
- 📖 [Migrating from GHES to GHEC](https://docs.github.com/en/migrations/using-github-enterprise-importer/migrating-from-github-enterprise-server-to-github-enterprise-cloud)
- 📖 [Migrating from Azure DevOps](https://docs.github.com/en/migrations/using-github-enterprise-importer/migrating-from-azure-devops-to-github-enterprise-cloud)
- 📖 [Migrating from Bitbucket Server](https://docs.github.com/en/migrations/using-github-enterprise-importer/migrating-from-bitbucket-server-to-github-enterprise-cloud)
- 📖 [GitHub Actions Importer](https://docs.github.com/en/actions/migrating-to-github-actions/using-github-actions-importer-to-automate-migrations)
- 📖 [Reclaiming mannequins](https://docs.github.com/en/migrations/using-github-enterprise-importer/completing-your-migration-with-github-enterprise-importer/reclaiming-mannequins-for-github-enterprise-importer)
- 📖 [Migration support](https://docs.github.com/en/migrations/overview/planning-your-migration-to-github)
