---
layout: training-module
title: "Context & Overview"
permalink: /advanced/module-13/overview/
module_number: 13
module_title: "GitHub Copilot Fundamentals"
section_number: 1
total_sections: 10
phase: advanced
estimated_time: "20 min"
module_index: /advanced/module-13/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-13/overview/"
    short_title: "Overview"
    icon: "📋"
  - title: "Core Concepts"
    url: "/advanced/module-13/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "IDE Setup"
    url: "/advanced/module-13/setup/"
    short_title: "Setup"
    icon: "⚙️"
  - title: "Prompt Crafting"
    url: "/advanced/module-13/prompts/"
    short_title: "Prompts"
    icon: "✍️"
  - title: "Copilot Chat"
    url: "/advanced/module-13/chat/"
    short_title: "Chat"
    icon: "💬"
  - title: "IDE Differences"
    url: "/advanced/module-13/ide-differences/"
    short_title: "IDE Differences"
    icon: "🔀"
  - title: "Best Practices"
    url: "/advanced/module-13/best-practices/"
    short_title: "Best Practices"
    icon: "✅"
  - title: "Hands-On Labs"
    url: "/advanced/module-13/labs/"
    short_title: "Labs"
    icon: "🧪"
  - title: "Knowledge Check"
    url: "/advanced/module-13/quiz/"
    short_title: "Quiz"
    icon: "📝"
  - title: "Resources"
    url: "/advanced/module-13/resources/"
    short_title: "Resources"
    icon: "📖"
toc: true
prev_section:
next_section:
  url: /advanced/module-13/concepts/
  title: "Core Concepts"
---

## The AI-Powered Development Revolution

Software development is undergoing a fundamental transformation. Just as IDEs revolutionized coding from punch cards to visual editors, AI-assisted development is changing how developers write, understand, and maintain code.

GitHub Copilot represents a new paradigm: **AI pair programming**. Rather than replacing developers, it augments their capabilities by providing intelligent suggestions, answering questions, and handling repetitive tasks.

---

## What is GitHub Copilot?

GitHub Copilot is an AI-powered coding assistant that provides:

| Capability | Description | Use Case |
|------------|-------------|----------|
| **Code Completions** | Real-time suggestions as you type | Writing new functions, completing patterns |
| **Copilot Chat** | Conversational AI in your IDE and on GitHub.com | Explaining code, debugging, refactoring |
| **Multi-file Context** | Understands your entire project | Consistent naming, following patterns |
| **Documentation** | Generates comments and docs | Inline comments, README content |
| **Model Selection** | Choose your preferred AI model (GPT-4o, Claude Opus 4.7, etc.) | Optimizing for different tasks |
| **Copilot CLI** | AI assistance directly in your terminal | Shell command explanation and generation |
| **Code Review** | Automated pull request reviews | Catching issues before human review |
| **Vision** | Attach images/screenshots for analysis | UI debugging, design implementation |

> **📚 Learn More:** [GitHub Copilot Documentation](https://docs.github.com/en/copilot)

---

## Understanding Large Language Models (LLMs)

GitHub Copilot is powered by **Large Language Models (LLMs)**, a type of AI that has been trained on vast amounts of text and code. Understanding the basics of how LLMs work will help you use Copilot more effectively.

### How LLMs Generate Code

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    A[Your Code + Context] --> B[Tokenization]
    B --> C[LLM Processing]
    C --> D[Token Prediction]
    D --> E[Code Suggestion]
    
    style A fill:#0969da,color:#fff
    style C fill:#8250df,color:#fff
    style E fill:#1a7f37,color:#fff
</div>
</div>

**Key Concepts:**

| Concept | Explanation | Why It Matters |
|---------|-------------|----------------|
| **Tokens** | Words or code broken into smaller pieces | LLMs process tokens, not characters |
| **Context Window** | The amount of text the model can "see" | More context = better suggestions |
| **Temperature** | Randomness in responses | Lower = more predictable, Higher = more creative |
| **Prompt** | The input you provide to guide the model | Better prompts = better output |

### What LLMs Are (and Aren't)

**LLMs ARE:**

- Pattern recognition systems trained on vast code repositories
- Probabilistic text generators that predict likely next tokens
- Tools that can understand and generate multiple programming languages
- Assistants that work best with clear context and guidance

**LLMs ARE NOT:**

- Databases that store and retrieve exact code snippets
- Deterministic systems that always give the same answer
- Compilers or interpreters that understand code execution
- Replacements for understanding what your code does

> **💡 CSM Insight**
>
> When customers ask "Is Copilot just copying code?", explain that LLMs generate new code based on learned patterns, similar to how a skilled developer draws on experience but writes original code.
{: .callout .callout-info}

---

## The Copilot Product Family

GitHub offers Copilot in several tiers to meet different needs:

| Product | Audience | Key Features | AI Credits/Month |
|---------|----------|--------------|-----------------|
| **Copilot Free** | Individual developers | 2,000 completions + 50 chat messages/month | Limited |
| **Copilot Pro** | Power users | Unlimited completions, model selection | 1,000 ($10/mo) |
| **Copilot Pro+** | Advanced users | All Pro features + premium models | 3,900 ($39/mo) |
| **Copilot Business** | Teams & organizations | Admin controls, policy management, coding agent | 1,900/user ($19/user/mo, pooled) |
| **Copilot Enterprise** | Large enterprises | Knowledge bases, enhanced admin controls, Copilot Dashboard | 3,900/user ($39/user/mo, pooled) |

<div class="callout callout-warning">
<div class="callout-title">🆕 Usage-Based Billing (June 2026)</div>

Starting **June 1, 2026**, Copilot billing transitions from request-based to **usage-based billing with AI Credits** (1 credit = $0.01 USD). Key changes:

- **Token-based pricing**: Usage is measured by input, output, and cached tokens consumed per interaction
- **Model-specific rates**: Different AI models cost different amounts per million tokens (e.g., GPT-4.1 is cheaper than Claude Opus)
- **Monthly credit allotment**: Each plan includes credits equal to its dollar price; overage credits can be purchased
- **Code completions stay free**: Standard ghost text suggestions remain unlimited on all paid plans
- **Pooled credits**: For Business and Enterprise plans, credits are pooled across all org users
- **Budget controls**: Enterprise admins can set spend limits at enterprise, cost center, or user levels

For detailed per-model pricing, see the [Models and Pricing documentation](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing).
</div>

<div class="callout callout-info">
<div class="callout-title">🆕 Model Changes (2026)</div>

**Claude Opus 4.7** became generally available in April 2026, bringing stronger multi-step reasoning and task performance to all Copilot subscribers.

**Gemini 3 Pro was removed** from all Copilot experiences on March 26, 2026. If customers ask about Gemini in Copilot, refer them to the supported models page for what's currently available.

Model availability changes over time — always check the [supported AI models documentation](https://docs.github.com/en/copilot/using-github-copilot/ai-models/supported-ai-models-in-copilot) for the current list.
</div>

> **📚 Learn More:** [Copilot Plans and Pricing](https://docs.github.com/en/copilot/about-github-copilot/subscription-plans-for-github-copilot) | [Models and Pricing](https://docs.github.com/en/copilot/reference/copilot-billing/models-and-pricing)

---

## Why CSMs Need Copilot Expertise

As a Customer Success professional, understanding Copilot helps you:

### 1. **Drive Adoption**
Copilot is often the entry point for organizations exploring AI. Being able to demonstrate value quickly builds trust and opens doors for broader GitHub adoption.

### 2. **Measure Impact**
Copilot provides measurable productivity metrics:

- Code acceptance rates
- Time saved on repetitive tasks
- Developer satisfaction scores

### 3. **Address Concerns**
Common customer questions you'll encounter:

- "Is our code being used to train AI?"
- "How do we ensure code quality with AI assistance?"
- "What about security and compliance?"

### 4. **Identify Expansion Opportunities**
Understanding Copilot usage patterns helps identify:

- Teams ready for Business/Enterprise upgrades
- Use cases for Copilot Extensions
- Integration opportunities with Actions and Security

---

## Module Learning Path

This module takes you through a comprehensive Copilot journey:

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    A[1. Overview] --> B[2. Core Concepts]
    B --> C[3. IDE Setup]
    C --> D[4. Prompt Crafting]
    D --> E[5. Copilot Chat]
    E --> F[6. IDE Differences]
    F --> G[7. Best Practices]
    G --> H[8. Hands-On Labs]
    H --> I[9. Knowledge Check]
    I --> J[10. Resources]
    
    style A fill:#0969da,color:#fff
    style H fill:#1a7f37,color:#fff
</div>
</div>

**What You'll Learn:**

- ✅ How Copilot works under the hood
- ✅ Setting up Copilot across different IDEs
- ✅ Crafting effective prompts for better suggestions
- ✅ Using Copilot Chat for complex tasks
- ✅ Best practices for AI-assisted development

---

## Prerequisites

Before starting this module, you should have:

- Basic programming experience in any language
- Familiarity with at least one IDE (VS Code recommended)
- Access to GitHub Copilot (Free, Pro, Business, or Enterprise)
- Completed Modules 1-12 (recommended but not required)

---

## Ready to Begin?

Let's explore the core concepts behind GitHub Copilot and understand how AI-powered code generation works.
