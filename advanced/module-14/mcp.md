---
layout: training-module
title: "Model Context Protocol"
permalink: /advanced/module-14/mcp/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 3
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
  url: /advanced/module-14/agents/
  title: "Copilot Agents"
next_section:
  url: /advanced/module-14/extensions/
  title: "Copilot Extensions"
---

## What is Model Context Protocol?

Model Context Protocol (MCP) is an open standard that enables AI models to connect with external data sources and tools. Think of it as a universal adapter that lets Copilot access information beyond your code.

### The Problem MCP Solves

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph Before["Before MCP"]
    CP1["🤖 Copilot"] --> Code1["📄 Code in editor"]
    X1["❌ Can't access internal documentation"]
    X2["❌ Can't query databases"]
    X3["❌ Can't use internal APIs"]
    X4["❌ Can't read company wikis"]
  end
</div>
</div>

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph After["With MCP"]
    CP2["🤖 Copilot"]
    CP2 --> MCP1["🔌 MCP Server"] --> Docs["📚 Internal Docs"]
    CP2 --> MCP2["🔌 MCP Server"] --> DB["💾 Database"]
    CP2 --> MCP3["🔌 MCP Server"] --> API["🔗 Company API"]
    CP2 --> MCP4["🔌 MCP Server"] --> KB["📖 Knowledge Base"]
  end
</div>
</div>

### MCP Architecture

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Architecture["MCP Architecture"]
    Host["🖥️ AI Host (Client)<br/>VS Code + Copilot"]
    Host -->|"MCP Protocol (JSON-RPC)"| Server["🔌 MCP Server<br/>(Local or Remote)"]
    Server --> R["📄 Resources (data sources)"]
    Server --> T["🛠️ Tools (actions)"]
    Server --> P["📝 Prompts (templates)"]
  end
</div>
</div>

### MCP Capabilities

| Capability | Description | Example |
|-----------|-------------|---------|
| **Resources** | Read-only data sources | Documentation, configs |
| **Tools** | Actions that modify state | Create ticket, run query |
| **Prompts** | Pre-built prompt templates | Code review checklist |

## Setting Up MCP Servers

### Configuration Location

MCP servers are configured in VS Code settings or a dedicated config file:

**VS Code Settings (settings.json):**

```json
{
  "mcp.servers": {
    "company-docs": {
      "command": "npx",
      "args": ["-y", "@company/docs-mcp-server"],
      "env": {
        "API_KEY": "${env:COMPANY_DOCS_API_KEY}"
      }
    },
    "database": {
      "command": "python",
      "args": ["-m", "db_mcp_server"],
      "cwd": "${workspaceFolder}",
      "env": {
        "DB_CONNECTION": "${env:DATABASE_URL}"
      }
    }
  }
}

```

### Popular MCP Servers

| Server | Purpose | Source |
|--------|---------|--------|
| `@modelcontextprotocol/server-filesystem` | Local file access | Official |
| `@modelcontextprotocol/server-github` | GitHub API access | Official |
| `@modelcontextprotocol/server-postgres` | PostgreSQL queries | Official |
| `@modelcontextprotocol/server-sqlite` | SQLite database | Official |
| `@modelcontextprotocol/server-fetch` | HTTP requests | Official |

### Installing a Server

```bash
# Install official filesystem server
npm install -g @modelcontextprotocol/server-filesystem

# Or use npx for on-demand
# (configured in mcp.servers as shown above)

```

## Using MCP in Copilot Chat

### Querying MCP Resources

Once configured, MCP resources are available through chat participants:

```
# Access company documentation
@mcp-company-docs How do I configure the payment service?

# Query database
@mcp-database What are the top 10 customers by order volume?

# Search internal wiki
@mcp-wiki What's our policy on error handling?

```

### Using MCP Tools

```
# Create a Jira ticket
@mcp-jira Create a bug ticket for the login issue

# Run a database query
@mcp-postgres SELECT count(*) FROM orders WHERE status = 'pending'

# Fetch API data
@mcp-fetch Get the current deployment status from our monitoring API

```

## Building Custom MCP Servers

### Basic Server Structure (TypeScript)

```typescript
import { Server } from "@modelcontextprotocol/sdk/server";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio";

// Create server instance
const server = new Server({
  name: "my-company-server",
  version: "1.0.0"
}, {
  capabilities: {
    resources: {},
    tools: {}
  }
});

// Define a resource
server.setRequestHandler("resources/list", async () => {
  return {
    resources: [
      {
        uri: "docs://api-reference",
        name: "API Reference",
        description: "Internal API documentation",
        mimeType: "text/markdown"
      }
    ]
  };
});

// Define a tool
server.setRequestHandler("tools/list", async () => {
  return {
    tools: [
      {
        name: "search_docs",
        description: "Search internal documentation",
        inputSchema: {
          type: "object",
          properties: {
            query: { type: "string", description: "Search query" }
          },
          required: ["query"]
        }
      }
    ]
  };
});

// Handle tool calls
server.setRequestHandler("tools/call", async (request) => {
  if (request.params.name === "search_docs") {
    const query = request.params.arguments.query;
    // Implement search logic
    const results = await searchInternalDocs(query);
    return { content: [{ type: "text", text: JSON.stringify(results) }] };
  }
});

// Start server
const transport = new StdioServerTransport();
await server.connect(transport);

```

### Python Server Example

```python
from mcp.server import Server
from mcp.server.stdio import stdio_server
from mcp.types import Resource, Tool, TextContent

server = Server("company-knowledge")

@server.list_resources()
async def list_resources():
    return [
        Resource(
            uri="knowledge://policies",
            name="Company Policies",
            description="Internal policy documents",
            mimeType="text/plain"
        )
    ]

@server.read_resource()
async def read_resource(uri: str):
    if uri == "knowledge://policies":
        content = load_policies()  # Your implementation
        return TextContent(text=content)

@server.list_tools()
async def list_tools():
    return [
        Tool(
            name="search_knowledge_base",
            description="Search company knowledge base",
            inputSchema={
                "type": "object",
                "properties": {
                    "query": {"type": "string"}
                },
                "required": ["query"]
            }
        )
    ]

@server.call_tool()
async def call_tool(name: str, arguments: dict):
    if name == "search_knowledge_base":
        results = search_kb(arguments["query"])
        return TextContent(text=str(results))

async def main():
    async with stdio_server() as (read, write):
        await server.run(read, write)

if __name__ == "__main__":
    import asyncio
    asyncio.run(main())

```

## Enterprise MCP Use Cases

### Use Case 1: Internal Documentation

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph DocsMCP["Internal Docs MCP Server"]
    subgraph Resources["📄 Resources"]
      R1["Architecture decision records"]
      R2["API documentation"]
      R3["Runbooks and playbooks"]
      R4["Coding standards"]
    end
    
    subgraph Tools["🛠️ Tools"]
      T1["search_docs(query)"]
      T2["get_related_docs(topic)"]
    end
  end
</div>
</div>

**Chat interaction:**

```
User: @internal-docs How should I implement retry logic in our services?

Copilot: Based on your internal documentation...

[Architecture Decision Record 042: Retry Patterns]
- Use exponential backoff with jitter
- Maximum 3 retries for transient failures
- Circuit breaker for persistent failures

[Code Example from Service Template]

```typescript

import { retryWithBackoff } from '@company/resilience';

const result = await retryWithBackoff(
  () => externalApiCall(),
  { maxRetries: 3, baseDelay: 1000 }
);

```

```

### Use Case 2: Database Access

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph DBMCP["Database MCP Server"]
    subgraph Tools["🛠️ Tools"]
      T1["query_readonly(sql) - Read-only queries"]
      T2["describe_table(name) - Get table schema"]
      T3["list_tables() - Available tables"]
    end
    
    subgraph Security["🔐 Security"]
      S1["Read-only connection"]
      S2["Query timeout limits"]
      S3["Audit logging"]
    end
  end
</div>
</div>

### Use Case 3: Monitoring Integration

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph MonMCP["Monitoring MCP Server"]
    subgraph Resources["📄 Resources"]
      R1["Current alerts"]
      R2["Recent incidents"]
      R3["Service health status"]
    end
    
    subgraph Tools["🛠️ Tools"]
      T1["get_metrics(service, timerange)"]
      T2["get_logs(service, filter)"]
      T3["get_deployment_history(service)"]
    end
  end
</div>
</div>

**Chat interaction:**

```
User: @monitoring Why is the checkout service slow today?

Copilot: I checked your monitoring systems...

Current Status: Degraded
- Response time: 2.3s (normal: 200ms)
- Error rate: 5.2%

Recent Events:

- 10:30 AM: Database connection pool exhausted
- 10:45 AM: Auto-scaling triggered
- 11:00 AM: Connection pool size increased

Likely Cause: Traffic spike exceeded connection pool capacity.

Suggested Fix: [Link to runbook-db-connections]

```

## Security Considerations

### Authentication

```json
{
  "mcp.servers": {
    "secure-server": {
      "command": "node",
      "args": ["server.js"],
      "env": {
        "API_TOKEN": "${env:SECURE_API_TOKEN}",
        "AUTH_METHOD": "bearer"
      }
    }
  }
}

```

### Data Access Controls

| Level | Control |
|-------|---------|
| Server | Only expose what's needed |
| Query | Implement read-only modes |
| Data | Filter sensitive fields |
| Audit | Log all queries |

### Best Practices

1. **Principle of least privilege**: Expose minimal necessary data
2. **Read-only by default**: Write operations require explicit approval
3. **Audit logging**: Track all MCP interactions
4. **Secret management**: Use environment variables, not hardcoded keys
5. **Network isolation**: Run servers in secure network zones

---

**Next:** Learn how to build Copilot Extensions for custom chat participants.
