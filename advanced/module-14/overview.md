---
layout: training-module
title: "Context & Overview"
permalink: /advanced/module-14/overview/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 1
total_sections: 12
phase: advanced
estimated_time: "20 min"
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
next_section:
  url: /advanced/module-14/agents/
  title: "Copilot Agents"
---

<div class="callout callout-info" markdown="1">
<div class="callout-title">🚀 Official Documentation</div>
For comprehensive reference, see <a href="https://docs.github.com/en/copilot/using-github-copilot/using-extensions-to-integrate-external-tools-with-copilot-chat">Copilot Extensions</a>, <a href="https://docs.github.com/en/copilot/managing-copilot/managing-copilot-for-your-enterprise">Enterprise Administration</a>, and the <a href="https://docs.github.com/en/copilot/about-github-copilot/github-copilot-features">Copilot Features Overview</a>.
</div>

## Beyond Code Completion

Module 13 introduced GitHub Copilot fundamentals—inline completions, chat, and prompt engineering. This module explores Copilot's advanced capabilities that transform it from a code completion tool into an intelligent development platform.

### The Evolution of AI Assistance

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Evolution["GitHub Copilot Evolution"]
    Y2021["2021: Code Completion<br/>Inline suggestions, single-line focus"]
    Y2023["2023: Copilot Chat<br/>Conversational AI, explanations, debugging"]
    Y2024a["2024: Copilot Agents<br/>Autonomous task execution, multi-step reasoning"]
    Y2024b["2024: Extensions & MCP<br/>Custom integrations, external tools, enterprise data"]
    Y2025["2025+: Agentic Development<br/>End-to-end task completion, PR creation, issue fixing"]
    
    Y2021 --> Y2023 --> Y2024a --> Y2024b --> Y2025
  end
</div>
</div>

## What's New in Copilot Advanced

### Copilot Agents

Agents move beyond simple question-answering to autonomous task execution:

| Capability | Chat (Module 13) | Agents (Module 14) |
|-----------|-----------------|-------------------|
| **Scope** | Answer questions | Complete tasks |
| **Steps** | Single response | Multi-step reasoning |
| **Actions** | Suggestions only | Execute changes |
| **Context** | Current file/selection | Entire workspace |
| **Output** | Text/code snippets | PRs, commits, deployments |

**Example transformation:**

- **Chat**: "How do I add authentication to this API?"
- **Agent**: "Add OAuth authentication to the API, create the middleware, update routes, add tests, and open a PR"

### Model Context Protocol (MCP)

MCP enables Copilot to connect with external systems:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph MCP["Model Context Protocol"]
    CP["💬 Copilot<br/>Chat"] --> Server["🔌 MCP<br/>Server"]
    Server --> External["🌐 External<br/>Systems"]
    
    Server --> Resources
    subgraph Resources["Available Resources"]
      DB["💾 Databases"]
      API["🔗 APIs"]
      DOC["📄 Documents"]
      TOOL["🛠️ Tools"]
    end
  end
</div>
</div>

**Use cases:**

- Query internal documentation
- Access company-specific APIs
- Pull data from databases
- Integrate with internal tools

### Copilot Extensions

Build custom Copilot experiences for your organization:

| Extension Type | Purpose | Example |
|---------------|---------|---------|
| **Chat participant** | Domain-specific conversations | `@company-docs` for internal docs |
| **Slash commands** | Quick actions | `/deploy staging` |
| **Skill providers** | Custom capabilities | Integration with internal tools |

### Custom Instructions

Configure Copilot's behavior at multiple levels:

```
Enterprise Level
└── Organization Level
    └── Repository Level
        └── User Level

```

**Applications:**

- Enforce coding standards
- Apply security guidelines
- Use internal frameworks
- Follow team conventions

## Customer Value Proposition

### For Developers

| Challenge | Copilot Advanced Solution |
|-----------|--------------------------|
| Context switching | MCP brings tools into the IDE |
| Repetitive tasks | Agents automate multi-step work |
| Company knowledge | Extensions surface internal docs |
| Coding standards | Instructions enforce conventions |

### For Engineering Leaders

| Goal | How Copilot Advanced Helps |
|------|---------------------------|
| Consistency | Custom instructions enforce standards |
| Productivity | Agents handle complex tasks end-to-end |
| Onboarding | Extensions provide institutional knowledge |
| Governance | Enterprise admin controls usage |

### For Enterprise

| Requirement | Copilot Enterprise Features |
|-------------|----------------------------|
| Security | Content exclusions, audit logs |
| Compliance | Usage policies, data protection |
| Customization | Private knowledge bases |
| Measurement | Adoption metrics, ROI tracking |

---

## GitHub Spark (Preview)

GitHub Spark is a separate but related AI product that lets you build full-stack applications using natural language—no coding required.

### What is GitHub Spark?

Spark is an AI-powered app builder that:

- Generates complete applications from natural language descriptions
- Handles frontend, backend, and database automatically
- Deploys to GitHub's infrastructure
- Integrates with GitHub for version control

### Spark vs. Copilot

| Aspect | GitHub Copilot | GitHub Spark |
|--------|----------------|--------------|
| **Purpose** | Assist developers | Build apps without coding |
| **Output** | Code suggestions | Complete applications |
| **Users** | Developers | Anyone (no-code) |
| **Integration** | IDEs, GitHub.com | Standalone web platform |

### Example Spark Use Cases

- Prototyping ideas quickly
- Building internal tools
- Creating demos for customer conversations
- Generating proof-of-concept applications

> **📚 Learn More:** [GitHub Spark](https://docs.github.com/en/copilot/tutorials/spark/build-apps-with-spark)

---

## GitHub Models

GitHub Models (available at [github.com/marketplace/models](https://github.com/marketplace/models)) is GitHub's AI model playground and unified inference API. Think of it as a test kitchen for AI: you can experiment with dozens of foundation models from OpenAI, Anthropic, Meta (Llama), Mistral, Microsoft (Phi), and others — directly from GitHub, for free, with no infrastructure setup.

### What GitHub Models Provides

| Capability | Description |
|-----------|-------------|
| **Model Playground** | Interactive chat UI to test any supported model with your own prompts |
| **Unified Inference API** | One consistent API (OpenAI SDK-compatible) for all models |
| **Free experimentation** | Rate-limited free tier — no Azure subscription required |
| **Production path** | Production usage routes through Azure AI Foundry |

### Available Models (representative list)

Models available through GitHub Models include (subject to change):

- **OpenAI** — GPT-4o, GPT-4o mini, o1, o3-mini
- **Anthropic** — Claude Sonnet, Claude Haiku, Claude Opus
- **Meta** — Llama 4, Llama 3.x
- **Mistral** — Mistral Large, Mistral Nemo
- **Microsoft** — Phi-4, Phi-3.5

> **💡 Tip:** The full list changes as new models are added. Check [github.com/marketplace/models](https://github.com/marketplace/models) for the current catalog.

### GitHub Models vs. Copilot

Copilot uses these same models under the hood, but GitHub Models gives developers **direct access** for building their own AI-powered applications.

| Aspect | GitHub Copilot | GitHub Models |
|--------|----------------|---------------|
| **Who uses it** | Developers wanting coding help | Developers *building* AI apps |
| **Interface** | IDE, github.com, CLI | Playground UI + API |
| **Output** | Code suggestions, chat responses | Direct model completions via API |
| **Use case** | Developer productivity | AI app development, prompt engineering |
| **Copilot Extensions** | Built on top of Copilot | Can use the Models API as their backend |

### Why It Matters for CSMs

GitHub Models is a key piece of GitHub's **end-to-end AI developer platform** story:

- **Copilot** makes developers more productive today
- **GitHub Models** empowers developers to *build* AI-powered products themselves
- **Copilot Extensions** can use the Models API as their inference backend

When a customer asks "Is GitHub just a code host with Copilot bolted on?", GitHub Models is part of your answer. GitHub is building the platform where AI applications are developed, tested, and shipped — not just written.

### Example CSM Talking Point

> "Your developers can go from idea to production AI feature using GitHub the whole way: experiment with any foundation model in the GitHub playground, prototype with the free API, build their app, review it in a pull request, and ship it through Actions — all without leaving GitHub."

### Use Cases for Your Customers

- **Prompt engineering** — Test and compare how different models respond to the same prompt before committing to one
- **Prototyping** — Build proof-of-concept AI features before investing in production infrastructure
- **Copilot Extensions** — Extensions can call the Models API to power custom AI chat experiences
- **Model comparison** — Evaluate cost/quality trade-offs across models for a specific task

> **📚 Learn More:** [GitHub Models Documentation](https://docs.github.com/en/github-models)

---

## Module Learning Path

This module covers:

| Section | Topic | Focus |
|---------|-------|-------|
| Agents | Autonomous task execution | Using and understanding Copilot Agents |
| MCP | Model Context Protocol | Connecting external systems |
| Extensions | Custom Copilot capabilities | Building chat participants |
| Instructions | Behavior customization | Organization-wide configuration |
| Enterprise | Administration at scale | Policies, security, governance |
| Metrics | Measuring success | Adoption, usage, ROI |
| Labs | Hands-on practice | Build your own extension |
| Quiz | Knowledge check | Validate understanding |

## Prerequisites Check

Before proceeding, ensure you're comfortable with:

✅ Copilot Chat slash commands (`/explain`, `/fix`, `/tests`)  
✅ Chat participants (`@workspace`, `@terminal`)  
✅ Context management (file references, selection)  
✅ Prompt engineering basics  

If any of these are unfamiliar, review [Module 13: Copilot Fundamentals]({{ site.baseurl }}/advanced/module-13/) first.

---

**Ready to explore autonomous AI assistance?** Continue to learn about Copilot Agents.
