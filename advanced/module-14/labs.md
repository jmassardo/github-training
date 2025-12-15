---
layout: training-module
title: "Hands-On Labs"
permalink: /advanced/module-14/labs/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 8
total_sections: 10
phase: advanced
estimated_time: "90 min"
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
  url: /advanced/module-14/metrics/
  title: "Metrics & Adoption"
next_section:
  url: /advanced/module-14/quiz/
  title: "Knowledge Check"
---

## Advanced Copilot Labs

These hands-on labs provide practical experience with advanced GitHub Copilot features covered in this module.

### Prerequisites

- GitHub Copilot Enterprise license (or Business for some labs)
- VS Code with GitHub Copilot extension installed
- Node.js 18+ and npm installed
- GitHub CLI authenticated (`gh auth login`)
- A test repository to work with

### Time Estimate

| Lab | Duration | Difficulty |
|-----|----------|------------|
| Lab 1: Coding Agent | 20 min | Intermediate |
| Lab 2: MCP Server | 25 min | Advanced |
| Lab 3: Custom Instructions | 15 min | Beginner |
| Lab 4: Metrics Dashboard | 20 min | Intermediate |
| Lab 5: Integration Challenge | 30 min | Advanced |

---

## Lab 1: Working with the Coding Agent

### Objective

Use GitHub Copilot's coding agent to implement a feature from an issue.

### Setup

```bash
# Create a test repository
gh repo create copilot-labs-test --public --clone
cd copilot-labs-test

# Initialize a Node.js project
npm init -y
npm install express typescript @types/node @types/express ts-node
npx tsc --init

# Create basic structure
mkdir src
cat > src/index.ts << 'EOF'
import express from 'express';

const app = express();
app.use(express.json());

app.get('/health', (req, res) => {
  res.json({ status: 'healthy' });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
EOF

# Commit initial code
git add .
git commit -m "Initial project setup"
git push origin main

```

### Task 1.1: Create a Feature Issue

1. Navigate to your repository on GitHub
2. Create a new issue with this content:

```markdown
## Feature: User Management API

### Description
Implement a basic user management API with the following endpoints:

### Requirements
- `GET /users` - List all users with pagination
- `GET /users/:id` - Get user by ID
- `POST /users` - Create new user
- `PUT /users/:id` - Update user
- `DELETE /users/:id` - Delete user

### User Model

```typescript

interface User {
  id: string;
  email: string;
  name: string;
  role: 'admin' | 'user';
  createdAt: Date;
}

```

### Acceptance Criteria
- [ ] All endpoints implemented
- [ ] Input validation on create/update
- [ ] Proper error responses
- [ ] TypeScript types defined
- [ ] Basic unit tests included

```

### Task 1.2: Assign to Copilot

1. On the issue page, find the "Assignees" section
2. Click and select "Copilot" (if available in your plan)
3. Alternatively, in Chat, use: `@github Implement this issue #1`

### Task 1.3: Monitor Progress

1. Watch the Copilot agent's activity in the issue comments
2. Observe the branch creation and commits
3. Review the generated pull request

### Task 1.4: Review and Provide Feedback

1. Open the generated PR
2. Review the code changes
3. Add comments requesting improvements:
   - "Add input validation for email format"
   - "Include error handling for not found cases"
4. Watch Copilot address your feedback

### Validation Checklist

- [ ] Issue created with clear requirements
- [ ] Copilot assigned or invoked
- [ ] PR generated with implementation
- [ ] Feedback addressed by Copilot
- [ ] Code follows TypeScript best practices

---

## Lab 2: Building an MCP Server

### Objective

Create a custom MCP server that provides project-specific tools to Copilot.

### Setup

```bash
# Create MCP server project
mkdir copilot-mcp-lab && cd copilot-mcp-lab
npm init -y

# Install dependencies
npm install @modelcontextprotocol/sdk typescript @types/node ts-node
npm install -D @types/node

# Initialize TypeScript
npx tsc --init --target ES2022 --module NodeNext --moduleResolution NodeNext

```

### Task 2.1: Create the Server

Create `src/server.ts`:

```typescript
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import {
  CallToolRequestSchema,
  ListToolsRequestSchema,
} from "@modelcontextprotocol/sdk/types.js";

// Create server instance
const server = new Server(
  {
    name: "project-tools",
    version: "1.0.0",
  },
  {
    capabilities: {
      tools: {},
    },
  }
);

// Mock data store
const projects = new Map([
  ["PRJ-001", { id: "PRJ-001", name: "Website Redesign", status: "active", completion: 75 }],
  ["PRJ-002", { id: "PRJ-002", name: "Mobile App", status: "planning", completion: 10 }],
  ["PRJ-003", { id: "PRJ-003", name: "API Migration", status: "completed", completion: 100 }],
]);

// List available tools
server.setRequestHandler(ListToolsRequestSchema, async () => ({
  tools: [
    {
      name: "get_project_status",
      description: "Get the current status of a project by ID",
      inputSchema: {
        type: "object" as const,
        properties: {
          projectId: {
            type: "string",
            description: "The project ID (e.g., PRJ-001)",
          },
        },
        required: ["projectId"],
      },
    },
    {
      name: "list_projects",
      description: "List all projects with their status",
      inputSchema: {
        type: "object" as const,
        properties: {
          status: {
            type: "string",
            enum: ["active", "planning", "completed", "all"],
            description: "Filter by status (default: all)",
          },
        },
      },
    },
    {
      name: "search_documentation",
      description: "Search internal documentation",
      inputSchema: {
        type: "object" as const,
        properties: {
          query: {
            type: "string",
            description: "Search query",
          },
        },
        required: ["query"],
      },
    },
  ],
}));

// Handle tool calls
server.setRequestHandler(CallToolRequestSchema, async (request) => {
  const { name, arguments: args } = request.params;

  switch (name) {
    case "get_project_status": {
      const projectId = args?.projectId as string;
      const project = projects.get(projectId);
      
      if (!project) {
        return {
          content: [{ type: "text", text: `Project ${projectId} not found` }],
          isError: true,
        };
      }
      
      return {
        content: [{
          type: "text",
          text: JSON.stringify(project, null, 2),
        }],
      };
    }

    case "list_projects": {
      const status = args?.status as string || "all";
      let filtered = Array.from(projects.values());
      
      if (status !== "all") {
        filtered = filtered.filter(p => p.status === status);
      }
      
      return {
        content: [{
          type: "text",
          text: JSON.stringify(filtered, null, 2),
        }],
      };
    }

    case "search_documentation": {
      const query = (args?.query as string || "").toLowerCase();
      
      // Mock documentation search
      const docs = [
        { title: "Getting Started", path: "/docs/getting-started.md", snippet: "Quick start guide for new developers" },
        { title: "API Reference", path: "/docs/api/index.md", snippet: "Complete API documentation" },
        { title: "Deployment Guide", path: "/docs/deployment.md", snippet: "How to deploy to production" },
      ];
      
      const results = docs.filter(d => 
        d.title.toLowerCase().includes(query) || 
        d.snippet.toLowerCase().includes(query)
      );
      
      return {
        content: [{
          type: "text",
          text: results.length > 0 
            ? JSON.stringify(results, null, 2)
            : "No documentation found matching your query",
        }],
      };
    }

    default:
      return {
        content: [{ type: "text", text: `Unknown tool: ${name}` }],
        isError: true,
      };
  }
});

// Start the server
async function main() {
  const transport = new StdioServerTransport();
  await server.connect(transport);
  console.error("MCP server running");
}

main().catch(console.error);

```

### Task 2.2: Configure VS Code

Add to `.vscode/settings.json` in your workspace:

```json
{
  "github.copilot.chat.mcp.servers": {
    "project-tools": {
      "command": "npx",
      "args": ["ts-node", "--esm", "src/server.ts"],
      "cwd": "${workspaceFolder}/copilot-mcp-lab"
    }
  }
}

```

### Task 2.3: Test Your Server

1. Restart VS Code to load the MCP configuration
2. Open Copilot Chat
3. Test with these prompts:
   - "What's the status of project PRJ-001?"
   - "List all active projects"
   - "Search documentation for deployment"

### Task 2.4: Extend the Server

Add a new tool to your server:

```typescript
// Add to tools list
{
  name: "create_task",
  description: "Create a new task for a project",
  inputSchema: {
    type: "object" as const,
    properties: {
      projectId: { type: "string", description: "Project ID" },
      title: { type: "string", description: "Task title" },
      priority: { 
        type: "string", 
        enum: ["low", "medium", "high"],
        description: "Task priority" 
      },
    },
    required: ["projectId", "title"],
  },
}

// Add handler in switch statement
case "create_task": {
  const { projectId, title, priority = "medium" } = args as any;
  // Mock task creation
  const taskId = `TSK-${Date.now()}`;
  return {
    content: [{
      type: "text",
      text: JSON.stringify({
        success: true,
        task: { id: taskId, projectId, title, priority, status: "created" }
      }, null, 2),
    }],
  };
}

```

### Validation Checklist

- [ ] MCP server compiles without errors
- [ ] Server starts successfully
- [ ] Tools appear in Copilot
- [ ] Tools return correct data
- [ ] Custom tool added and working

---

## Lab 3: Custom Instructions

### Objective

Create and test custom instructions for your repository.

### Task 3.1: Create Repository Instructions

Create `.github/copilot-instructions.md`:

```markdown
# Project Copilot Instructions

## Project Overview
This is a TypeScript/Express REST API following clean architecture principles.

## Code Style

### TypeScript
- Use strict mode and explicit types
- Prefer interfaces over type aliases
- Use async/await, never callbacks
- No `any` - use `unknown` if type is unclear

### Naming Conventions
- Files: kebab-case (user-service.ts)
- Classes: PascalCase (UserService)
- Functions/variables: camelCase (getUserById)
- Constants: UPPER_SNAKE_CASE (MAX_RETRIES)
- Interfaces: PascalCase with 'I' prefix for contracts (IUserRepository)

### Error Handling
Use the custom AppError class for all errors:

```typescript

import { AppError } from '@/errors';

// Usage
throw new AppError('User not found', 404, 'USER_NOT_FOUND');

```

### API Response Format
All API responses must use this structure:

```typescript

// Success
{
  success: true,
  data: { ... },
  meta: { pagination: ... }  // if applicable
}

// Error
{
  success: false,
  error: {
    code: 'ERROR_CODE',
    message: 'Human readable message'
  }
}

```

### Testing
- Write tests alongside implementation
- Use Jest with ts-jest
- Mock external dependencies
- Aim for 80% coverage on business logic

```

### Task 3.2: Create Scoped Instructions

Create `.vscode/instructions/routes.md`:

```markdown
---
applyTo: "**/routes/**/*.ts"
---

# Route Development Guidelines

When creating API routes:

1. Use the asyncHandler wrapper for all handlers
2. Validate input with Zod schemas
3. Return responses through ApiResponse helper
4. Add OpenAPI JSDoc comments

Example:

```typescript

import { Router } from 'express';
import { asyncHandler, ApiResponse } from '@/utils';
import { UserSchema } from '@/schemas';

const router = Router();

/**
 * @openapi
 * /users/{id}:
 *   get:
 *     summary: Get user by ID
 *     parameters:
 *       - name: id
 *         in: path
 *         required: true
 *     responses:
 *       200:
 *         description: User found
 */
router.get('/:id', asyncHandler(async (req, res) => {
  const { id } = req.params;
  const user = await userService.findById(id);
  return ApiResponse.success(res, user);
}));

export default router;

```

```

### Task 3.3: Test Your Instructions

1. Open Copilot Chat
2. Ask: "Generate a new route for managing products"
3. Verify the generated code follows your instructions
4. Ask: "What error handling pattern should I use?"
5. Verify it references your AppError class

### Task 3.4: Refine Based on Results

If Copilot doesn't follow your patterns:
- Make instructions more specific
- Add more examples
- Clarify ambiguous guidance
- Test again

### Validation Checklist

- [ ] Repository instructions file created
- [ ] Scoped instructions created
- [ ] Copilot follows naming conventions
- [ ] Copilot uses specified patterns
- [ ] Generated code matches your style

---

## Lab 4: Metrics Dashboard

### Objective

Create scripts to collect and visualize Copilot metrics.

### Task 4.1: Create Metrics Collection Script

Create `scripts/collect-metrics.sh`:

```bash
#!/bin/bash
# Copilot Metrics Collection Script

ORG="${GITHUB_ORG:-your-org}"
OUTPUT_DIR="./metrics"
DATE=$(date +%Y%m%d)

mkdir -p "$OUTPUT_DIR"

echo "Collecting Copilot metrics for $ORG..."

# Get seat information
echo "Collecting seat data..."
gh api "/orgs/$ORG/copilot/billing" > "$OUTPUT_DIR/billing_$DATE.json"
gh api "/orgs/$ORG/copilot/billing/seats" > "$OUTPUT_DIR/seats_$DATE.json"

# Get usage data
echo "Collecting usage data..."
gh api "/orgs/$ORG/copilot/usage" > "$OUTPUT_DIR/usage_$DATE.json"

# Generate summary
echo "Generating summary..."
cat > "$OUTPUT_DIR/summary_$DATE.md" << EOF
# Copilot Metrics Summary - $(date +%Y-%m-%d)

## Seat Utilization
$(gh api "/orgs/$ORG/copilot/billing" --jq '"- Total Seats: \(.seat_breakdown.total)\n- Active This Cycle: \(.seat_breakdown.active_this_cycle)"')

## Usage (Last 7 Days)
$(gh api "/orgs/$ORG/copilot/usage" --jq '.usage_items[-7:] | "| Date | Active Users | Acceptances |\n|------|--------------|-------------|" + ([.[] | "| \(.day) | \(.total_active_users) | \(.total_acceptances_count) |"] | join("\n"))')

## Inactive Users (14+ days)
$(gh api "/orgs/$ORG/copilot/billing/seats" --jq '[.seats[] | select(.last_activity_at < (now - 1209600 | todate))] | if length > 0 then (["User", "Last Active"] | @tsv) + "\n" + ([.[] | [.assignee.login, .last_activity_at] | @tsv] | join("\n")) else "No inactive users" end')
EOF

echo "Metrics collected in $OUTPUT_DIR/"
echo "Summary: $OUTPUT_DIR/summary_$DATE.md"

```

### Task 4.2: Create Analysis Script

Create `scripts/analyze-metrics.js`:

```javascript
#!/usr/bin/env node
const fs = require('fs');
const path = require('path');

const metricsDir = './metrics';

// Load latest files
function loadLatest(prefix) {
  const files = fs.readdirSync(metricsDir)
    .filter(f => f.startsWith(prefix) && f.endsWith('.json'))
    .sort()
    .reverse();
  
  if (files.length === 0) return null;
  return JSON.parse(fs.readFileSync(path.join(metricsDir, files[0])));
}

const usage = loadLatest('usage_');
const seats = loadLatest('seats_');
const billing = loadLatest('billing_');

if (!usage || !seats) {
  console.error('No metrics data found. Run collect-metrics.sh first.');
  process.exit(1);
}

// Calculate metrics
const totalSeats = billing?.seat_breakdown?.total || seats.seats.length;
const activeSeats = billing?.seat_breakdown?.active_this_cycle || 0;
const utilizationRate = ((activeSeats / totalSeats) * 100).toFixed(1);

const last7Days = usage.usage_items.slice(-7);
const avgAcceptanceRate = last7Days.reduce((sum, day) => {
  if (day.total_suggestions_count === 0) return sum;
  return sum + (day.total_acceptances_count / day.total_suggestions_count);
}, 0) / last7Days.length * 100;

const avgActiveUsers = last7Days.reduce((sum, day) => sum + day.total_active_users, 0) / 7;

// Output report
console.log('='.repeat(50));
console.log('COPILOT METRICS ANALYSIS');
console.log('='.repeat(50));
console.log();
console.log('SEAT UTILIZATION');
console.log(`  Total Seats:     ${totalSeats}`);
console.log(`  Active (Cycle):  ${activeSeats}`);
console.log(`  Utilization:     ${utilizationRate}%`);
console.log();
console.log('USAGE (7-Day Average)');
console.log(`  Active Users:    ${avgActiveUsers.toFixed(1)}`);
console.log(`  Acceptance Rate: ${avgAcceptanceRate.toFixed(1)}%`);
console.log();
console.log('STATUS');
if (parseFloat(utilizationRate) < 70) {
  console.log('  ⚠️  Low utilization - consider seat review');
} else {
  console.log('  ✅ Healthy utilization');
}
if (avgAcceptanceRate < 20) {
  console.log('  ⚠️  Low acceptance rate - consider training');
} else {
  console.log('  ✅ Good acceptance rate');
}
console.log('='.repeat(50));

```

### Task 4.3: Run the Dashboard

```bash
# Set your organization
export GITHUB_ORG="your-org"

# Collect metrics
chmod +x scripts/collect-metrics.sh
./scripts/collect-metrics.sh

# Analyze metrics
node scripts/analyze-metrics.js

```

### Validation Checklist

- [ ] Collection script runs successfully
- [ ] JSON data files created
- [ ] Summary markdown generated
- [ ] Analysis script calculates metrics
- [ ] Dashboard identifies issues

---

## Lab 5: Integration Challenge

### Objective

Combine all advanced features into a cohesive development workflow.

### Scenario

Your team is building a new microservice. Use Copilot's advanced features throughout the entire development process.

### Task 5.1: Project Setup (10 min)

1. Create a new repository with initial structure
2. Add custom instructions for your patterns
3. Configure MCP server for your internal tools (optional)

### Task 5.2: Feature Development (15 min)

1. Create a GitHub issue describing a feature
2. Use the coding agent to implement it
3. Review the generated PR
4. Provide feedback and iterate

### Task 5.3: Documentation (5 min)

1. Use Copilot Chat to generate API documentation
2. Create README with setup instructions
3. Add code comments for complex logic

### Task 5.4: Measurement (5 min)

1. Document time spent on each phase
2. Note quality of Copilot assistance
3. Identify what worked well and what didn't

### Deliverables

- [ ] Working microservice code
- [ ] Custom instructions file
- [ ] Generated documentation
- [ ] Lessons learned document

---

## Tips for Success

1. **Read instructions carefully** before starting each lab
2. **Experiment freely** - Copilot improves with iteration
3. **Document your approach** for future reference
4. **Ask for help** in Copilot Chat when stuck
5. **Share learnings** with your team

## Troubleshooting

| Issue | Solution |
|-------|----------|
| MCP server not loading | Check VS Code settings, restart VS Code |
| Copilot not following instructions | Make instructions more specific |
| API calls failing | Verify `gh auth status` and permissions |
| Agent not available | Confirm Enterprise license |
