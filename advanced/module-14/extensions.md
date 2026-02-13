---
layout: training-module
title: "Copilot Extensions"
permalink: /advanced/module-14/extensions/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 4
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
  url: /advanced/module-14/mcp/
  title: "Model Context Protocol"
next_section:
  url: /advanced/module-14/instructions/
  title: "Custom Instructions"
---

## What Are Copilot Extensions?

Copilot Extensions allow you to create custom chat participants that integrate with your organization's tools and workflows. They appear in Copilot Chat with an `@` mention prefix.

### Built-in vs. Custom Participants

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph Participants["Chat Participants"]
    subgraph BuiltIn["📦 Built-in"]
      B1["@workspace - Codebase context"]
      B2["@terminal - Terminal commands"]
      B3["@vscode - Editor features"]
      B4["@github - GitHub operations"]
    end
    
    subgraph Custom["🔧 Custom Extensions"]
      C1["@company-docs - Internal documentation"]
      C2["@deploy - Deployment automation"]
      C3["@oncall - Incident management"]
      C4["@security - Security scanning"]
    end
  end
</div>
</div>

### Extension Types

| Type | Location | Use Case |
|------|----------|----------|
| **GitHub App Extension** | GitHub.com | GitHub-integrated workflows |
| **VS Code Extension** | Local IDE | IDE-specific features |
| **Enterprise Extension** | Organization | Internal tools integration |

## Building a GitHub App Extension

### Architecture Overview

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
  subgraph Architecture["Copilot Extension Architecture"]
    Chat["💬 Copilot Chat"] --> API["🔗 GitHub/Copilot API"] --> Backend["🖥️ Your Backend Server"]
    Backend --> Services
    
    subgraph Services["Your Services"]
      DB["💾 Databases"]
      APIs["🔗 APIs"]
      Tools["🛠️ Tools"]
    end
  end
</div>
</div>

### Step 1: Create a GitHub App

1. Go to **Settings → Developer settings → GitHub Apps**
2. Click "New GitHub App"
3. Configure:
   - **Name**: `my-copilot-extension`
   - **Homepage URL**: Your documentation URL
   - **Callback URL**: Your OAuth callback
   - **Webhook URL**: Your webhook endpoint
   - **Permissions**: Based on your needs

### Step 2: Enable Copilot Extension

In your GitHub App settings, enable:

- **Copilot** → Enable as Copilot Extension
- Configure the agent endpoint URL

### Step 3: Build the Backend

```typescript
// server.ts - Express backend for Copilot Extension
import express from 'express';
import { Octokit } from '@octokit/rest';
import { verifyWebhookSignature } from './auth';

const app = express();
app.use(express.json());

// Copilot Extension endpoint
app.post('/copilot/agent', async (req, res) => {
  // Verify request is from GitHub
  if (!verifyWebhookSignature(req)) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
  
  const { messages, context } = req.body;
  const userMessage = messages[messages.length - 1].content;
  
  // Process the user's request
  const response = await processRequest(userMessage, context);
  
  // Return response in Copilot format
  res.json({
    choices: [{
      message: {
        role: 'assistant',
        content: response
      }
    }]
  });
});

async function processRequest(message: string, context: any): Promise<string> {
  // Your custom logic here
  // Could query databases, call APIs, etc.
  
  if (message.includes('deploy')) {
    return await handleDeployRequest(message);
  }
  
  if (message.includes('status')) {
    return await getServiceStatus();
  }
  
  return "I can help with deployments and service status. What would you like to know?";
}

app.listen(3000);

```

### Step 4: Handle Conversations

```typescript
// Maintain conversation context
interface ConversationContext {
  threadId: string;
  history: Message[];
  metadata: {
    user: string;
    repository?: string;
  };
}

app.post('/copilot/agent', async (req, res) => {
  const { messages, context } = req.body;
  
  // Build context from conversation history
  const conversationContext: ConversationContext = {
    threadId: context.threadId,
    history: messages,
    metadata: {
      user: context.user,
      repository: context.repository?.full_name
    }
  };
  
  // Use conversation history for context
  const response = await generateResponse(conversationContext);
  
  res.json({
    choices: [{
      message: {
        role: 'assistant',
        content: response
      }
    }]
  });
});

```

## VS Code Extension Integration

### Creating a Chat Participant in VS Code Extension

```typescript
// extension.ts
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  // Register chat participant
  const participant = vscode.chat.createChatParticipant(
    'mycompany.docs',
    handleDocsChat
  );
  
  participant.iconPath = vscode.Uri.joinPath(
    context.extensionUri, 
    'images', 
    'icon.png'
  );
  
  context.subscriptions.push(participant);
}

async function handleDocsChat(
  request: vscode.ChatRequest,
  context: vscode.ChatContext,
  stream: vscode.ChatResponseStream,
  token: vscode.CancellationToken
): Promise<vscode.ChatResult> {
  
  const query = request.prompt;
  
  // Show progress
  stream.progress('Searching documentation...');
  
  // Search internal documentation
  const results = await searchDocs(query);
  
  if (results.length === 0) {
    stream.markdown('No documentation found for that query.');
    return { metadata: { success: false } };
  }
  
  // Stream response
  stream.markdown('## Documentation Results\n\n');
  
  for (const doc of results) {
    stream.markdown(`### ${doc.title}\n\n`);
    stream.markdown(doc.content);
    stream.markdown('\n\n---\n\n');
  }
  
  return { metadata: { success: true, resultCount: results.length } };
}

async function searchDocs(query: string): Promise<Doc[]> {
  // Your documentation search implementation
  // Could call internal API, search local files, etc.
}

```

### Adding Slash Commands

```typescript
// Add slash commands to your participant
const participant = vscode.chat.createChatParticipant(
  'mycompany.deploy',
  handleDeployChat
);

// Define available commands
participant.followupProvider = {
  provideFollowups: (result, context, token) => {
    return [
      { command: 'staging', message: 'Deploy to staging' },
      { command: 'production', message: 'Deploy to production' },
      { command: 'status', message: 'Check deployment status' },
      { command: 'rollback', message: 'Rollback last deployment' }
    ];
  }
};

async function handleDeployChat(
  request: vscode.ChatRequest,
  context: vscode.ChatContext,
  stream: vscode.ChatResponseStream,
  token: vscode.CancellationToken
): Promise<vscode.ChatResult> {
  
  // Handle different commands
  switch (request.command) {
    case 'staging':
      return await deployToStaging(request, stream);
    case 'production':
      return await deployToProduction(request, stream);
    case 'status':
      return await getDeploymentStatus(stream);
    case 'rollback':
      return await rollbackDeployment(request, stream);
    default:
      return await handleGeneralDeployQuery(request, stream);
  }
}

```

## Enterprise Extension Patterns

### Pattern 1: Internal Documentation Search

```typescript
// docs-extension.ts
export async function searchInternalDocs(
  query: string,
  stream: ChatResponseStream
): Promise<void> {
  stream.progress('Searching knowledge base...');
  
  // Call internal documentation API
  const response = await fetch('https://docs.internal/api/search', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${process.env.DOCS_API_KEY}`,
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ query })
  });
  
  const results = await response.json();
  
  // Format and stream results
  stream.markdown('## Found in Internal Documentation\n\n');
  
  for (const result of results.hits) {
    stream.markdown(`### ${result.title}\n`);
    stream.markdown(`*Source: ${result.source}*\n\n`);
    stream.markdown(result.excerpt);
    stream.markdown(`\n\n[Read more](${result.url})\n\n---\n\n`);
  }
}

```

### Pattern 2: Service Status Dashboard

```typescript
// status-extension.ts
interface ServiceStatus {
  name: string;
  status: 'healthy' | 'degraded' | 'down';
  latency: number;
  lastIncident?: string;
}

export async function getServiceStatus(
  stream: ChatResponseStream
): Promise<void> {
  const services = await fetchServiceStatuses();
  
  stream.markdown('## Service Status Dashboard\n\n');
  stream.markdown('| Service | Status | Latency | Last Incident |\n');
  stream.markdown('|---------|--------|---------|---------------|\n');
  
  for (const service of services) {
    const statusEmoji = {
      healthy: '🟢',
      degraded: '🟡',
      down: '🔴'
    }[service.status];
    
    stream.markdown(
      `| ${service.name} | ${statusEmoji} ${service.status} | ${service.latency}ms | ${service.lastIncident || 'None'} |\n`
    );
  }
}

```

### Pattern 3: Deployment Automation

```typescript
// deploy-extension.ts
export async function handleDeployment(
  environment: string,
  stream: ChatResponseStream
): Promise<ChatResult> {
  // Confirmation step
  stream.markdown(`⚠️ You're about to deploy to **${environment}**\n\n`);
  stream.markdown('This will:\n');
  stream.markdown('- Build the latest main branch\n');
  stream.markdown('- Run integration tests\n');
  stream.markdown('- Deploy to ' + environment + '\n\n');
  
  // Would typically wait for confirmation
  stream.progress('Starting deployment...');
  
  try {
    const deployment = await triggerDeployment(environment);
    
    stream.markdown('✅ Deployment initiated!\n\n');
    stream.markdown(`**Deployment ID:** ${deployment.id}\n`);
    stream.markdown(`**Status:** ${deployment.status}\n`);
    stream.markdown(`**ETA:** ${deployment.eta}\n\n`);
    stream.markdown(`[View in Dashboard](${deployment.dashboardUrl})`);
    
    return { metadata: { deploymentId: deployment.id } };
  } catch (error) {
    stream.markdown('❌ Deployment failed!\n\n');
    stream.markdown(`**Error:** ${error.message}\n`);
    return { metadata: { error: true } };
  }
}

```

## Extension Best Practices

### Security

| Practice | Implementation |
|----------|---------------|
| Verify requests | Check GitHub signatures |
| Authenticate users | Validate OAuth tokens |
| Limit permissions | Request minimal scopes |
| Audit logging | Log all operations |
| Secret management | Use secure vaults |

### User Experience

| Practice | Why |
|----------|-----|
| Stream responses | Better perceived performance |
| Show progress | User knows work is happening |
| Provide followups | Guide users to next actions |
| Handle errors gracefully | Clear error messages |
| Rate limit | Prevent abuse |

### Performance

```typescript
// Implement caching for frequently accessed data
const cache = new Map<string, CacheEntry>();

async function getCachedData(key: string, fetcher: () => Promise<any>) {
  const cached = cache.get(key);
  
  if (cached && Date.now() - cached.timestamp < 60000) {
    return cached.data;
  }
  
  const data = await fetcher();
  cache.set(key, { data, timestamp: Date.now() });
  return data;
}

```

---

**Next:** Learn how to customize Copilot's behavior with Custom Instructions.
