---
layout: training-module
title: "IDE Setup & Configuration"
permalink: /advanced/module-13/setup/
module_number: 13
module_title: "GitHub Copilot Fundamentals"
section_number: 3
total_sections: 10
phase: advanced
estimated_time: "25 min"
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
  url: /advanced/module-13/concepts/
  title: "Core Concepts"
next_section:
  url: /advanced/module-13/prompts/
  title: "Prompt Crafting"
---

## Supported IDEs

GitHub Copilot supports a wide range of development environments. Here's the current landscape:

| IDE | Extension | Copilot Chat | Notes |
|-----|-----------|--------------|-------|
| **VS Code** | ✅ Full | ✅ Full | Best experience, most features |
| **Visual Studio** | ✅ Full | ✅ Full | 2022 17.4+ required |
| **JetBrains IDEs** | ✅ Full | ✅ Full | IntelliJ, PyCharm, WebStorm, etc. |
| **Neovim** | ✅ Full | ⚠️ Limited | Lua plugin required |
| **Vim** | ✅ Basic | ❌ No | Basic completions only |
| **Xcode** | ✅ Full | ✅ Beta | macOS only |
| **Eclipse** | ⚠️ Preview | ❌ No | Limited functionality |

> **📚 Learn More:** [Supported IDEs](https://docs.github.com/en/copilot/github-copilot-chat/using-github-copilot-chat-in-your-ide)

---

## VS Code Setup

VS Code offers the most complete Copilot experience. Here's how to set it up:

### Step 1: Install the Extension

1. Open VS Code
2. Go to Extensions (`Ctrl+Shift+X` / `Cmd+Shift+X`)
3. Search for "GitHub Copilot"
4. Click **Install** on the official extension

### Step 2: Sign In

1. Click the Copilot icon in the status bar
2. Select "Sign in to GitHub"
3. Complete OAuth authentication
4. Authorize the Copilot application

### Step 3: Verify Installation

Open a new file and start typing. You should see ghost text suggestions appear:

```python
# Create a function to calculate fibonacci
def fibonacci(n):
    # Copilot should suggest the implementation here

```

### Recommended VS Code Settings

```json
{
  "github.copilot.enable": {
    "*": true,
    "markdown": true,
    "plaintext": false
  },
  "github.copilot.editor.enableAutoCompletions": true,
  "editor.inlineSuggest.enabled": true
}

```

> **📚 Learn More:** [VS Code Copilot Setup](https://docs.github.com/en/copilot/using-github-copilot/getting-code-suggestions-in-your-ide-with-github-copilot)

---

## JetBrains IDE Setup

JetBrains IDEs (IntelliJ IDEA, PyCharm, WebStorm, etc.) have excellent Copilot support.

### Installation Steps

1. Open your JetBrains IDE
2. Go to **Settings/Preferences** → **Plugins**
3. Search for "GitHub Copilot"
4. Click **Install** and restart the IDE
5. Go to **Tools** → **GitHub Copilot** → **Login to GitHub**

### JetBrains-Specific Settings

| Setting | Location | Recommended |
|---------|----------|-------------|
| Auto-suggestions | Preferences → GitHub Copilot | Enabled |
| Completion trigger | Preferences → Editor → General | Automatic |
| Ghost text color | Preferences → Editor → Color Scheme | High contrast |

> **📚 Learn More:** [JetBrains Copilot Setup](https://docs.github.com/en/copilot/using-github-copilot/getting-code-suggestions-in-your-ide-with-github-copilot?tool=jetbrains)

---

## Visual Studio Setup

For .NET and C++ developers using Visual Studio on Windows.

### Requirements

- Visual Studio 2022 version 17.4 or later
- Active GitHub account with Copilot access

### Installation

1. Go to **Extensions** → **Manage Extensions**
2. Search for "GitHub Copilot"
3. Download and install
4. Restart Visual Studio
5. Sign in via **Tools** → **Options** → **GitHub** → **Copilot**

### Visual Studio Configuration

```plaintext
Tools → Options → GitHub → Copilot
├── Enable GitHub Copilot: ✓
├── Show completions inline: ✓
└── Enable Copilot Chat: ✓

```

> **📚 Learn More:** [Visual Studio Copilot Setup](https://docs.github.com/en/copilot/using-github-copilot/getting-code-suggestions-in-your-ide-with-github-copilot?tool=visualstudio)

---

## Neovim Setup

For terminal enthusiasts, Copilot works with Neovim via a Lua plugin.

### Prerequisites

- Neovim 0.6+ (0.9+ recommended)
- Node.js 18+
- Git

### Installation with lazy.nvim

```lua
{
  "github/copilot.vim",
  config = function()
    vim.g.copilot_no_tab_map = true
    vim.api.nvim_set_keymap("i", "<C-J>", 'copilot#Accept("<CR>")', { silent = true, expr = true })
  end,
}

```

### Authentication

```vim
:Copilot auth

```

Follow the browser prompts to authenticate.

> **📚 Learn More:** [Neovim Copilot Plugin](https://github.com/github/copilot.vim)

---

## Organization Settings (Business/Enterprise)

For administrators managing Copilot across an organization:

### Enabling Copilot for Your Organization

1. Go to **Organization Settings** → **Copilot**
2. Choose a subscription plan
3. Configure seat assignments (all members or specific teams)
4. Set policies for your organization

### Policy Configuration

| Policy | Options | Recommendation |
|--------|---------|----------------|
| **Suggestions matching public code** | Allow / Block | Start with Allow, review metrics |
| **Copilot Chat** | Enable / Disable | Enable for productivity |
| **Copilot in CLI** | Enable / Disable | Enable for DevOps teams |

### Content Exclusions

Block Copilot from accessing specific repositories or paths:

```yaml
# .github/copilot-policy.yml
content_exclusion:
  - "secrets/**"
  - "*.env"
  - "internal-tools/**"

```

> **📚 Learn More:** [Managing Copilot Policies](https://docs.github.com/en/copilot/managing-copilot/managing-copilot-for-your-enterprise/managing-policies-and-features-for-copilot-in-your-enterprise)

---

## Troubleshooting Common Issues

### Copilot Not Showing Suggestions

| Symptom | Possible Cause | Solution |
|---------|---------------|----------|
| No ghost text | Extension disabled | Check extension status |
| "Not authorized" | Auth expired | Re-authenticate |
| Works in some files | Language disabled | Check per-language settings |
| Slow suggestions | Network issues | Check proxy/firewall settings |

### Authentication Issues

1. Sign out completely: **Command Palette** → "GitHub Copilot: Sign Out"
2. Clear cached credentials
3. Re-authenticate with fresh browser session

### Performance Optimization

- Disable Copilot for file types you don't need suggestions in
- Reduce the number of open tabs for faster context processing
- Check for conflicting extensions

---

## Keyboard Shortcuts Reference

### VS Code

| Action | Windows/Linux | macOS |
|--------|---------------|-------|
| Accept suggestion | `Tab` | `Tab` |
| Dismiss suggestion | `Esc` | `Esc` |
| Next suggestion | `Alt+]` | `Option+]` |
| Previous suggestion | `Alt+[` | `Option+[` |
| Open Copilot Chat | `Ctrl+I` | `Cmd+I` |
| Inline Chat | `Ctrl+Shift+I` | `Cmd+Shift+I` |

### JetBrains

| Action | Windows/Linux | macOS |
|--------|---------------|-------|
| Accept suggestion | `Tab` | `Tab` |
| Next suggestion | `Alt+]` | `Option+]` |
| Previous suggestion | `Alt+[` | `Option+[` |
| Show suggestions | `Alt+\` | `Option+\` |

---

## Summary

You've learned how to:

- ✅ Install Copilot in VS Code, JetBrains, Visual Studio, and Neovim
- ✅ Configure settings for optimal performance
- ✅ Manage organization policies (Business/Enterprise)
- ✅ Troubleshoot common issues
- ✅ Use keyboard shortcuts efficiently

---

## Next Steps

Now that Copilot is set up, let's learn how to craft effective prompts for better suggestions.
