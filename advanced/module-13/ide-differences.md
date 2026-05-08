---
layout: training-module
title: "IDE Differences"
permalink: /advanced/module-13/ide-differences/
module_number: 13
module_title: "GitHub Copilot Fundamentals"
section_number: 6
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
  url: /advanced/module-13/chat/
  title: "Copilot Chat"
next_section:
  url: /advanced/module-13/best-practices/
  title: "Best Practices"
---

## Feature Availability by IDE

Not all Copilot features are available in every IDE. This section helps you understand what's available where and how to work with different IDEs.

### Feature Comparison Matrix

| Feature | VS Code | JetBrains | Visual Studio | Neovim | Xcode |
|---------|---------|-----------|---------------|--------|-------|
| **Inline Completions** | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Multi-line Completions** | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Copilot Chat Panel** | ✅ | ✅ | ✅ | ⚠️ | ✅ |
| **Inline Chat** | ✅ | ✅ | ✅ | ❌ | ❌ |
| **@workspace** | ✅ | ✅ | ✅ | ❌ | ❌ |
| **Model Selection** | ✅ | ✅ | ✅ | ❌ | ❌ |
| **Agent Mode** | ✅ | ✅ | ⚠️ | ❌ | ❌ |
| **Copilot Edits** | ✅ | ✅ | ⚠️ | ❌ | ❌ |

> **📚 Learn More:** [IDE Feature Availability](https://docs.github.com/en/copilot/github-copilot-chat/copilot-chat-in-ides)

---

## VS Code

VS Code offers the most complete Copilot experience, being the primary development platform for Copilot features.

### Unique VS Code Features

| Feature | Description |
|---------|-------------|
| **Copilot Edits** | Multi-file editing with AI guidance |
| **Agent Mode** | Autonomous coding with terminal access |
| **@workspace** | Full codebase queries |
| **Model Selection** | Choose between available models (GPT-4o, Claude, and more — see [supported models](https://docs.github.com/en/copilot/using-github-copilot/ai-models/supported-ai-models-in-copilot)) |
| **Terminal Integration** | `@terminal` for shell help |
| **Notebook Support** | Jupyter notebook AI assistance |

### VS Code Shortcuts

| Action | Windows/Linux | macOS |
|--------|---------------|-------|
| Accept suggestion | `Tab` | `Tab` |
| Inline Chat | `Ctrl+I` | `Cmd+I` |
| Chat Panel | `Ctrl+Alt+I` | `Cmd+Alt+I` |
| Quick Chat | `Ctrl+Shift+I` | `Cmd+Shift+I` |
| Next suggestion | `Alt+]` | `Option+]` |
| Previous suggestion | `Alt+[` | `Option+[` |

### VS Code Settings

```json
{
  "github.copilot.enable": {
    "*": true,
    "markdown": true,
    "yaml": true
  },
  "github.copilot.editor.enableAutoCompletions": true,
  "github.copilot.chat.localeOverride": "en"
}

```

---

## JetBrains IDEs

JetBrains IDEs (IntelliJ IDEA, PyCharm, WebStorm, etc.) offer robust Copilot support with deep integration into their powerful IDE features.

### JetBrains-Specific Features

| Feature | Status | Notes |
|---------|--------|-------|
| **Inline Completions** | ✅ Full | Works across all JetBrains IDEs |
| **Chat Panel** | ✅ Full | Integrated tool window |
| **Inline Chat** | ✅ Full | Right-click context menu |
| **Code Analysis Integration** | ✅ | Works with JetBrains inspections |
| **Refactoring Integration** | ⚠️ | Limited compared to native refactoring |

### JetBrains Shortcuts

| Action | Windows/Linux | macOS |
|--------|---------------|-------|
| Accept suggestion | `Tab` | `Tab` |
| Show suggestions | `Alt+\` | `Option+\` |
| Next suggestion | `Alt+]` | `Option+]` |
| Previous suggestion | `Alt+[` | `Option+[` |
| Open Chat | `Alt+C` | `Option+C` |

### JetBrains Configuration

Navigate to **Settings** → **Tools** → **GitHub Copilot**:

- Enable/disable for specific file types
- Configure suggestion behavior
- Set up proxy for enterprise networks

---

## Visual Studio

Visual Studio 2022 provides Copilot integration optimized for .NET development.

### Visual Studio Features

| Feature | Status | Notes |
|---------|--------|-------|
| **Inline Completions** | ✅ Full | Integrated with IntelliSense |
| **Chat Panel** | ✅ Full | Docked tool window |
| **Inline Chat** | ✅ Full | Context menu integration |
| **Solution-Wide Context** | ✅ | Understands .NET solution structure |
| **Debugging Integration** | ✅ | Explain exceptions, suggest fixes |

### Visual Studio Shortcuts

| Action | Shortcut |
|--------|----------|
| Accept suggestion | `Tab` |
| Open Copilot Chat | `Ctrl+\, Ctrl+C` |
| Inline Chat | `Alt+/` |
| Next suggestion | `Alt+.` |
| Previous suggestion | `Alt+,` |

### Visual Studio Requirements

- Visual Studio 2022 version 17.8 or later
- .NET Framework 4.7.2+ or .NET Core 3.1+
- GitHub Copilot extension installed

---

## Neovim

For terminal-focused developers, Neovim offers Copilot through the official vim plugin.

### Neovim Features

| Feature | Status | Notes |
|---------|--------|-------|
| **Inline Completions** | ✅ Full | Ghost text support |
| **Chat** | ⚠️ Limited | Via copilot.lua or external tools |
| **Context** | ✅ | Current buffer + open buffers |
| **Customization** | ✅ Excellent | Full Lua configuration |

### Neovim Configuration

```lua
-- lazy.nvim configuration
{
  "github/copilot.vim",
  event = "InsertEnter",
  config = function()
    vim.g.copilot_no_tab_map = true
    vim.g.copilot_assume_mapped = true
    vim.keymap.set("i", "<C-J>", 'copilot#Accept("\\<CR>")', {
      expr = true,
      replace_keycodes = false
    })
    vim.keymap.set("i", "<C-]>", "<Plug>(copilot-next)")
    vim.keymap.set("i", "<C-[>", "<Plug>(copilot-previous)")
  end,
}

```

### Alternative: copilot.lua

For a more Neovim-native experience:

```lua
{
  "zbirenbaum/copilot.lua",
  cmd = "Copilot",
  event = "InsertEnter",
  config = function()
    require("copilot").setup({
      suggestion = {
        auto_trigger = true,
        keymap = {
          accept = "<C-y>",
          next = "<C-]>",
          prev = "<C-[>",
        },
      },
    })
  end,
}

```

---

## Xcode

Xcode support brings Copilot to iOS and macOS developers.

### Xcode Features

| Feature | Status | Notes |
|---------|--------|-------|
| **Inline Completions** | ✅ Full | Native integration |
| **Chat** | ✅ Full | Separate panel |
| **Swift Support** | ✅ Excellent | Strong language understanding |
| **Objective-C Support** | ✅ Good | Reasonable suggestions |
| **SwiftUI** | ✅ Good | Understands declarative patterns |

### Xcode Installation

1. Download Copilot for Xcode from GitHub
2. Move to Applications folder
3. Enable in **System Preferences** → **Privacy & Security** → **Accessibility**
4. Sign in with GitHub account

---

## Copilot in the CLI

GitHub Copilot CLI is a **standalone terminal tool** that brings full conversational AI and agentic capabilities directly to your command line. The previous `gh copilot` extension was deprecated in October 2025 and replaced by this more powerful standalone tool.

<div class="callout callout-warning">
<div class="callout-title">⚠️ Migration Notice</div>

If your customers are still using `gh copilot`, they need to migrate to the standalone `copilot` CLI. The old extension is no longer functional. See the [migration guide](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference) for details.
</div>

### Installation

```bash
# Install the standalone Copilot CLI
brew install copilot-cli          # macOS
winget install GitHub.Copilot     # Windows
npm install -g @github/copilot    # Cross-platform via npm

# Authenticate with GitHub
copilot login

# For GitHub Enterprise Cloud with data residency
copilot login --host https://example.ghe.com

# Launch the interactive interface
copilot

# Check version
copilot version
```

### Core Features

| Feature | Description |
|---------|-------------|
| **Interactive chat** | Full conversational interface with multi-turn context |
| **Plan mode** | Multi-step task planning and autonomous execution |
| **Autopilot mode** | Fully autonomous agentic workflows |
| **File operations** | Read, create, and edit files from the terminal |
| **Command execution** | Run shell commands with approval prompts |
| **MCP support** | Connect external tools via Model Context Protocol |
| **Tab completion** | Shell completions for bash, zsh, and fish |

### Interactive Interface

The Copilot CLI provides a rich interactive experience:

```bash
# Launch and start chatting
copilot

# Include files in context with @
# @src/auth.js Explain this authentication flow

# Reference GitHub issues/PRs with #
# #42 What changes are needed for this issue?

# Run shell commands directly with !
# !npm test

# Cycle modes with Shift+Tab:
# Standard → Plan → Autopilot
```

### Key Shortcuts

| Shortcut | Purpose |
|----------|---------|
| `Shift+Tab` | Cycle between standard, plan, and autopilot mode |
| `@ FILENAME` | Include file contents in context |
| `# NUMBER` | Reference a GitHub issue or PR |
| `! COMMAND` | Execute a shell command |
| `Ctrl+C` | Cancel / clear (press twice to exit) |
| `Ctrl+V` | Paste from clipboard as attachment |
| `Ctrl+R` | Reverse search command history |

### Shell Integration

Set up tab completion for your shell:

```bash
# Bash (persistent, Linux)
copilot completion bash | sudo tee /etc/bash_completion.d/copilot

# Zsh
copilot completion zsh > "${fpath[1]}/_copilot"

# Fish
copilot completion fish > ~/.config/fish/completions/copilot.fish
```

### Use Cases for CSMs

**Helping customers with Git:**
```bash
# Ask Copilot CLI about a complex Git operation interactively
copilot
# "Explain what git reflog expire --expire=now --all && git gc --prune=now does"
```

**Troubleshooting Actions:**
```bash
# Understanding workflow commands
copilot
# "What does this Actions runner config command do: config.sh --url https://github.com/org/repo --token TOKEN"
```

**Quick demos:**
```bash
# Generate commands for demos without memorizing syntax
copilot
# "Show me how to list all GitHub Actions secrets for a repo using the gh CLI"
```

### Authentication

Copilot CLI supports multiple authentication methods:

- **Browser-based OAuth** (default): `copilot login`
- **Environment variables**: `COPILOT_GITHUB_TOKEN`, `GH_TOKEN`, or `GITHUB_TOKEN`
- **Fine-grained PATs**: With the "Copilot Requests" permission

### Requirements

- Active GitHub Copilot subscription (Free, Pro, Business, or Enterprise)
- Authenticated via `copilot login` or environment variable

> **📚 Learn More:** [Copilot CLI Command Reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-command-reference)

---

## Copilot on GitHub.com

Beyond IDEs and CLI, Copilot offers powerful features directly on GitHub.com.

### Copilot Code Review

Request AI-powered code review on pull requests:

**How to use:**

1. Open a pull request
2. Click **Request review** 
3. Select **Copilot** as a reviewer
4. Copilot analyzes changes and leaves comments

**What Copilot reviews:**

- Logic errors and bugs
- Security vulnerabilities
- Performance issues
- Code style inconsistencies
- Missing error handling

**Example Copilot review comment:**
```
⚠️ Potential SQL injection vulnerability

The user input `searchTerm` is concatenated directly into the query.
Consider using parameterized queries:

- query = f"SELECT * FROM users WHERE name = '{searchTerm}'"
+ query = "SELECT * FROM users WHERE name = %s"
+ cursor.execute(query, (searchTerm,))
```

> **📚 Learn More:** [Copilot Code Review](https://docs.github.com/en/copilot/using-github-copilot/code-review/using-copilot-code-review)

### Copilot Pull Request Summaries

Generate AI summaries of pull request changes:

**How to use:**

1. Create or open a pull request
2. Click **Copilot** icon in the description area
3. Select **Generate summary**
4. Edit as needed before publishing

**Generated summary includes:**

- Overview of changes
- List of modified files with descriptions
- Potential impacts
- Suggested reviewer focus areas

**Example generated summary:**
```markdown
## Summary
This PR adds user authentication using JWT tokens.

## Changes
- **auth/jwt.py**: New JWT token generation and validation
- **routes/login.py**: Login endpoint with credential verification
- **middleware/auth.py**: Request authentication middleware
- **tests/test_auth.py**: Unit tests for authentication flow

## Reviewer Notes
Focus on token expiration handling and error responses.
```

> **📚 Learn More:** [PR Summaries](https://docs.github.com/en/copilot/using-github-copilot/creating-a-pull-request-summary-with-github-copilot)

### Copilot Text Completion

AI-assisted text completion for PR descriptions and issue bodies:

**How it works:**

- Start typing in a PR description or issue body
- Copilot suggests completions as ghost text
- Press `Tab` to accept suggestions

**Best for:**

- Writing detailed PR descriptions
- Creating comprehensive issue reports
- Documenting acceptance criteria

> **📚 Learn More:** [Text Completion](https://docs.github.com/en/copilot/using-github-copilot/using-copilot-text-completion)

---

## Copilot in GitHub Desktop

GitHub Desktop integrates Copilot for commit message generation.

### Generate Commit Messages

1. Stage your changes in GitHub Desktop
2. Click the **Copilot** icon in the commit message area
3. Copilot analyzes your staged changes
4. Review and edit the generated message
5. Commit with the AI-generated message

**Example:**
```
# Staged changes:
# - Modified src/auth/login.js
# - Added src/auth/logout.js
# - Modified tests/auth.test.js

# Generated commit message:
feat(auth): add logout functionality and update tests

- Implement logout endpoint that invalidates session
- Add logout button to navigation component
- Update auth tests to cover logout flow
```

### Requirements

- GitHub Desktop 3.0+
- Copilot subscription
- Signed in to GitHub account

> **📚 Learn More:** [Copilot in GitHub Desktop](https://docs.github.com/en/desktop)

---

## Next Edit Suggestions

Next Edit Suggestions predict where you'll edit next and offer completions proactively.

### How It Works

Unlike inline completions (which suggest at your cursor), Next Edit Suggestions:

1. Analyze your recent edits
2. Predict the next location you'll likely change
3. Show suggestions at that location before you navigate there

### Example Scenario

You rename a function parameter:
```javascript
// You changed 'data' to 'userData' here
function processUser(userData) {
  // Copilot predicts you'll update the reference below
  console.log(data); // ← Suggestion appears: "userData"
}
```

### Availability

- **VS Code**: Generally available
- **JetBrains**: Generally available
- **Xcode**: Generally available
- **Eclipse**: Available in preview

> **📚 Learn More:** [Next Edit Suggestions](https://docs.github.com/en/copilot/using-github-copilot/getting-code-suggestions-in-your-ide-with-github-copilot)

---

## Copilot Edits

Copilot Edits enables multi-file editing from a single prompt.

### Edit Mode vs Agent Mode

| Aspect | Edit Mode | Agent Mode |
|--------|-----------|------------|
| **Control** | You specify files | Copilot determines files |
| **Scope** | Defined set of files | Entire workspace |
| **Autonomy** | Low (review each change) | High (autonomous execution) |
| **Terminal** | No terminal commands | Can run terminal commands |
| **Best for** | Precise, scoped changes | Complex multi-step tasks |

### Using Edit Mode

1. Open Copilot Chat in VS Code
2. Click **Edit** mode (or use `Ctrl+Shift+I`)
3. Add files to the working set
4. Describe your changes
5. Review and accept/reject each change

**Example:**
```
Working set: src/api/*.ts

Prompt: "Add input validation to all POST endpoints 
using Zod schemas"

Copilot proposes changes → Review each file → Accept/Reject
```

### Using Agent Mode

1. Switch to **Agent** mode in Copilot Edits
2. Describe your task
3. Copilot autonomously:
   - Determines which files to modify
   - Makes changes
   - Runs terminal commands if needed
   - Iterates until task is complete

**Example:**
```
Prompt: "Add a new user preferences API with 
database migrations and tests"

Agent: Analyzing project structure...
Agent: Creating migration file...
Agent: Creating model...
Agent: Creating API routes...
Agent: Running migrations...
Agent: Creating tests...
Agent: Running tests... ✅ All passing
```

> **📚 Learn More:** [Copilot Edits](https://docs.github.com/en/copilot/using-github-copilot/copilot-edits)

---

## Choosing the Right IDE

### For Web Development
**Recommended: VS Code**
- Best JavaScript/TypeScript support
- Extensive extension ecosystem
- Full Copilot feature set

### For .NET Development
**Recommended: Visual Studio**
- Deep C#/.NET integration
- Solution-aware suggestions
- Debugging integration

### For Java Development
**Recommended: IntelliJ IDEA**
- Superior Java refactoring
- Spring/Maven/Gradle awareness
- Robust Copilot integration

### For Python Data Science
**Recommended: VS Code + Jupyter**
- Notebook support
- Python-specific completions
- Data science extensions

### For Terminal Enthusiasts
**Recommended: Neovim + CLI**
- Keyboard-driven workflow
- Lightweight and fast
- Full customization

---

## Summary

| IDE | Best For | Copilot Strength |
|-----|----------|------------------|
| **VS Code** | Web dev, Python, general | Most complete features |
| **JetBrains** | Java, Kotlin, enterprise | Deep IDE integration |
| **Visual Studio** | .NET, C++, Windows | Solution awareness |
| **Neovim** | Terminal users | Customization |
| **Xcode** | iOS/macOS | Swift support |
| **CLI** | DevOps, scripting | Command-line help |

---

## Next Steps

Now let's explore best practices for getting the most out of Copilot.
