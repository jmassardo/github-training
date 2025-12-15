---
layout: training-module
title: "Knowledge Check"
permalink: /advanced/module-14/quiz/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 9
total_sections: 10
phase: advanced
estimated_time: "15 min"
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
toc: true
prev_section:
  url: /advanced/module-14/labs/
  title: "Hands-On Labs"
next_section:
  url: /advanced/module-14/resources/
  title: "Resources"
---

## Test Your Understanding

Answer the following questions to validate your knowledge of GitHub Copilot advanced features. Select the best answer for each question.

---

### Question 1: Copilot Agents

**What is the primary function of the GitHub Copilot coding agent?**

A) Providing inline code completions as you type  
B) Answering questions about code in the chat interface  
C) Autonomously implementing features from issues with minimal guidance  
D) Managing GitHub repository settings and permissions  

<details>
<summary>Show Answer</summary>

**C) Autonomously implementing features from issues with minimal guidance**

The coding agent can independently analyze issues, plan implementations, write code, run tests, and create pull requests without requiring constant user guidance.

</details>

---

### Question 2: MCP Architecture

**What does MCP stand for and what is its primary purpose?**

A) Model Completion Protocol - speeds up code suggestions  
B) Model Context Protocol - connects Copilot to external tools and data sources  
C) Managed Copilot Platform - enterprise administration tools  
D) Multi-Cloud Provider - deployment automation for cloud services  

<details>
<summary>Show Answer</summary>

**B) Model Context Protocol - connects Copilot to external tools and data sources**

MCP is an open standard that enables AI assistants to connect with external data sources and tools through a unified interface, extending Copilot's capabilities.

</details>

---

### Question 3: MCP Capabilities

**Which three capability types do MCP servers provide?**

A) Resources, Tools, and Prompts  
B) Files, Databases, and APIs  
C) Chat, Completions, and Search  
D) Users, Teams, and Permissions  

<details>
<summary>Show Answer</summary>

**A) Resources, Tools, and Prompts**

MCP servers provide three types of capabilities:

- **Resources**: Static data that can be read
- **Tools**: Actions that can be performed  
- **Prompts**: Pre-defined prompt templates

</details>

---

### Question 4: Copilot Extensions

**How do users invoke a Copilot Extension in chat?**

A) By enabling it in VS Code settings first  
B) By using the @mention syntax (e.g., @extension-name)  
C) By right-clicking and selecting from context menu  
D) Extensions are invoked automatically based on context  

<details>
<summary>Show Answer</summary>

**B) By using the @mention syntax (e.g., @extension-name)**

Copilot Extensions are invoked through the @ mention system in chat, allowing users to direct questions or requests to specific extensions like @workspace or @docker.

</details>

---

### Question 5: Extensions vs MCP

**What is a key difference between Copilot Extensions and MCP servers?**

A) Extensions are free, MCP servers require additional payment  
B) Extensions run locally, MCP servers run in the cloud  
C) Extensions are chat-based with streaming responses, MCP servers are tool-based with structured data  
D) Extensions only work in VS Code, MCP servers work in all IDEs  

<details>
<summary>Show Answer</summary>

**C) Extensions are chat-based with streaming responses, MCP servers are tool-based with structured data**

Extensions provide interactive chat experiences with streaming text, while MCP servers provide tools that agents can use to retrieve structured data or perform actions.

</details>

---

### Question 6: Custom Instructions Location

**Where should repository-level Copilot instructions be placed?**

A) /copilot/instructions.md  
B) /.github/copilot-instructions.md  
C) /docs/copilot.md  
D) /.vscode/copilot.json  

<details>
<summary>Show Answer</summary>

**B) /.github/copilot-instructions.md**

Repository-level instructions are placed in `.github/copilot-instructions.md` and apply to everyone working in that repository.

</details>

---

### Question 7: Instruction Scoping

**How can you make instructions apply only to specific file patterns?**

A) Use the applyTo front matter property in instruction files  
B) Create separate repositories for different file types  
C) Instructions always apply globally to all files  
D) Use environment variables to filter by file type  

<details>
<summary>Show Answer</summary>

**A) Use the applyTo front matter property in instruction files**

The `applyTo` property uses glob patterns to scope instructions to specific files:

```yaml
---
applyTo: "**/api/**/*.ts"
---

```

</details>

---

### Question 8: Seat Assignment Methods

**Which of the following is NOT a valid method for assigning Copilot seats?**

A) All members  
B) Selected members  
C) Selected teams  
D) Selected repositories  

<details>
<summary>Show Answer</summary>

**D) Selected repositories**

Copilot seats can be assigned to:

- All members (everyone in the organization)
- Selected members (specific individual users)
- Selected teams (team-based allocation)

Seats are assigned to **users**, not repositories.

</details>

---

### Question 9: Content Exclusions

**What is the purpose of content exclusions in Copilot enterprise settings?**

A) Exclude certain users from receiving Copilot access  
B) Prevent Copilot from reading or suggesting code from specified paths  
C) Exclude specific programming languages from suggestions  
D) Block Copilot suggestions in certain IDE applications  

<details>
<summary>Show Answer</summary>

**B) Prevent Copilot from reading or suggesting code from specified paths**

Content exclusions prevent Copilot from accessing specified file paths, protecting sensitive code like secrets directories, proprietary algorithms, or configuration files.

</details>

---

### Question 10: Acceptance Rate Definition

**What does the "acceptance rate" metric measure?**

A) Percentage of users who accepted the Copilot license agreement  
B) Percentage of shown suggestions that users accepted  
C) Percentage of generated code that passed automated tests  
D) Percentage of pull requests approved on first review  

<details>
<summary>Show Answer</summary>

**B) Percentage of shown suggestions that users accepted**

Acceptance Rate = (Accepted Suggestions / Total Suggestions Shown) × 100

This metric indicates how useful and relevant Copilot's suggestions are to users.

</details>

---

### Question 11: ROI Measurement

**Which approach is most effective for measuring Copilot ROI?**

A) Only count lines of code generated by Copilot  
B) Combine quantitative metrics with qualitative data like surveys and time savings  
C) Only rely on developer satisfaction surveys  
D) Compare number of commits before and after Copilot  

<details>
<summary>Show Answer</summary>

**B) Combine quantitative metrics with qualitative data like surveys and time savings**

Effective ROI measurement combines:

- Usage metrics (from API data)
- Quality metrics (acceptance rate, code retention)
- Impact metrics (surveys, productivity indicators, time savings)

</details>

---

### Question 12: Audit Events

**Which Copilot audit event type indicates a user was given access?**

A) copilot.access_granted  
B) copilot.seat_assigned  
C) copilot.user_enabled  
D) copilot.license_added  

<details>
<summary>Show Answer</summary>

**B) copilot.seat_assigned**

The `copilot.seat_assigned` audit event is logged when a user is given Copilot access through seat assignment.

</details>

---

### Question 13: MCP Security

**What security practice should MCP servers implement for tool inputs?**

A) Trust all inputs that come from Copilot  
B) Validate and sanitize all inputs before processing  
C) Only accept inputs from users in specific roles  
D) Encrypt all inputs before processing  

<details>
<summary>Show Answer</summary>

**B) Validate and sanitize all inputs before processing**

MCP servers should validate all inputs to prevent injection attacks and ensure data integrity. Use schema validation libraries like Zod for TypeScript.

</details>

---

### Question 14: Extension Security

**How should Copilot Extensions verify incoming requests?**

A) Check the user's email domain matches the organization  
B) Verify the request signature using GitHub's public key  
C) Trust all requests originating from github.com  
D) Require an API key in request headers  

<details>
<summary>Show Answer</summary>

**B) Verify the request signature using GitHub's public key**

Extensions should verify request authenticity using the `github-public-key-signature` and `github-public-key-identifier` headers provided with each request.

</details>

---

### Question 15: Knowledge Bases

**What type of content is best suited for Copilot Enterprise knowledge bases?**

A) Customer personal data and PII  
B) Architecture documentation, coding standards, and API specifications  
C) Third-party proprietary source code  
D) Encrypted configuration files and secrets  

<details>
<summary>Show Answer</summary>

**B) Architecture documentation, coding standards, and API specifications**

Knowledge bases work best with:

- Architecture documentation
- API specifications
- Coding standards
- Runbooks and playbooks
- Decision records (ADRs)
- README files

</details>

---

## Scoring Guide

| Score | Rating | Recommendation |
|-------|--------|----------------|
| 13-15 | Expert | Ready to lead Copilot initiatives |
| 10-12 | Proficient | Strong understanding, ready for advanced usage |
| 7-9 | Developing | Review specific topics before practical application |
| 0-6 | Needs Review | Revisit module content thoroughly |

---

## Next Steps Based on Your Score

**Score 13-15**: 
- Continue to [Resources](../resources/) for advanced materials
- Consider becoming a Copilot champion for your team
- Explore building custom MCP servers or extensions

**Score 10-12**: 
- Review any questions you missed
- Practice with the hands-on labs
- Continue to Resources for further learning

**Score below 10**: 
- Revisit the sections covering topics you missed
- Complete all hands-on labs
- Retake the quiz before proceeding

---

## Review Topics by Question

| Question | Topic | Section |
|----------|-------|---------|
| 1 | Coding Agent | [Agents](../agents/) |
| 2-3 | MCP Protocol | [MCP](../mcp/) |
| 4-5 | Extensions | [Extensions](../extensions/) |
| 6-7 | Custom Instructions | [Instructions](../instructions/) |
| 8-9 | Enterprise Admin | [Enterprise](../enterprise/) |
| 10-11 | Metrics | [Metrics](../metrics/) |
| 12 | Audit Logging | [Enterprise](../enterprise/) |
| 13 | MCP Security | [MCP](../mcp/) |
| 14 | Extension Security | [Extensions](../extensions/) |
| 15 | Knowledge Bases | [Instructions](../instructions/) |
