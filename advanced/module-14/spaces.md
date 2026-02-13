---
layout: training-module
title: "Copilot Spaces"
permalink: /advanced/module-14/spaces/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 6
total_sections: 12
phase: advanced
estimated_time: "25 min"
module_index: /advanced/module-14/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-14/overview/"
    short_title: "Overview"
    icon: "📋"
  - title: "Copilot Agents"
    url: "/advanced/module-14/agents/"
    short_title: "Agents"
    icon: "🤖"
  - title: "Model Context Protocol"
    url: "/advanced/module-14/mcp/"
    short_title: "MCP"
    icon: "🔌"
  - title: "Copilot Extensions"
    url: "/advanced/module-14/extensions/"
    short_title: "Extensions"
    icon: "🧩"
  - title: "Custom Instructions"
    url: "/advanced/module-14/instructions/"
    short_title: "Instructions"
    icon: "📝"
  - title: "Copilot Spaces"
    url: "/advanced/module-14/spaces/"
    short_title: "Spaces"
    icon: "🗂️"
  - title: "Enterprise Administration"
    url: "/advanced/module-14/enterprise/"
    short_title: "Enterprise"
    icon: "🏢"
  - title: "Metrics & Adoption"
    url: "/advanced/module-14/metrics/"
    short_title: "Metrics"
    icon: "📊"
  - title: "Hands-On Labs"
    url: "/advanced/module-14/labs/"
    short_title: "Labs"
    icon: "🧪"
  - title: "Knowledge Check"
    url: "/advanced/module-14/quiz/"
    short_title: "Quiz"
    icon: "📝"
  - title: "Resources"
    url: "/advanced/module-14/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/advanced/module-14/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
prev_section:
  url: /advanced/module-14/instructions/
  title: "Custom Instructions"
next_section:
  url: /advanced/module-14/enterprise/
  title: "Enterprise Administration"
---

# Copilot Spaces

## What Are Copilot Spaces?

**Copilot Spaces** let you create curated collections of context — files, repositories, instructions, and notes — that ground Copilot's responses for specific tasks or projects.

> 💡 **Think of it like a project briefcase:** When you start a new engagement with a customer, you'd gather all the relevant documents, notes, and reference materials into a folder. Copilot Spaces does the same thing for AI conversations — instead of re-explaining context every time, you create a Space with everything Copilot needs.

---

## Why Spaces Matter

### The Context Problem

Without Spaces, every Copilot conversation starts from scratch. You have to:

- Manually attach files each session
- Re-explain project requirements
- Hope Copilot infers the right context from your current file
- Repeat yourself across multiple conversations

### How Spaces Solve It

Spaces provide **persistent, shareable, structured context** for Copilot:

| Without Spaces | With Spaces |
|----------------|-------------|
| Context resets every conversation | Context persists across sessions |
| Limited to current file/repo | Spans multiple repos and sources |
| Individual only | Shareable with team |
| Manual file attachment | Pre-loaded context |
| Generic responses | Project-specific answers |

---

## Spaces vs. Other Context Methods

Understanding when to use Spaces versus other Copilot context mechanisms:

| Method | Scope | Persistence | Sharing | Best For |
|--------|-------|-------------|---------|----------|
| **Spaces** | Task/project | Permanent | Team/org | Cross-repo projects, customer engagements |
| **@workspace** | Current repo | Session | No | Single repo exploration |
| **Custom Instructions** | Repo/org | Permanent | Via repo config | Coding standards, style guides |
| **Knowledge Bases** | Organization | Permanent | Org-wide | Documentation, internal wikis |
| **File attachment** | Conversation | Session only | No | Quick one-off questions |

### When to Use Spaces

- Working across **multiple repositories** on a related project
- Customer engagements that span **weeks or months**
- **Team collaboration** where multiple people need the same context
- Projects involving **external documentation** (specs, architecture docs, API references)
- **Migration projects** with source and target platform context

---

## Creating and Managing Spaces

### Creating a Space

1. Navigate to [github.com/copilot/spaces](https://github.com/copilot/spaces)
2. Click **Create space**
3. Name your space (e.g., "Q4 Platform Migration" or "Acme Corp Onboarding")
4. Choose ownership: **Personal** or **Organization**
5. Add a description that helps teammates understand the space's purpose

### Adding Context to a Space

Spaces support multiple content types:

#### Instructions

Guide Copilot's behavior within the space:

```markdown
You are helping plan a migration from Azure DevOps to GitHub Enterprise Cloud.
The customer has 200 repositories and uses Azure Pipelines extensively.
Focus on:
- Migration planning and phasing
- ADO Pipelines → GitHub Actions conversion
- Identity mapping from Azure AD to GitHub EMU
```

#### Source Content

| Source Type | How to Add | Example |
|-------------|-----------|---------|
| **Repositories** | Search and add entire repos or specific folders | Customer's main application repo |
| **Files** | Add specific files from any repo you have access to | Architecture decision records, config files |
| **Links** | Paste GitHub URLs (Issues, PRs, Discussions) | Related issues, design PRs |
| **Uploads** | Upload local files (docs, images, spreadsheets) | Customer's architecture diagram, meeting notes |
| **Text Content** | Write or paste freeform text | Call transcripts, requirements notes |

#### Quick Add from Code View

While browsing code on GitHub.com:

1. Open any file in a repository
2. Click the **Add to space** icon at the top of the file
3. Select an existing space or create a new one

This lets you build context as you investigate code without interrupting your workflow.

---

## Space Organization Best Practices

### Structuring Your Spaces

| Do | Don't |
|----|-------|
| Create focused spaces per task/project | Create one giant "everything" space |
| Include clear instructions | Leave instructions empty |
| Update sources as the project evolves | Let spaces become stale |
| Add context breadcrumbs (explain *why* a file is included) | Add files without explanation |
| Archive completed project spaces | Delete spaces (preserve history for reference) |
| Name spaces descriptively | Use vague names like "Stuff" or "Test" |

### Space Naming Conventions

Good naming helps your team find and understand spaces at a glance:

```
✅ Good names:
  "Acme Corp - GHES to GHEC Migration"
  "Q1 2026 Security Audit - FinCo"
  "Platform Team - Actions Runner Scaling"
  "Auth Service Refactor - Phase 2"

❌ Poor names:
  "Migration"
  "Customer stuff"
  "Work"
  "Test space 3"
```

---

## Sharing and Collaboration

### Sharing Organization Spaces

Organization-owned Spaces can be shared with team members:

1. Open the Space
2. Go to **Settings** → **Sharing**
3. Add individual team members or entire GitHub teams
4. Choose permission level:
   - **View** — Can use the space for conversations but not modify it
   - **Edit** — Can add/remove sources and update instructions

### Collaboration Scenarios

| Scenario | Space Setup | Sharing |
|----------|-------------|---------|
| **CSM pair working a customer** | One space per customer engagement | Share with co-assigned CSM |
| **Team migration project** | Space with migration tools, customer repos, runbook | Share with entire CS team |
| **Knowledge sharing** | Space with best practices, common architectures | Share org-wide |
| **Escalation handoff** | Space with customer context, issue history | Share with support engineer |

---

## Practical Use Cases for CSMs

### Customer Onboarding Space

Create a space with everything you need for a new customer relationship:

- Customer's main repositories (architecture overview)
- Their organization settings and configuration
- Relevant GitHub documentation for their use case
- Notes from discovery calls and kickoff meetings
- Industry-specific compliance requirements

**Example instructions:**
```markdown
This space is for the Acme Corp onboarding engagement.
Acme is a financial services company with 500 developers.
They're migrating from GitLab to GHEC with EMU.
Key concerns: SOC 2 compliance, audit logging, SAML with Okta.
When answering questions, consider regulatory requirements.
```

### Migration Project Space

Organize all migration-related context in one place:

- Source platform documentation and export data
- Target platform (GHEC) configuration guides
- Migration runbook and timeline
- Customer's repository inventory
- CI/CD pipeline conversion notes
- Identity mapping spreadsheet

### Support Escalation Space

When escalating a customer issue:

- Customer's repository with the issue
- Related GitHub Issues and Pull Requests
- Error logs and diagnostic data
- Support playbooks and known resolutions
- Previous escalation history

### Enterprise Architecture Review Space

Prepare for architecture review meetings:

- Customer's current GitHub organization structure
- Security audit findings
- GHAS configuration and dashboards
- Actions workflow patterns
- Governance policy documents

---

## Spaces and Enterprise Copilot

### Organization-Level Considerations

| Feature | Personal Spaces | Organization Spaces |
|---------|----------------|---------------------|
| **Ownership** | Individual user | Organization |
| **Visibility** | Creator only (unless shared) | Configurable by admin |
| **Content Access** | User's accessible repos | Org-accessible repos |
| **Admin Control** | User manages | Org admins can manage |
| **Persistence** | Tied to user account | Tied to organization |

### Admin Controls

Enterprise administrators can:

- **Enable/disable Spaces** at the organization or enterprise level
- **Set content policies** for what can be added to shared spaces
- **Monitor usage** through Copilot metrics
- **Configure access** based on team membership

---

## CSM Talking Points

**For customers evaluating Copilot Spaces:**

> "Copilot Spaces is like creating a custom expert for each project. Instead of Copilot starting from scratch every conversation, you give it the full context of your project — the relevant code, docs, and requirements — so every answer is grounded in your specific situation."

**For enterprise customers:**

> "Organization-owned Spaces let your teams share context so everyone working on a project has the same grounding. When someone new joins a project or picks up an escalation, they can open the Space and Copilot immediately understands the context."

**For customers comparing to Knowledge Bases:**

> "Think of Knowledge Bases as your organization's reference library — stable documentation that applies broadly. Spaces are more like project-specific working folders — they evolve with the project and can include context from multiple repos, uploaded documents, and custom instructions."

### Discovery Questions

- "How often do your developers work across multiple repositories for a single feature?"
- "When you onboard someone new to a project, how long does it take them to understand the codebase?"
- "Do your teams have a way to share context and institutional knowledge about ongoing projects?"
- "How do you handle knowledge transfer during escalations or team transitions?"

---

## Hands-On Exercise

### Create Your First Space (5 minutes)

1. Go to [github.com/copilot/spaces](https://github.com/copilot/spaces)
2. Click **Create space**
3. Name it "Training - Module 14 Practice"
4. Add instructions: "You are helping a CSM learn about GitHub Copilot advanced features."
5. Add a repository you've been working with during this training
6. Start a conversation in the Space and ask a question about the repo
7. Notice how Copilot's responses are grounded in the Space's context

### Build a Customer Engagement Space (10 minutes)

1. Create a new Space named "Practice - Customer Migration"
2. Add instructions describing a fictional customer migration scenario
3. Add relevant documentation links
4. Add text notes simulating discovery call findings
5. Use the Space to ask Copilot for a migration planning outline
6. Compare the response quality to asking the same question without a Space

---

## Key Takeaways

1. **Spaces = Persistent Context** — Create once, use across many conversations
2. **Cross-Repo Scope** — Combine context from multiple repositories, docs, and notes
3. **Shareable** — Organization-owned Spaces enable team collaboration
4. **Project-Focused** — Create focused spaces per task, not one giant catch-all
5. **CSM Superpower** — Use Spaces to maintain customer engagement context and accelerate your work

---

## Additional Resources

- 📖 [About Copilot Spaces](https://docs.github.com/en/copilot/concepts/about-organizing-and-sharing-context-with-copilot-spaces)
- 📖 [Using Copilot Spaces](https://docs.github.com/en/copilot/using-github-copilot/using-copilot-spaces)
- 📖 [Managing Copilot Spaces for your organization](https://docs.github.com/en/copilot/managing-copilot/managing-copilot-for-your-organization)
- 📖 [Copilot Features Overview](https://docs.github.com/en/copilot/about-github-copilot/github-copilot-features)
