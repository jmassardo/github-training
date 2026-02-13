---
layout: training-module
title: "Custom Instructions"
permalink: /advanced/module-14/instructions/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 5
total_sections: 12
phase: advanced
estimated_time: "30 min"
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
prev_section:
  url: /advanced/module-14/extensions/
  title: "Copilot Extensions"
next_section:
  url: /advanced/module-14/spaces/
  title: "Copilot Spaces"
---

## Configuring Copilot Behavior

Custom instructions tell GitHub Copilot how to behave for your specific codebase, team, or organization. They ensure consistent, context-aware suggestions that follow your standards.

### Why Custom Instructions Matter

Without custom instructions, Copilot generates suggestions based on general programming patterns. With custom instructions, you get suggestions tailored to:

- **Your tech stack** - Framework-specific patterns
- **Your standards** - Naming conventions, error handling
- **Your architecture** - Project structure, design patterns
- **Your security requirements** - Compliance, data handling

---

## Instruction Hierarchy

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Hierarchy["Instruction Hierarchy"]
    Org["🏢 Organization Instructions<br/>(Highest Priority - Set by Admins)"]
    Repo["📁 Repository Instructions<br/>(.github/copilot-instructions.md)"]
    Workspace["💼 Workspace Instructions<br/>(.vscode/instructions/*.md)"]
    User["👤 User Instructions<br/>(VS Code settings / preferences)"]
    
    Org --> Repo --> Workspace --> User
  end
</div>
</div>

Instructions cascade from organization level down to user level, with higher levels taking precedence for conflicting guidance.

---

## Repository Instructions

Repository instructions live in `.github/copilot-instructions.md` and apply to everyone working in that repository.

### Example: Full-Stack Application

```markdown
# Copilot Instructions

## Project Overview
This is a Node.js/React full-stack application using:

- Backend: Express.js with TypeScript
- Frontend: React with TypeScript
- Database: PostgreSQL with Prisma ORM
- Testing: Jest and React Testing Library

## Code Standards

### TypeScript
- Use strict mode
- Prefer interfaces over types for object shapes
- Use explicit return types on exported functions
- No `any` - use `unknown` if type is unclear

### React
- Use functional components with hooks
- Prefer named exports
- Use React.FC sparingly (prefer explicit prop types)
- Use CSS modules for styling

### API Design
- RESTful endpoints under /api/v1/
- Use proper HTTP status codes
- Always return JSON with consistent structure
- Include pagination for list endpoints

## File Naming
- Components: PascalCase (UserProfile.tsx)
- Utilities: camelCase (formatDate.ts)
- Tests: *.test.ts or *.spec.ts
- Styles: *.module.css

## Error Handling
- Use custom error classes in /src/errors/
- Log errors with structured format
- Never expose internal errors to clients
- Include error codes for client handling

## Security
- Sanitize all user inputs
- Use parameterized queries (Prisma handles this)
- Validate JWT tokens on all protected routes
- Never log sensitive data (passwords, tokens)

```

---

## Workspace Instructions

Workspace instructions provide IDE-specific guidance and can be scoped to particular files or directories.

### VS Code Configuration

```json
// .vscode/settings.json
{
  "github.copilot.chat.codeGeneration.instructions": [
    {
      "file": ".vscode/instructions/general.md"
    },
    {
      "file": ".vscode/instructions/api.md"
    },
    {
      "text": "Always add JSDoc comments to public functions"
    }
  ]
}

```

### Scoped Instructions with applyTo

Create instructions that only apply to specific file patterns:

```markdown
<!-- .vscode/instructions/api.md -->
---
applyTo: "**/api/**/*.ts"
---

# API Development Guidelines

When writing API code:

1. Use the ApiResponse wrapper for all responses
2. Include request validation with Zod
3. Add OpenAPI annotations for documentation
4. Log request/response for debugging

Example pattern:

```typescript

export const handler = async (req: Request, res: Response) => {
  const validated = RequestSchema.parse(req.body);
  const result = await service.process(validated);
  return ApiResponse.success(res, result);
};

```

```

### Frontend-Specific Instructions

```markdown
<!-- .vscode/instructions/components.md -->
---
applyTo: "**/components/**/*.tsx"
---

# React Component Guidelines

When creating React components:

1. Define Props interface at top of file
2. Use semantic HTML elements
3. Include aria attributes for accessibility
4. Add loading and error states
5. Write corresponding Storybook story

Structure:

```tsx

interface ComponentProps {
  // Props here
}

export function Component({ ...props }: ComponentProps) {
  // Component logic
  return (
    // JSX with semantic HTML
  );
}

```

```

---

## Prompt Files (.prompt.md)

Prompt files are reusable templates for common tasks that include context, tools, and instructions.

### Creating a Prompt File

```markdown
<!-- .vscode/prompts/create-component.prompt.md -->
---
mode: "agent"
tools: ["file_search", "read_file", "create_file"]
description: "Create a new React component with tests"
---

# Create React Component

Create a new React component that includes:

## Requirements
- TypeScript interfaces for all props
- CSS module for styling
- Unit tests with React Testing Library
- Storybook story for visual testing

## Reference
Use existing components in /src/components as reference for patterns.

## Steps
1. Analyze similar components for patterns
2. Create component file with proper structure
3. Create CSS module with BEM naming
4. Write comprehensive tests
5. Create Storybook story with variants

```

### Using Prompt Files

1. Open Copilot Chat panel
2. Click "Attach" → "Prompt"
3. Select your prompt file
4. Add any additional context
5. Copilot executes with full context and tools

---

## Knowledge Bases

Enterprise organizations can create knowledge bases from repositories containing documentation, standards, and examples.

### Setting Up Knowledge Bases

1. Navigate to **Organization Settings**
2. Go to **Copilot** → **Knowledge**
3. Click **New Knowledge Base**
4. Select repositories to include
5. Configure indexing options

### Best Content for Knowledge Bases

| Include | Exclude |
|---------|---------|
| Architecture documentation | Sensitive credentials |
| API specifications | Customer data |
| Coding standards | Compliance documents |
| Runbooks and playbooks | HR policies |
| Decision records | Third-party proprietary code |
| README files | Encrypted configurations |

### Knowledge Base in Action

Once configured, Copilot automatically references your knowledge base when relevant:

```
User: How should I implement authentication in this project?

Copilot: Based on your architecture documentation, this project uses JWT 
authentication with refresh token rotation. Here's how to implement it 
following your established patterns...

[References: /docs/architecture/authentication.md]

```

---

## Copilot Spaces

Copilot Spaces let you create curated collections of context—files, repositories, instructions, and notes—that ground Copilot's responses for specific tasks or projects.

### What Are Copilot Spaces?

Think of Spaces as "workspaces" for Copilot conversations. Instead of manually attaching files each time, you create a Space with all relevant context and Copilot uses it automatically.

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Space["📦 Copilot Space: 'Auth Migration'"]
    I["📝 Instructions<br/>'You are helping migrate from JWT to OAuth2'"]
    R1["📁 auth-service repo"]
    R2["📁 user-service repo"]
    D["📄 OAuth2 spec document"]
    N["📋 Migration notes"]
  end
  
  Space --> C["💬 Copilot Chat"]
  C --> A["Contextual answers using all sources"]
</div>
</div>

### Spaces vs. Other Context Methods

| Method | Scope | Persistence | Sharing | Best For |
|--------|-------|-------------|---------|----------|
| **Spaces** | Task/project | Permanent | Team/org | Cross-repo projects |
| **@workspace** | Current repo | Session | No | Single repo work |
| **Instructions** | Repo/org | Permanent | Via repo | Coding standards |
| **Knowledge Bases** | Organization | Permanent | Org-wide | Documentation |

### Creating a Space

1. Navigate to [github.com/copilot/spaces](https://github.com/copilot/spaces)
2. Click **Create space**
3. Name your space (e.g., "Q4 Platform Migration")
4. Choose ownership: Personal or Organization
5. Add a description (helps teammates understand the space's purpose)

### Adding Context to a Space

Spaces can include multiple types of content:

**Instructions** - Guide Copilot's behavior:
```
You are a SQL optimization expert. Your job is to analyze queries 
against the schemas in this space and suggest performance improvements.
Focus on index usage, query plans, and N+1 patterns.
```

**Sources:**

- **Files & Repositories** - Add entire repos or specific files/folders
- **Links** - GitHub Issues, PRs, and other content via URL
- **Uploads** - Local files (images, documents, spreadsheets)
- **Text Content** - Notes, transcripts, or any freeform text

### Quick Add from Code View

While browsing code on GitHub:

1. Open any file
2. Click the **Add to space** icon at the top
3. Select an existing space or create new one

This lets you build context without interrupting your workflow.

### Use Cases for CSMs

**Customer Onboarding Space:**

- Customer's architecture docs
- Relevant GitHub guides
- Notes from discovery calls
- Their repo structure

**Migration Project Space:**

- Source and target platform docs
- Migration runbook
- Customer's affected repositories
- Previous migration case studies

**Support Escalation Space:**

- Customer's repo with the issue
- Related GitHub Issues/PRs
- Error logs and diagnostics
- Support playbooks

### Sharing Spaces

Organization-owned Spaces can be shared with team members using GitHub's permission model:

1. Open the Space
2. Go to **Settings** → **Sharing**
3. Add team members or teams
4. Choose permission level (view/edit)

### Space Best Practices

| Do | Don't |
|----|-------|
| Create focused spaces per task/project | Create one giant "everything" space |
| Include clear instructions | Leave instructions empty |
| Update sources as project evolves | Let spaces become stale |
| Add context breadcrumbs (why included) | Add files without explanation |
| Archive completed project spaces | Delete spaces (preserve history) |

> **📚 Learn More:** [About Copilot Spaces](https://docs.github.com/en/copilot/concepts/about-organizing-and-sharing-context-with-copilot-spaces)

---

## Writing Effective Instructions

### Be Specific, Not Vague

```markdown
# ❌ Too Vague
Use good error handling.

# ✅ Specific
Wrap async operations in try-catch blocks.
Use the CustomError class from /src/errors/.
Log errors with logger.error() including stack trace.
Return appropriate HTTP status codes:

- 400 for validation errors
- 401 for authentication failures
- 403 for authorization failures
- 500 for server errors

```

### Include Examples

```markdown
# ❌ No Example
Use dependency injection for services.

# ✅ With Example
Use dependency injection for services:

```typescript

// Define interface
interface UserService {
  findById(id: string): Promise<User>;
  create(data: CreateUserDto): Promise<User>;
}

// Inject in controller constructor
class UserController {
  constructor(private readonly userService: UserService) {}
  
  async getUser(id: string) {
    return this.userService.findById(id);
  }
}

```

```

### Document Deprecated Patterns

```markdown
## Deprecated Patterns (DO NOT USE)
- Class components (use functional components)
- Redux for state (migrated to Zustand)
- Moment.js (use date-fns)
- Axios (use native fetch)

## Current Patterns
- React Query for server state
- Zustand for client state  
- Zod for validation
- date-fns for date manipulation

```

---

## Framework-Specific Examples

### Django Instructions

```markdown
# Django Project Instructions

## Architecture
- Use class-based views for CRUD operations
- Define models in models.py, not separate apps for each model
- Use Django REST Framework for API endpoints
- Write tests with pytest-django

## Patterns
- Use ViewSets for resource-based APIs
- Implement custom permissions in permissions.py
- Use serializers for all input/output transformation
- Cache expensive queries with django-redis

## Database
- Always create migrations for model changes
- Use select_related/prefetch_related to prevent N+1
- Index fields used in filters and ordering

```

### Spring Boot Instructions

```markdown
# Spring Boot Instructions

## Dependency Injection
- Use constructor injection exclusively (not @Autowired)
- Define beans in @Configuration classes
- Use @Qualifier for multiple implementations

## Data Transfer
- Define DTOs separate from entities
- Use MapStruct for entity-DTO mapping
- Validate requests with @Valid annotation

## Error Handling
- Use @ControllerAdvice for global exception handling
- Define custom exceptions extending RuntimeException
- Return consistent error response format

```

### Terraform Instructions

```markdown
# Terraform Instructions

## Module Structure
- Each module must include:
  - main.tf for resources
  - variables.tf with descriptions for all variables
  - outputs.tf for all needed outputs
  - README.md with usage examples
  - versions.tf for provider constraints

## Naming
- Use snake_case for resource names
- Prefix resources with project identifier
- Use meaningful, descriptive names

## Security
- Never hardcode secrets (use variables or data sources)
- Enable encryption at rest for storage resources
- Use least-privilege IAM policies

```

---

## Testing Your Instructions

### Validation Checklist

- [ ] Instructions are clear and actionable
- [ ] Examples match current codebase patterns
- [ ] No conflicting guidance between files
- [ ] Updated after major codebase changes
- [ ] Team has reviewed and approved

### Testing Process

1. **Create test branch** with instruction changes
2. **Test with common scenarios**:
   - Generate new component
   - Write test for existing function
   - Ask about architecture patterns
3. **Compare suggestions** to your standards
4. **Iterate** based on results
5. **Merge** after validation

---

## Key Takeaways

1. **Layer instructions** from organization to user level
2. **Be specific** with examples and patterns
3. **Keep updated** as your codebase evolves
4. **Scope appropriately** using applyTo patterns
5. **Use knowledge bases** for organizational context
6. **Test and iterate** on instruction effectiveness
