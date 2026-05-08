---
layout: training-module
title: "Core Concepts"
permalink: /advanced/module-13/concepts/
module_number: 13
module_title: "GitHub Copilot Fundamentals"
section_number: 2
total_sections: 10
phase: advanced
estimated_time: "30 min"
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
  url: /advanced/module-13/overview/
  title: "Context & Overview"
next_section:
  url: /advanced/module-13/setup/
  title: "IDE Setup"
---

<div class="callout callout-info" markdown="1">
<div class="callout-title">🤖 Official Documentation</div>
For comprehensive reference, see <a href="https://docs.github.com/en/copilot">GitHub Copilot Documentation</a> and the <a href="https://docs.github.com/en/copilot/getting-started-with-github-copilot">Getting Started Guide</a>.
</div>

## How GitHub Copilot Works

Understanding Copilot's architecture helps you use it more effectively and address customer questions with confidence.

### The Copilot Architecture

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph IDE["Your IDE"]
        A[Editor Context]
        B[Open Files]
        C[Cursor Position]
    end
    
    subgraph Service["GitHub Copilot Service"]
        D[Context Processing]
        E[LLM Models]
        F[Response Generation]
    end
    
    subgraph Azure["Microsoft Azure"]
        G[Secure Infrastructure]
        H[SOC 2 Compliant]
    end
    
    IDE --> |Encrypted Request| Service
    Service --> Azure
    Service --> |Suggestions| IDE
    
    style IDE fill:#0969da,color:#fff
    style Service fill:#8250df,color:#fff
    style Azure fill:#1a7f37,color:#fff
</div>
</div>

**Key Points:**

- Your code context is sent securely to GitHub's Copilot service
- Processing happens on Microsoft Azure infrastructure
- Suggestions are returned to your IDE in real-time
- Business/Enterprise plans include additional privacy controls

> **📚 Learn More:** [GitHub Copilot Trust Center](https://copilot.github.trust.page/)

---

## LLM Fundamentals for Developers

### What Are Large Language Models?

Large Language Models (LLMs) are neural networks trained on massive datasets of text and code. They learn statistical patterns in language and can generate human-like text—including code.

| Component | Description | Analogy |
|-----------|-------------|---------|
| **Training Data** | Billions of lines of code and documentation | A developer reading thousands of projects |
| **Neural Network** | Layers of mathematical transformations | Brain synapses learning patterns |
| **Weights** | Learned parameters from training | Memory of patterns and associations |
| **Inference** | Generating predictions from input | Applying experience to new problems |

### How Code Generation Works

When you type code, Copilot:

1. **Tokenizes** your input - breaks code into meaningful pieces
2. **Encodes** context - converts to numerical representations
3. **Processes** through the model - applies learned patterns
4. **Generates** tokens - predicts likely next pieces
5. **Decodes** output - converts back to readable code

<div class="mermaid-container">
<div class="mermaid">
sequenceDiagram
    participant Dev as Developer
    participant IDE as IDE Extension
    participant API as Copilot API
    participant LLM as LLM Model
    
    Dev->>IDE: Types code
    IDE->>API: Send context (file, cursor, open tabs)
    API->>LLM: Process with model
    LLM->>API: Generate tokens
    API->>IDE: Return suggestions
    IDE->>Dev: Display ghost text
    Dev->>IDE: Accept (Tab) or Reject (Esc)
</div>
</div>

---

## Context: The Key to Good Suggestions

Copilot's suggestions are only as good as the context it receives. Understanding what context is used helps you write better code and get better suggestions.

### What Context Does Copilot Use?

| Context Source | Description | Priority |
|----------------|-------------|----------|
| **Current File** | The code in your active editor | Highest |
| **Cursor Position** | Where you're typing | Highest |
| **Open Tabs** | Related files in your IDE | High |
| **File Type** | Language and framework detection | High |
| **Comments** | Documentation above code | Medium |
| **Function Names** | Semantic meaning from naming | Medium |

### Tips for Better Context

> **💡 Pro Tip:** Think of context like explaining a task to a junior developer. The more relevant information you provide, the better the help you'll get.

**DO:**

- Keep related files open in tabs
- Write descriptive comments before complex functions
- Use meaningful variable and function names
- Include type hints in dynamically typed languages

**DON'T:**

- Expect perfect suggestions from an empty file
- Assume Copilot knows your entire codebase
- Rely on context from closed files
- Use vague or generic names

---

## Privacy and Security

This is one of the most common customer concerns. Here's what you need to know:

### Data Handling by Plan

| Aspect | Free/Pro | Business | Enterprise |
|--------|----------|----------|------------|
| Code used for training | Not by default (opt-in) | No | No |
| Telemetry collection | Yes | Configurable | Configurable |
| Content exclusions | No | Yes | Yes |
| Audit logs | No | Yes | Yes |
| IP indemnification | No | Yes | Yes |

### Key Privacy Features

**For Business and Enterprise:**

- Code is NOT retained after suggestions are generated
- Prompts and suggestions are encrypted in transit
- Organization admins can configure policies
- Content exclusion rules can block sensitive repositories

> **📚 Learn More:** [Copilot Privacy FAQ](https://docs.github.com/en/copilot/copilot-individual/about-github-copilot-individual#about-privacy-for-github-copilot-individual)

---

## The Suggestion Lifecycle

Understanding how suggestions flow helps you work more efficiently with Copilot.

### Types of Suggestions

| Type | Trigger | Example |
|------|---------|---------|
| **Inline** | Automatic as you type | Completing a function body |
| **Multi-line** | After comment or signature | Generating entire functions |
| **Ghost Text** | Visible but not inserted | Gray text showing suggestions |
| **Completions Panel** | Keyboard shortcut | Multiple alternatives to choose from |

### Accepting and Rejecting

| Action | Keyboard | Result |
|--------|----------|--------|
| Accept all | `Tab` | Insert entire suggestion |
| Accept word | `Ctrl+→` | Insert next word only |
| Accept line | `Ctrl+End` | Insert to end of line |
| Reject | `Esc` | Dismiss suggestion |
| Next suggestion | `Alt+]` | Cycle through alternatives |
| Previous suggestion | `Alt+[` | Go back to previous |

---

## Understanding Model Limitations

Being honest about limitations builds trust with customers and helps developers use Copilot appropriately.

### What Copilot Does Well

- ✅ Boilerplate code and common patterns
- ✅ API usage with popular libraries
- ✅ Unit test generation
- ✅ Documentation and comments
- ✅ Translating between similar languages

### Where Copilot May Struggle

- ⚠️ Very new APIs or libraries (not in training data)
- ⚠️ Highly domain-specific code
- ⚠️ Complex business logic unique to your organization
- ⚠️ Security-critical code (always review!)
- ⚠️ Code requiring real-time data or current information

### The Human-in-the-Loop

> **🔑 Key Principle:** Copilot suggests, developers decide. Every suggestion should be reviewed and understood before acceptance.

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    A[Copilot Suggests] --> B{Developer Reviews}
    B -->|Looks Good| C[Accept & Test]
    B -->|Needs Work| D[Modify]
    B -->|Not Right| E[Reject]
    C --> F[Verify Correctness]
    D --> F
    
    style A fill:#8250df,color:#fff
    style B fill:#bf8700,color:#fff
    style F fill:#1a7f37,color:#fff
</div>
</div>

---

## Copilot CLI: Terminal-Native AI Assistance

GitHub Copilot CLI is a **standalone terminal tool** that brings full conversational AI and agentic capabilities directly to your command line — no IDE required.

<div class="callout callout-warning">
<div class="callout-title">⚠️ Deprecation Notice</div>

The previous `gh copilot` extension was **deprecated in October 2025** and is no longer functional. All terminal-based Copilot usage now goes through the standalone `copilot` CLI. If customers mention `gh copilot`, guide them to migrate to the new tool.
</div>

### What Copilot CLI Does

The standalone Copilot CLI provides an interactive, conversational AI experience in the terminal:

| Feature | Description |
|---------|-------------|
| **Interactive chat** | Full conversational interface with context awareness |
| **File editing** | Read, create, and edit files directly from the terminal |
| **Command execution** | Run shell commands with AI guidance and approval flows |
| **Plan mode** | Multi-step task planning and execution |
| **Autopilot mode** | Autonomous agentic workflows with trust controls |
| **MCP support** | Connect to external tools via Model Context Protocol |

### Why This Matters

<div class="callout callout-tip">
<div class="callout-title">💡 CSM Insight</div>

Copilot CLI is available to **all Copilot subscribers** — Free, Pro, Business, and Enterprise. It's an easy win for developers who spend significant time in the terminal, especially DevOps engineers and platform teams who may not use a traditional IDE. The standalone CLI is far more capable than the old `gh copilot` extension — it supports full agentic workflows, not just explain/suggest.
</div>

### Getting Started

```bash
# Install the standalone Copilot CLI
brew install copilot-cli          # macOS
winget install GitHub.Copilot     # Windows
npm install -g @github/copilot    # Cross-platform via npm

# Authenticate with GitHub
copilot login

# Launch the interactive interface
copilot

# Check version and updates
copilot version
copilot update
```

### Key Commands and Shortcuts

| Command / Shortcut | Purpose |
|---------|---------|
| `copilot` | Launch interactive interface |
| `copilot login` | Authenticate via OAuth device flow |
| `copilot init` | Initialize custom instructions for a repository |
| `Shift+Tab` | Cycle between standard, plan, and autopilot mode |
| `@ FILENAME` | Include file contents in context |
| `# NUMBER` | Include a GitHub issue or PR in context |
| `! COMMAND` | Execute a shell command directly |

### How It Works

Copilot CLI provides a full interactive terminal session powered by LLMs. Unlike the old `gh copilot` extension (which only offered explain/suggest), the standalone CLI supports multi-turn conversations, file operations, command execution with approval prompts, and agentic plan-and-execute workflows.

> **📚 Learn More:** [Copilot CLI Command Reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference)

---

## CSM Conversation Starters

Use these talking points with customers:

### For Skeptics
> "Copilot isn't about replacing developers—it's about eliminating the boring parts so they can focus on creative problem-solving."

### For Security-Conscious
> "With Copilot Business, your code is never retained and you have full control over policies and exclusions."

### For ROI-Focused
> "GitHub's research shows developers using Copilot complete tasks up to 55% faster, with higher satisfaction rates."

### For the Curious
> "The best way to understand Copilot is to try it. Would you like to walk through a demo together?"

---

## Summary

Key takeaways from this section:

| Concept | Key Point |
|---------|-----------|
| **Architecture** | Secure cloud service processing context in real-time |
| **LLMs** | Pattern-based generation, not code retrieval |
| **Context** | Better context = better suggestions |
| **Privacy** | Business/Enterprise have strongest controls |
| **Limitations** | AI assists, humans decide |
| **Copilot CLI** | Terminal-native AI via standalone `copilot` CLI — available to all plan tiers |

---

## Next Steps

Now that you understand how Copilot works, let's get it set up in your preferred IDE.
