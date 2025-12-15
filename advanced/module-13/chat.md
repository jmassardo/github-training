---
layout: training-module
title: "Copilot Chat"
permalink: /advanced/module-13/chat/
module_number: 13
module_title: "GitHub Copilot Fundamentals"
section_number: 5
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
  url: /advanced/module-13/prompts/
  title: "Prompt Crafting"
next_section:
  url: /advanced/module-13/ide-differences/
  title: "IDE Differences"
---

## What is Copilot Chat?

Copilot Chat is a conversational AI interface integrated into your IDE. Unlike inline completions, Chat allows you to have interactive dialogues about code—asking questions, requesting explanations, and getting help with complex tasks.

### Chat vs. Completions

| Feature | Inline Completions | Copilot Chat |
|---------|-------------------|--------------|
| **Trigger** | Automatic as you type | Explicit command/shortcut |
| **Output** | Code suggestions | Conversational responses + code |
| **Context** | Current file + open tabs | Selected code + conversation history |
| **Use case** | Writing new code | Understanding, debugging, refactoring |

> **📚 Learn More:** [GitHub Copilot Chat Documentation](https://docs.github.com/en/copilot/github-copilot-chat/using-github-copilot-chat-in-your-ide)

---

## Opening Copilot Chat

### VS Code

| Method | Shortcut | Description |
|--------|----------|-------------|
| Chat Panel | `Ctrl+Alt+I` / `Cmd+Alt+I` | Opens side panel |
| Inline Chat | `Ctrl+I` / `Cmd+I` | Opens at cursor position |
| Quick Chat | `Ctrl+Shift+I` / `Cmd+Shift+I` | Floating dialog |

### JetBrains

| Method | Shortcut | Description |
|--------|----------|-------------|
| Chat Panel | `Alt+C` | Opens tool window |
| Inline Chat | `Ctrl+Shift+G` | Opens at cursor |

---

## Chat Participants (Slash Commands)

Copilot Chat supports special participants that provide focused functionality:

### @workspace

Query your entire workspace for context-aware answers:

```
@workspace How is authentication implemented in this project?

@workspace Find all API endpoints that don't have rate limiting

@workspace What dependencies does this project use for testing?

```

### @vscode

Get help with VS Code itself:

```
@vscode How do I enable word wrap?

@vscode What's the shortcut to duplicate a line?

@vscode How do I configure auto-save?

```

### @terminal

Terminal and shell assistance:

```
@terminal How do I find all files modified in the last 24 hours?

@terminal What's the command to see disk usage by folder?

```

> **📚 Learn More:** [Chat Participants](https://docs.github.com/en/copilot/github-copilot-chat/copilot-chat-in-ides/using-github-copilot-chat-in-your-ide#using-chat-participants)

---

## Slash Commands

Slash commands provide quick access to common tasks:

| Command | Purpose | Example |
|---------|---------|---------|
| `/explain` | Explain selected code | `/explain what does this regex do` |
| `/tests` | Generate unit tests | `/tests for this function` |
| `/fix` | Fix bugs in selection | `/fix this null pointer error` |
| `/doc` | Generate documentation | `/doc add JSDoc comments` |
| `/simplify` | Simplify complex code | `/simplify this nested logic` |
| `/new` | Create new code | `/new React component for user profile` |

### Using Slash Commands Effectively

```
# Explain complex code
/explain 
[Select confusing code block]

# Generate tests with specific framework
/tests using pytest with parametrized inputs

# Fix with specific constraint
/fix without changing the public API

```

---

## Effective Chat Patterns

### Code Explanation

**Ask specific questions:**

```
What is the time complexity of this function?

Why does this use a WeakMap instead of a regular Map?

What edge cases might this code fail to handle?

```

### Debugging Assistance

**Describe the problem:**

```
This function returns undefined when the input array is empty. 
I expected it to return an empty array. What's wrong?

I'm getting a "maximum call stack exceeded" error. 
Where is the infinite recursion?

```

### Code Generation

**Be specific about requirements:**

```
Create a React hook that:
- Fetches data from an API endpoint
- Handles loading, error, and success states
- Supports cancellation when component unmounts
- Includes TypeScript types

```

### Refactoring

**State your goals:**

```
Refactor this to:
- Use early returns instead of nested if statements
- Extract the validation logic to a separate function
- Add error handling for network failures

```

---

## Context in Chat

### Selecting Code

Always select relevant code before asking questions:

1. Highlight code in your editor
2. Open Copilot Chat
3. The selection is automatically included as context

### Using #file and #selection

Reference specific files or selections in your prompts:

```
#file:package.json What version of React is this project using?

#selection Explain what this code does

Compare #file:old-implementation.js with #file:new-implementation.js

```

### Conversation Memory

Chat maintains context within a conversation:

```
You: Explain this sorting algorithm
Copilot: [Explanation of quicksort]

You: Now optimize it for nearly-sorted arrays
Copilot: [References previous context to provide relevant optimization]

You: Add unit tests for both versions
Copilot: [Creates tests for original and optimized versions]

```

---

## Advanced Chat Features

### Multi-Turn Conversations

Build on previous responses:

```
1. "Create a user authentication service"
2. "Add password hashing with bcrypt"
3. "Include JWT token generation"
4. "Add rate limiting for login attempts"
5. "Write integration tests for the complete flow"

```

### Code Review Simulation

```
Review this code for:
- Security vulnerabilities
- Performance issues
- Code style consistency
- Missing error handling

```

### Architecture Discussions

```
I need to design a system that:
- Processes 10,000 events per second
- Stores events for 30 days
- Allows real-time queries

What architecture would you recommend using AWS services?

```

---

## Inline Chat

Inline Chat lets you make targeted changes without leaving your code:

### When to Use Inline Chat

- Quick fixes and small modifications
- Adding documentation to existing code
- Simple refactoring tasks
- Generating test cases for specific functions

### Inline Chat Workflow

1. Position cursor or select code
2. Press `Ctrl+I` / `Cmd+I`
3. Type your request
4. Review the diff
5. Accept or reject changes

### Example Inline Requests

```
Add error handling for null input

Convert this to async/await

Add TypeScript types

Extract this to a helper function

```

---

## Chat Best Practices

### DO

- ✅ Provide specific, detailed context
- ✅ Use code selection when relevant
- ✅ Build on conversation history
- ✅ Review generated code carefully
- ✅ Use slash commands for common tasks

### DON'T

- ❌ Expect Chat to know your entire codebase (use @workspace for that)
- ❌ Accept code without understanding it
- ❌ Use Chat for simple completions (inline is faster)
- ❌ Ask multiple unrelated questions in one message

---

## Chat vs. Web Search

| Need | Use Chat | Use Web Search |
|------|----------|----------------|
| Code-specific questions | ✅ | |
| Your codebase questions | ✅ (@workspace) | |
| Latest API changes | | ✅ |
| Library comparisons | Sometimes | ✅ |
| General programming concepts | ✅ | |
| Current events/news | | ✅ |

---

## Summary

| Feature | Purpose | Trigger |
|---------|---------|---------|
| **Chat Panel** | Extended conversations | `Ctrl+Alt+I` |
| **Inline Chat** | Quick edits in place | `Ctrl+I` |
| **@workspace** | Codebase-aware queries | `@workspace` prefix |
| **Slash commands** | Common tasks | `/explain`, `/tests`, etc. |

---

## Next Steps

Now let's explore how Copilot features differ across various IDEs.
