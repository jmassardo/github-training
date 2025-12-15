---
layout: training-module
title: "Copilot Agents"
permalink: /advanced/module-14/agents/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 2
total_sections: 10
phase: advanced
estimated_time: "35 min"
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
  url: /advanced/module-14/overview/
  title: "Context & Overview"
next_section:
  url: /advanced/module-14/mcp/
  title: "Model Context Protocol"
---

## What Are Copilot Agents?

Copilot Agents represent a fundamental shift from AI assistance to AI collaboration. While Copilot Chat answers questions and suggests code, Agents can autonomously plan and execute multi-step tasks.

### Agent vs. Assistant

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Assistant["Assistant Mode (Chat)"]
    direction TB
    A1["👤 User: 'How do I add tests for this function?'"]
    A2["🤖 Copilot: Explains testing approach, shows example"]
    A3["👤 User: Manually implements tests"]
    A1 --> A2 --> A3
  end
  
  subgraph Agent["Agent Mode"]
    direction TB
    B1["👤 User: 'Add comprehensive tests for the auth module'"]
    B2["🤖 Agent: Analyzes auth module"]
    B3["🤖 Identifies functions needing tests"]
    B4["🤖 Creates test file"]
    B5["🤖 Writes unit & integration tests"]
    B6["🤖 Runs tests to verify"]
    B7["🤖 Creates PR with changes"]
    B1 --> B2 --> B3 --> B4 --> B5 --> B6 --> B7
  end
</div>
</div>

### Agent Capabilities

| Capability | Description |
|-----------|-------------|
| **Planning** | Break complex tasks into steps |
| **Tool Use** | Execute terminal commands, read/write files |
| **Iteration** | Run, observe results, adjust approach |
| **Context Gathering** | Search codebase, understand architecture |
| **Execution** | Make actual changes to code |
| **Validation** | Run tests, verify changes work |

## Agent Mode in VS Code

### Activating Agent Mode

In VS Code with GitHub Copilot, agent mode is activated through specific chat interactions:

```
# Using @workspace with complex tasks
@workspace Add authentication to the Express API using JWT

# The agent will:
# 1. Analyze the current API structure
# 2. Identify where auth should be added
# 3. Create auth middleware
# 4. Update routes
# 5. Add configuration

```

### Agent-Specific Commands

| Command | Purpose |
|---------|---------|
| `@workspace` | Full workspace context for complex tasks |
| `/new` | Create new projects with scaffolding |
| `/fix` | Diagnose and fix issues autonomously |
| `/tests` | Generate comprehensive test suites |

### Example: Building a Feature

**User prompt:**

```
@workspace Create a user profile feature including:
- Profile model with avatar, bio, and preferences
- REST API endpoints for CRUD operations
- Unit tests for the service layer
- API documentation in OpenAPI format

```

**Agent execution:**

```
Agent is working...

Step 1: Analyzing existing project structure
  └── Found: Express app, MongoDB, existing User model

Step 2: Creating profile model
  └── Created: src/models/profile.js

Step 3: Creating profile service
  └── Created: src/services/profileService.js

Step 4: Creating API routes
  └── Created: src/routes/profileRoutes.js
  └── Updated: src/routes/index.js

Step 5: Creating tests
  └── Created: tests/services/profileService.test.js
  └── Created: tests/routes/profileRoutes.test.js

Step 6: Creating OpenAPI documentation
  └── Updated: docs/openapi.yaml

Step 7: Running tests
  └── All 12 tests passing

[View Changes] [Accept All] [Review in PR]

```

## Copilot Coding Agent (GitHub.com)

The Copilot Coding Agent works directly in GitHub, operating on issues and pull requests.

### How It Works

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Workflow["Copilot Coding Agent Workflow"]
    S1["1️⃣ ASSIGN<br/>User assigns Copilot to an issue"]
    S2["2️⃣ ANALYZE<br/>Agent reads issue, explores codebase"]
    S3["3️⃣ PLAN<br/>Creates implementation plan, posts comment"]
    S4["4️⃣ IMPLEMENT<br/>Creates branch, makes changes"]
    S5["5️⃣ VALIDATE<br/>Runs tests, checks for errors"]
    S6["6️⃣ DELIVER<br/>Opens PR with changes, requests review"]
    
    S1 --> S2 --> S3 --> S4 --> S5 --> S6
  end
</div>
</div>

### Assigning Copilot to Issues

You can assign Copilot to issues through:

1. **Issue comment**: Type `@github-copilot` in a comment
2. **Assignee field**: Select Copilot as an assignee
3. **CLI**: `gh issue assign 123 --assignee @github-copilot`

### Good Issues for Copilot

**Ideal characteristics:**
- Clear, specific requirements
- Well-defined scope
- Good test coverage exists
- Standard patterns in codebase

**Examples of good issues:**

```markdown
Title: Add rate limiting to API endpoints

Description:
Add rate limiting middleware to protect API endpoints:
- Limit: 100 requests per minute per IP
- Use Redis for distributed counting
- Return 429 status when exceeded
- Add X-RateLimit headers to responses

Acceptance criteria:
- [ ] Rate limiter middleware created
- [ ] Applied to all /api routes
- [ ] Unit tests added
- [ ] Documentation updated

```

**Issues to avoid:**
- Vague requirements ("Make it better")
- Massive scope ("Rewrite the entire app")
- No clear success criteria
- Requires external context not in repo

## Copilot Workspace

Copilot Workspace is a web-based agentic development environment that transforms GitHub Issues into implementation plans and pull requests.

### What is Copilot Workspace?

Unlike IDE-based agents, Copilot Workspace operates entirely in your browser on GitHub, providing a dedicated environment for planning and implementing changes from issues.

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph Workspace["Copilot Workspace Flow"]
    I["📋 Issue<br/>Requirements"] --> S["📖 Spec<br/>AI generates specification"]
    S --> P["📝 Plan<br/>File changes outlined"]
    P --> C["💻 Code<br/>Implementation generated"]
    C --> PR["🔀 PR<br/>Ready for review"]
  end
</div>
</div>

### Workspace vs. Coding Agent

| Feature | Copilot Workspace | Coding Agent |
|---------|-------------------|---------------|
| **Interface** | Dedicated web UI | GitHub Issues/PRs |
| **Process** | Interactive 4-step flow | Autonomous execution |
| **Human involvement** | Review each step | Review final PR |
| **Best for** | Complex planning | Well-defined tasks |
| **Control** | Edit spec, plan, code | Provide clear issue |

### Using Copilot Workspace

**Step 1: Open from Issue**

From any GitHub Issue, click the "Open in Workspace" button (or navigate to `copilot-workspace.githubnext.com`).

**Step 2: Review the Specification**

Workspace generates a natural language specification:

```markdown
## Specification

The task is to add user profile avatars to the application.

### Requirements:
- Users can upload avatar images (JPEG, PNG, max 5MB)
- Avatars are stored in cloud storage with CDN delivery
- Default avatar generated from user initials
- Avatar displayed in header, comments, and profile page

### Technical approach:
- Use existing S3 integration for storage
- Add ImageMagick for resizing to standard dimensions
- Update User model with avatarUrl field
```

You can edit this specification before proceeding.

**Step 3: Review the Plan**

Workspace creates a file-by-file implementation plan:

```
📁 Files to modify:

├── src/models/user.js
│   └── Add avatarUrl field with default value
│
├── src/services/avatarService.js (new)
│   └── Upload, resize, generate default avatar
│
├── src/routes/users.js
│   └── Add POST /users/:id/avatar endpoint
│
├── src/components/Avatar.jsx (new)
│   └── Reusable avatar component
│
└── tests/avatarService.test.js (new)
    └── Unit tests for avatar operations
```

You can add, remove, or modify planned changes.

**Step 4: Implement and Create PR**

Workspace generates the code. You can:
- Review each file's changes
- Edit generated code directly
- Run in integrated Codespace for testing
- Create PR when satisfied

### When to Use Workspace

**Ideal scenarios:**
- Complex features needing planning
- Unfamiliar codebases
- When you want to guide the approach
- Learning how to implement something

**Example workflow:**

```
1. Create detailed issue describing feature
2. Open in Workspace
3. Review and refine the specification
4. Adjust the plan (add tests, remove scope)
5. Review generated code
6. Test in Codespace
7. Create PR for team review
```

> **📚 Learn More:** [Copilot Workspace](https://githubnext.com/projects/copilot-workspace)

---

## Agent Best Practices

### Writing Effective Agent Prompts

**Be specific about scope:**

```
# ❌ Too vague
"Improve the authentication"

# ✅ Specific scope
"Add refresh token support to the JWT authentication:
 - Create refresh token endpoint
 - Store refresh tokens in Redis with 7-day expiry
 - Add token rotation on refresh
 - Update auth middleware to validate both token types"

```

**Provide context:**

```
# ❌ Missing context
"Add caching"

# ✅ With context
"Add Redis caching to the product catalog API:
 - Cache GET /products for 5 minutes
 - Invalidate cache on POST/PUT/DELETE
 - Use existing Redis connection from src/config/redis.js
 - Follow caching patterns from userService.js"

```

**Define success criteria:**

```
# ❌ No criteria
"Write tests"

# ✅ With criteria
"Add tests for the payment service:
 - Unit tests for calculateTotal, applyDiscount, processPayment
 - Integration test for full checkout flow
 - Mock Stripe API calls
 - Target 80% code coverage"

```

### Reviewing Agent Output

Always review agent-generated changes:

1. **Verify logic**: Does the implementation match requirements?
2. **Check patterns**: Does it follow project conventions?
3. **Review tests**: Are edge cases covered?
4. **Security scan**: Any vulnerabilities introduced?
5. **Performance**: Any inefficient patterns?

### When NOT to Use Agents

| Situation | Why | Alternative |
|-----------|-----|-------------|
| Security-critical code | Needs human expertise | Human + Chat assist |
| Novel algorithms | Agent follows patterns | Human design first |
| Architectural decisions | High-level context needed | Human planning |
| Vague requirements | Agents need clarity | Clarify first |
| Breaking changes | Risk assessment needed | Human oversight |

## Agent Limitations

### Current Constraints

| Limitation | Description |
|-----------|-------------|
| **Context window** | Large codebases may exceed limits |
| **External services** | Can't authenticate to external APIs |
| **Complex debugging** | Multi-system issues challenging |
| **Business logic** | May miss domain-specific nuances |
| **Creativity** | Better at patterns than novel solutions |

### Managing Expectations

**Agents excel at:**
- Implementing well-defined features
- Adding tests to existing code
- Refactoring with clear goals
- Bug fixes with reproducible steps
- Documentation generation

**Agents struggle with:**
- Ambiguous requirements
- Cross-system integration
- Performance optimization
- Security hardening
- Architecture decisions

## Practical Applications

### Use Case 1: Bug Fixing

```markdown
Issue: Login fails when email contains '+'

Steps to reproduce:
1. Try to register with email "test+label@example.com"
2. Receive validation error

Expected: Email should be accepted
Actual: "Invalid email format" error

Relevant code: src/validators/email.js

```

### Use Case 2: Feature Implementation

```markdown
Feature: Add dark mode support

Requirements:
- Detect system preference
- Add manual toggle in settings
- Persist preference in localStorage
- Apply to all UI components

Design: Follow existing theming pattern in src/styles/theme.js

```

### Use Case 3: Test Generation

```markdown
Task: Add tests for OrderService

Current coverage: 45%
Target coverage: 80%

Focus areas:
- createOrder validation
- calculateShipping logic
- applyPromoCode edge cases

Mocking: Use existing mocks in tests/__mocks__

```

---

**Next:** Learn how to extend Copilot's capabilities with Model Context Protocol.
