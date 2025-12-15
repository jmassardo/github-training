---
layout: training-module
title: "Knowledge Check"
permalink: /advanced/module-13/quiz/
module_number: 13
module_title: "GitHub Copilot Fundamentals"
section_number: 9
total_sections: 10
phase: advanced
estimated_time: "15 min"
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
  url: /advanced/module-13/labs/
  title: "Hands-On Labs"
next_section:
  url: /advanced/module-13/resources/
  title: "Resources"
---

## Test Your Knowledge

This quiz covers key concepts from Module 13: GitHub Copilot Fundamentals. Try to answer each question before revealing the answer.

---

### Question 1: LLM Basics

**What is the primary mechanism by which Large Language Models (LLMs) like those powering Copilot generate code?**

A) Searching a database of stored code snippets  
B) Predicting the most likely next tokens based on patterns learned during training  
C) Compiling and executing code to determine correctness  
D) Copying exact matches from public repositories  

<details>
<summary>Show Answer</summary>

**B) Predicting the most likely next tokens based on patterns learned during training**

LLMs are trained on vast amounts of text and code to learn statistical patterns. They generate new content by predicting what tokens are most likely to come next given the context. They don't store or retrieve exact code—they generate new code based on learned patterns.

</details>

---

### Question 2: Context Understanding

**Which of the following provides the HIGHEST priority context for Copilot suggestions?**

A) Open tabs in your IDE  
B) Your entire Git repository history  
C) The current file and cursor position  
D) Comments from other team members  

<details>
<summary>Show Answer</summary>

**C) The current file and cursor position**

The current file and cursor position provide the highest priority context. Open tabs provide additional context but at a lower priority. Copilot does not have access to your entire Git history or comments from other systems.

</details>

---

### Question 3: Privacy Features

**A customer using Copilot Business asks: "Is our code used to train the AI model?" What is the correct answer?**

A) Yes, all code is used to improve the model  
B) No, Copilot Business does not use customer code for training  
C) Only public code is used for training  
D) It depends on the organization's settings  

<details>
<summary>Show Answer</summary>

**B) No, Copilot Business does not use customer code for training**

Copilot Business and Enterprise plans do not use customer code for training. Code is processed for generating suggestions but is not retained or used to improve the underlying models. This is a key differentiator for enterprise customers with privacy concerns.

</details>

---

### Question 4: Prompt Crafting

**Which prompt is most likely to produce a good Copilot suggestion?**

A) `def process(data):`  
B) `# do stuff with users`  
C) `def validate_email_format(email: str) -> bool:`  
D) `def fn(x):`  

<details>
<summary>Show Answer</summary>

**C) `def validate_email_format(email: str) -> bool:`**

This prompt provides:

- A descriptive function name that explains the purpose
- Type hints that clarify input and output
- Clear semantic meaning

The other options are too vague for Copilot to understand what you want.

</details>

---

### Question 5: Copilot Chat

**What does the `/explain` slash command do in Copilot Chat?**

A) Explains how to use Copilot  
B) Provides an explanation of selected code  
C) Exports code to documentation  
D) Expands abbreviated code  

<details>
<summary>Show Answer</summary>

**B) Provides an explanation of selected code**

The `/explain` command asks Copilot Chat to analyze and explain the selected code, including what it does, how it works, and any important details about its implementation.

</details>

---

### Question 6: IDE Differences

**Which IDE offers the most complete Copilot feature set, including Copilot Edits and @workspace?**

A) Visual Studio  
B) JetBrains IntelliJ  
C) VS Code  
D) Neovim  

<details>
<summary>Show Answer</summary>

**C) VS Code**

VS Code is the primary development platform for Copilot and offers the most complete feature set, including Copilot Edits, @workspace queries, voice control, and terminal integration. Other IDEs have excellent support but may not have all features.

</details>

---

### Question 7: Security Best Practices

**What should you ALWAYS do before accepting a Copilot suggestion for code that handles user authentication?**

A) Accept it immediately since AI code is always secure  
B) Review the code carefully for security vulnerabilities  
C) Ask Copilot if the code is secure  
D) Run it in production to test  

<details>
<summary>Show Answer</summary>

**B) Review the code carefully for security vulnerabilities**

Security-critical code requires extra scrutiny. Copilot can generate code with vulnerabilities like SQL injection, hardcoded secrets, or insecure defaults. Always review security-sensitive code manually and consider having a security-focused code review.

</details>

---

### Question 8: Keyboard Shortcuts

**In VS Code, what keyboard shortcut opens Copilot inline chat at your cursor position?**

A) `Ctrl+I` / `Cmd+I`  
B) `Ctrl+Alt+I` / `Cmd+Alt+I`  
C) `Tab`  
D) `Alt+]`  

<details>
<summary>Show Answer</summary>

**A) `Ctrl+I` / `Cmd+I`**

- `Ctrl+I` / `Cmd+I` opens inline chat
- `Ctrl+Alt+I` / `Cmd+Alt+I` opens the chat panel
- `Tab` accepts a suggestion
- `Alt+]` cycles to the next suggestion

</details>

---

### Question 9: Context Management

**A developer complains that Copilot suggestions are irrelevant. Which action is LEAST likely to help?**

A) Opening related files in IDE tabs  
B) Adding descriptive comments before the code  
C) Using more meaningful variable names  
D) Increasing the temperature setting  

<details>
<summary>Show Answer</summary>

**D) Increasing the temperature setting**

Temperature controls randomness in LLM outputs—higher temperature means more creative/random responses, which would likely make suggestions less relevant, not more. The other options (opening related files, adding comments, using meaningful names) all improve context quality.

</details>

---

### Question 10: Customer Conversations

**A customer asks: "Will Copilot replace our developers?" What's the best response?**

A) "Yes, eventually AI will write all code"  
B) "Copilot augments developers by handling routine tasks, letting them focus on creative problem-solving"  
C) "No, AI can't write any useful code"  
D) "Only junior developers will be affected"  

<details>
<summary>Show Answer</summary>

**B) "Copilot augments developers by handling routine tasks, letting them focus on creative problem-solving"**

This response accurately frames Copilot as a productivity tool that enhances developers rather than replacing them. It emphasizes the human-AI partnership where Copilot handles boilerplate and routine tasks while developers focus on design, architecture, and complex problem-solving.

</details>

---

## Scoring

| Score | Assessment |
|-------|------------|
| 9-10 | Excellent! Ready for Module 14 |
| 7-8 | Good understanding, review missed topics |
| 5-6 | Review the concepts section again |
| Below 5 | Re-read the module before proceeding |

---

## Key Takeaways

If you missed any questions, review these key points:

1. **LLMs generate code through pattern prediction**, not retrieval
2. **Current file and cursor** provide the highest priority context
3. **Business/Enterprise plans** don't use customer code for training
4. **Descriptive names and type hints** improve suggestion quality
5. **Always review security-critical code** before accepting
6. **VS Code** has the most complete feature set
7. **Copilot augments**, doesn't replace, developers

---

## Next Steps

Review the resources section for additional learning materials and documentation links.
