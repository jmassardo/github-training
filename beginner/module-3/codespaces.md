---
layout: training-module
title: "GitHub Codespaces"
permalink: /beginner/module-3/codespaces/
module_number: 3
module_title: "GitHub Platform Essentials"
section_number: 8
total_sections: 9
phase: beginner
estimated_time: "30 min"
module_index: /beginner/module-3/
sections:
  - title: "Context & Overview"
    url: "/beginner/module-3/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/beginner/module-3/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/beginner/module-3/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/beginner/module-3/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/beginner/module-3/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/beginner/module-3/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/beginner/module-3/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
  - title: "GitHub Codespaces"
    url: "/beginner/module-3/codespaces/"
    short_title: "Codespaces"
    icon: "☁️"
  - title: "GitHub Discussions"
    url: "/beginner/module-3/discussions/"
    short_title: "Discussions"
    icon: "💬"
prev_section:
  url: /beginner/module-3/scenarios/
  title: "CSM Scenarios"
next_section:
  url: /beginner/module-3/discussions/
  title: "GitHub Discussions"
---

## What is GitHub Codespaces?

**GitHub Codespaces** is a cloud-based development environment that runs entirely in your browser. It provides a complete, configurable dev environment with a single click—no local setup required.

> 💡 **Think of it as:** VS Code in the cloud, connected directly to your repository, with all dependencies pre-installed.

---

## Why Codespaces Matters

### The Problem It Solves

Traditional development setup challenges:

| Challenge | Traditional Approach | With Codespaces |
|-----------|---------------------|-----------------|
| **Onboarding** | Hours/days installing tools | Minutes to start coding |
| **"Works on my machine"** | Environment inconsistencies | Identical environments for everyone |
| **Hardware limitations** | Need powerful local machine | Cloud compute handles the load |
| **Security** | Code on personal devices | Code stays in the cloud |
| **Maintenance** | Each dev maintains their setup | Centrally managed configurations |

### Key Benefits

- ☁️ **Instant Development Environments** - Start coding in seconds
- 🔧 **Pre-configured Setup** - Tools, extensions, and settings ready to go
- 🔄 **Consistency** - Every team member gets the same environment
- 💻 **Device Flexibility** - Code from any device with a browser
- 🔒 **Security** - Source code never leaves GitHub's infrastructure

---

## How Codespaces Works

<div class="mermaid-container">
<div class="mermaid">
flowchart TB
    subgraph Browser["Your Browser"]
        subgraph VSCode["VS Code Web"]
            Explorer["Explorer\n📁 src\n📁 tests\n📄 ..."]
            Editor["Editor\n// Your code here"]
            Terminal["Terminal: npm run dev"]
        end
    end
    subgraph Cloud["GitHub Codespaces - Cloud"]
        Code["Your Code\n(from repo)"]
        Tools["Dev Tools\nNode, Python\nDocker, etc"]
        Compute["Compute Resources\n2-32 cores\n4-64 GB RAM"]
    end
    Browser -- "Secure Connection" --> Cloud
</div>
</div>

---

## Creating a Codespace

### Quick Start (60 seconds)

1. Navigate to any repository on GitHub
2. Click the green **Code** button
3. Select the **Codespaces** tab
4. Click **Create codespace on main**

That's it! GitHub provisions a cloud VM, clones the repo, installs dependencies, and opens VS Code in your browser.

### From the Command Line

```bash
# Using GitHub CLI
gh codespace create --repo owner/repo-name

# List your codespaces
gh codespace list

# Connect to existing codespace
gh codespace ssh -c <codespace-name>
```

---

## Codespace Configuration

### Dev Containers (`.devcontainer/`)

Repositories can include configuration that defines the development environment:

```
.devcontainer/
├── devcontainer.json    # Main configuration
├── Dockerfile           # Custom container image (optional)
└── docker-compose.yml   # Multi-container setup (optional)
```

### Example `devcontainer.json`

```json
{
  "name": "Node.js Development",
  "image": "mcr.microsoft.com/devcontainers/javascript-node:18",
  
  "features": {
    "ghcr.io/devcontainers/features/github-cli:1": {}
  },
  
  "customizations": {
    "vscode": {
      "extensions": [
        "dbaeumer.vscode-eslint",
        "esbenp.prettier-vscode",
        "GitHub.copilot"
      ],
      "settings": {
        "editor.formatOnSave": true
      }
    }
  },
  
  "postCreateCommand": "npm install",
  "forwardPorts": [3000],
  
  "remoteUser": "node"
}
```

### What You Can Configure

| Setting | Purpose | Example |
|---------|---------|---------|
| `image` | Base container image | `mcr.microsoft.com/devcontainers/python:3.11` |
| `features` | Add tools/runtimes | GitHub CLI, Docker, AWS CLI |
| `extensions` | VS Code extensions | ESLint, Prettier, Copilot |
| `settings` | VS Code settings | Format on save, theme |
| `postCreateCommand` | Setup script | `npm install`, `pip install -r requirements.txt` |
| `forwardPorts` | Expose ports | `[3000, 5432, 8080]` |

---

## Machine Types & Billing

### Available Machine Types

| Type | Cores | RAM | Storage | Best For |
|------|-------|-----|---------|----------|
| **2-core** | 2 | 4 GB | 32 GB | Light editing, documentation |
| **4-core** | 4 | 8 GB | 32 GB | Most development work |
| **8-core** | 8 | 16 GB | 64 GB | Large projects, multiple services |
| **16-core** | 16 | 32 GB | 128 GB | Heavy compilation, data processing |
| **32-core** | 32 | 64 GB | 128 GB | Enterprise workloads |

### Billing Model

- **Compute**: Billed per hour of active use
- **Storage**: Billed per GB-month for stopped codespaces
- **Free tier**: GitHub Free includes 120 core-hours/month + 15 GB storage

> 💰 **Cost Tip:** Codespaces automatically stop after 30 minutes of inactivity. Configure shorter timeouts in organization settings to save costs.

---

## Codespaces vs. Local Development

| Aspect | Local Development | GitHub Codespaces |
|--------|-------------------|-------------------|
| **Setup time** | Hours to days | Minutes |
| **Hardware needs** | Powerful laptop | Any device with browser |
| **Consistency** | Varies by machine | Identical for all |
| **Offline work** | ✅ Full capability | ❌ Requires internet |
| **Cost** | Hardware + electricity | Usage-based billing |
| **Security** | Code on device | Code in cloud |
| **Customization** | Unlimited | Container-based |

### When to Use Codespaces

✅ **Great for:**
- Onboarding new team members
- Contributing to open source
- Quick bug fixes and code reviews
- Standardized team environments
- Remote/distributed teams
- Security-sensitive organizations

⚠️ **Consider alternatives for:**
- Offline development needs
- Extremely low-latency requirements
- Legacy systems requiring local hardware
- Very long-running processes

---

## Connecting via VS Code Desktop

Codespaces aren't limited to the browser. You can connect to a running codespace from **VS Code Desktop** for a native editor experience with cloud compute:

1. Install the **GitHub Codespaces extension** in VS Code
2. Open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`)
3. Search for **"Codespaces: Connect to Codespace"**
4. Select your running codespace

This gives you the best of both worlds — the full VS Code desktop experience with all your local extensions, backed by cloud-hosted compute and storage.

```bash
# Or connect via GitHub CLI
gh codespace code -c <codespace-name>
```

---

## Prebuilds

### What Are Prebuilds?

**Prebuilds** pre-build your dev container image so codespaces launch in seconds instead of minutes. Without prebuilds, every new codespace must build the container from scratch (installing dependencies, compiling, etc.).

> 💡 **Think of it like a restaurant:** Without prebuilds, every order is cooked from raw ingredients (slow). With prebuilds, common dishes are prepped in advance so they're ready almost instantly.

### How Prebuilds Work

| Without Prebuilds | With Prebuilds |
|-------------------|----------------|
| Clone repo → Build container → Install deps → Ready | Clone repo → Use pre-built image → Ready |
| 2-10 minutes | 10-30 seconds |
| Built on every new codespace | Built once, reused many times |
| Each user waits | Wait happens in the background |

### Configuring Prebuilds

Prebuilds are configured at the repository level:

1. Go to **Repository Settings** → **Codespaces** → **Prebuilds**
2. Click **Set up prebuild**
3. Choose the branch (typically `main`)
4. Select a region
5. Configure triggers (on push, on config change, or scheduled)

### When to Recommend Prebuilds

- Repositories with **long setup times** (heavy dependencies, large builds)
- Teams with **frequent contributor onboarding**
- Projects used for **demos or training** where fast startup matters
- Active repositories where **multiple developers** create codespaces daily

<div class="callout callout-info" markdown="1">
<div class="callout-title">💰 Billing Note</div>

Prebuilds consume **storage** (for the cached images) and **compute** (when rebuilding). The trade-off is faster codespace creation. For high-usage repositories, the time savings typically far outweigh the prebuild cost.
</div>

---

## Codespace Lifecycle

### Managing Your Codespaces

Codespaces have a lifecycle you should understand:

| State | Description | Billing |
|-------|-------------|---------|
| **Running** | Active and usable | Compute + storage billed |
| **Stopped** | Paused, data preserved | Storage only |
| **Deleted** | Removed permanently | No billing |

### Key Lifecycle Actions

```bash
# List all your codespaces
gh codespace list

# Stop a running codespace
gh codespace stop -c <codespace-name>

# Restart a stopped codespace
gh codespace start -c <codespace-name>

# Delete a codespace
gh codespace delete -c <codespace-name>

# Rebuild a codespace (re-runs container build)
gh codespace rebuild -c <codespace-name>
```

### Automatic Lifecycle Policies

Organizations can set policies to manage costs:

- **Idle timeout** — Auto-stop after inactivity (default: 30 minutes)
- **Retention period** — Auto-delete stopped codespaces after a set time
- **Maximum lifetime** — Force-stop long-running codespaces

---

## Secrets & Personalization

### Codespace Secrets

You can configure encrypted secrets that are available inside your codespaces:

- **User-level secrets** — Available in all your codespaces (Settings → Codespaces → Secrets)
- **Organization-level secrets** — Shared across org members' codespaces
- **Repository-level secrets** — Scoped to codespaces for a specific repo

This lets developers use API keys, tokens, and credentials without hardcoding them.

### Dotfiles

**Dotfiles** let you personalize every codespace with your preferred shell configuration, aliases, and tool settings:

1. Create a repository named `dotfiles` in your GitHub account
2. Add your config files (`.bashrc`, `.zshrc`, `.gitconfig`, `.vimrc`, etc.)
3. Go to **Settings** → **Codespaces** → enable **Automatically install dotfiles**

Every new codespace will automatically clone your dotfiles repo and apply your personalization. This means your aliases, shell prompt, and tool preferences follow you everywhere.

---

## Enterprise Codespaces

### Organization Controls

Administrators can configure:

- **Allowed repositories** - Which repos can use Codespaces
- **Machine type limits** - Cap compute resources
- **Idle timeout** - Auto-stop after inactivity
- **Retention period** - Auto-delete unused codespaces
- **Spending limits** - Monthly budget caps

### Security Features

- **Network isolation** - Connect to private resources via VPN
- **Secrets management** - Encrypted environment variables
- **Audit logging** - Track codespace creation and access
- **Image control** - Require approved base images

---

## CSM Talking Points

### Value Propositions by Persona

**For Engineering Leaders:**
> "Codespaces reduces developer onboarding from days to minutes. New hires can make their first commit on day one, not week one."

**For Security Teams:**
> "Source code never leaves GitHub's infrastructure. Developers can contribute from any device without downloading sensitive code."

**For Finance/Procurement:**
> "Replace expensive developer workstations with any device. Pay only for compute time actually used."

**For Platform Teams:**
> "Define your development environment as code. Every developer gets the exact same setup, eliminating 'works on my machine' issues."

### Common Objections & Responses

| Objection | Response |
|-----------|----------|
| "We need to work offline" | "Codespaces complements local development. Use it for quick tasks, onboarding, and standardization while keeping local for offline needs." |
| "It's too expensive" | "Compare to laptop costs, IT support time, and developer hours lost to setup. Many teams see ROI within months." |
| "Our setup is too complex" | "Dev containers can replicate any environment—Docker, multiple services, specific versions. If it runs locally, it can run in Codespaces." |
| "Security concerns" | "Code stays in GitHub's SOC 2 compliant infrastructure. No source code on personal devices actually improves security posture." |

### Discovery Questions

- "How long does it take a new developer to make their first commit?"
- "How often do you hear 'it works on my machine'?"
- "What percentage of your team works remotely or uses multiple devices?"
- "How do you handle contractors or short-term contributors?"
- "What's your current approach to securing source code on devices?"

---

## Hands-On Exercise

### Try It Now (5 minutes)

1. Go to [github.com/codespaces/templates](https://github.com/codespaces/templates)
2. Choose a template (e.g., "Blank" or "React")
3. Click **Use this template** → **Open in a codespace**
4. Wait for the environment to load (~60 seconds)
5. Make a change, see it preview, then delete the codespace

### Explore Dev Containers

1. Visit [github.com/devcontainers/templates](https://github.com/devcontainers/templates)
2. Browse available configurations
3. Try opening a customer's repository in a codespace

---

## Key Takeaways

1. **Codespaces = Cloud IDE** - Full VS Code experience in your browser
2. **Configuration as Code** - Dev containers define reproducible environments
3. **Instant Onboarding** - New contributors productive in minutes
4. **Enterprise Ready** - Policies, security, and cost controls for organizations
5. **Flexible Billing** - Pay for what you use, free tier available

---

## Additional Resources

- 📖 [GitHub Codespaces Documentation](https://docs.github.com/en/codespaces)
- 🎓 [GitHub Skills: Code with Codespaces](https://github.com/skills/code-with-codespaces)
- 🔧 [Dev Container Specification](https://containers.dev/)
- 💰 [Codespaces Billing](https://docs.github.com/en/billing/managing-billing-for-github-codespaces)
- 🏢 [Managing Codespaces for Your Organization](https://docs.github.com/en/codespaces/managing-codespaces-for-your-organization)
