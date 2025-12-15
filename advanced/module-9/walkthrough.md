---
layout: training-module
title: "Guided Walkthrough"
permalink: /advanced/module-9/walkthrough/
module_number: 9
module_title: "Enterprise Administration"
section_number: 3
total_sections: 7
phase: advanced
estimated_time: "35 min"
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
toc: true
prev_section:
  url: /advanced/module-9/concepts/
  title: "Core Concepts"
next_section:
  url: /advanced/module-9/labs/
  title: "Hands-On Labs"
---

# Guided Walkthrough

## Walkthrough 3.1: Configuring SAML SSO with Azure AD

This walkthrough demonstrates configuring SAML SSO using Microsoft Entra ID (Azure AD):

### Step 1: Create Enterprise Application in Azure AD

1. Navigate to **Azure Portal** → **Microsoft Entra ID** → **Enterprise applications**
2. Click **New application** → **Create your own application**
3. Name: `GitHub Enterprise Cloud - [Your Enterprise]`
4. Select **Integrate any other application you don't find in the gallery**

### Step 2: Configure SAML Settings in Azure AD

```
# Azure AD SAML Configuration

Basic SAML Configuration:
  Identifier (Entity ID): https://github.com/enterprises/YOUR_ENTERPRISE
  Reply URL (ACS URL): https://github.com/enterprises/YOUR_ENTERPRISE/saml/consume
  Sign-on URL: https://github.com/enterprises/YOUR_ENTERPRISE/sso

Attributes & Claims:
  Required Claim:
    - Unique User Identifier (Name ID)
      Source: Attribute
      Value: user.userprincipalname
  
  Additional Claims:
    - emails
      Source: user.mail
    - name  
      Source: user.displayname

```

### Step 3: Download Federation Metadata

1. In the SAML Signing Certificate section, download **Federation Metadata XML**
2. Or note individual values:
   - Login URL
   - Azure AD Identifier
   - Certificate (Base64)

### Step 4: Configure GitHub Enterprise

1. Navigate to **Enterprise settings** → **Settings** → **Authentication security**
2. Under SAML single sign-on, click **Configure SAML**
3. Enter Azure AD values:

```yaml
SAML Configuration:
  Sign-on URL: https://login.microsoftonline.com/{tenant-id}/saml2
  Issuer: https://sts.windows.net/{tenant-id}/
  Public Certificate: [Paste certificate content]

```

4. Click **Test SAML configuration** with your account
5. Once verified, enable **Require SAML authentication**

---

## Walkthrough 3.2: Setting Up SCIM Provisioning

### Step 1: Enable SCIM in GitHub

1. In Enterprise settings, navigate to **Authentication security**
2. Under SCIM, click **Enable SCIM user provisioning**
3. Generate a new **SCIM token** (save this securely!)

### Step 2: Configure SCIM in Azure AD

1. In your GitHub Enterprise application, go to **Provisioning**
2. Set Provisioning Mode to **Automatic**
3. Enter credentials:

```yaml
Admin Credentials:
  Tenant URL: https://api.github.com/scim/v2/enterprises/YOUR_ENTERPRISE
  Secret Token: [Your SCIM Token]

```

4. Click **Test Connection** to verify

### Step 3: Configure Attribute Mappings

```yaml
Required Attribute Mappings:
  userName: userPrincipalName
  active: Switch([IsSoftDeleted], , "False", "True", "True", "False")
  displayName: displayName
  emails[type eq "work"].value: mail
  name.givenName: givenName
  name.familyName: surname
  externalId: objectId

Optional (for team sync):
  roles[primary eq "True"].value: [Custom mapping for team membership]

```

### Step 4: Start Provisioning

1. Enable provisioning
2. Start with **Provision on demand** to test specific users
3. Once validated, enable **Full sync**
4. Monitor provisioning logs for errors

---

## Walkthrough 3.3: Enterprise Migration with GitHub Enterprise Importer

Migrate repositories from other platforms to GitHub Enterprise Cloud:

### Step 1: Install GitHub CLI Extension

```bash
# Install the GitHub Enterprise Importer extension
gh extension install github/gh-gei

# Authenticate with GitHub
gh auth login

# Verify installation
gh gei --help

```

### Step 2: Generate Migration Script (from Azure DevOps)

```bash
# Generate migration script for Azure DevOps organization
gh gei generate-script \
  --ado-org "source-ado-org" \
  --github-org "target-github-org" \
  --output migrate-repos.sh

# Review generated script
cat migrate-repos.sh

```

### Step 3: Execute Migration

```bash
# Set required tokens
export ADO_PAT="your-azure-devops-pat"
export GH_PAT="your-github-pat"

# Run migration for a single repository (dry run first)
gh gei migrate-repo \
  --ado-org "source-ado-org" \
  --ado-team-project "MyProject" \
  --ado-repo "my-repository" \
  --github-org "target-github-org" \
  --github-repo "my-repository"

# Monitor migration status
gh gei wait-for-migration --migration-id <migration-id>

```

### Step 4: Validate Migration

```bash
# Check migration log
gh gei download-logs --migration-id <migration-id>

# Verify repository content
gh repo view target-github-org/my-repository

# Validate branch protection rules (manual recreation needed)
gh api repos/target-github-org/my-repository/branches/main/protection

```

---

## Key Takeaways

1. **SAML SSO** provides enterprise authentication through your identity provider
2. **SCIM** automates the user provisioning lifecycle
3. **GitHub Enterprise Importer** simplifies migrations from other platforms
4. Always **test configurations** with a single user before full rollout
5. **Document recovery procedures** before enabling SSO requirements
