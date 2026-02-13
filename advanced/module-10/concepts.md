---
layout: training-module
title: "Module 10, Section 2: Core Concepts"
description: "Deep dive into REST API, GraphQL, webhooks, and GitHub Apps"
permalink: /advanced/module-10/concepts/
module_number: 10
section_number: 2
phase: advanced
prev_section:
  url: /advanced/module-10/overview/
  title: "Context & Overview"
next_section:
  url: /advanced/module-10/walkthrough/
  title: "Guided Walkthrough"
toc: true
module_title: "GitHub APIs, Webhooks & Apps"
total_sections: 7
module_index: /advanced/module-10/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-10/overview/"
    short_title: "Overview"
    icon: "🎯"
    time: "15 min"
  - title: "Core Concepts"
    url: "/advanced/module-10/concepts/"
    short_title: "Concepts"
    icon: "📚"
    time: "45 min"
  - title: "Guided Walkthrough"
    url: "/advanced/module-10/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
    time: "45 min"
  - title: "Hands-On Labs"
    url: "/advanced/module-10/labs/"
    short_title: "Labs"
    icon: "💻"
    time: "45 min"
  - title: "Knowledge Check"
    url: "/advanced/module-10/quiz/"
    short_title: "Quiz"
    icon: "✅"
    time: "20 min"
  - title: "Resources"
    url: "/advanced/module-10/resources/"
    short_title: "Resources"
    icon: "📖"
    time: "10 min"
  - title: "CSM Scenarios"
    url: "/advanced/module-10/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
    time: "25 min"
---
{% raw %}

<div class="callout callout-info">
<div class="callout-title">🔌 Official Documentation</div>
For comprehensive reference, see <a href="https://docs.github.com/en/rest">GitHub REST API Documentation</a>, <a href="https://docs.github.com/en/graphql">GraphQL API</a>, and <a href="https://docs.github.com/en/apps">GitHub Apps</a>.
</div>

## Section 2: Core Concepts Deep Dive

### 2.1 REST API Fundamentals
GitHub's REST API provides straightforward CRUD operations:

#### API Structure

```
Base URL: https://api.github.com
Endpoint Pattern: /{resource}/{identifier}
Examples:
  GET  /repos/{owner}/{repo}           # Get repository
  GET  /repos/{owner}/{repo}/issues    # List issues
  POST /repos/{owner}/{repo}/issues    # Create issue
  PATCH /repos/{owner}/{repo}/issues/{issue_number}  # Update issue

```

#### Common REST API Operations

| Operation | Method | Endpoint | Use Case |
|-----------|--------|----------|----------|
| List repos | GET | `/orgs/{org}/repos` | Inventory |
| Get user | GET | `/users/{username}` | Profile lookup |
| Create issue | POST | `/repos/{owner}/{repo}/issues` | Automation |
| Update PR | PATCH | `/repos/{owner}/{repo}/pulls/{number}` | Status updates |
| Delete branch | DELETE | `/repos/{owner}/{repo}/git/refs/heads/{branch}` | Cleanup |

#### Authentication Methods

```bash
# Personal Access Token - fine-grained (recommended)
curl -H "Authorization: Bearer github_pat_xxxxxxxxxxxx" \
  https://api.github.com/user

# Personal Access Token - classic (being phased down)
curl -H "Authorization: token ghp_xxxxxxxxxxxx" \
  https://api.github.com/user

# GitHub App Installation Token
curl -H "Authorization: Bearer ghs_xxxxxxxxxxxx" \
  https://api.github.com/repos/owner/repo

```

<div class="callout callout-warning">
<div class="callout-title">⚠️ Classic PATs Are Being Phased Down</div>

Fine-grained personal access tokens (`github_pat_`) are the recommended approach for new integrations. They offer repository-scoped access, granular permissions, and mandatory expiration. Organizations can restrict or disable classic PAT usage via policies.
</div>

#### API Versioning

GitHub uses date-based API versioning. Always include the version header to ensure consistent behavior:

```bash
curl -H "X-GitHub-Api-Version: 2022-11-28" \
  -H "Authorization: Bearer github_pat_xxxxxxxxxxxx" \
  https://api.github.com/repos/owner/repo
```

Breaking changes are introduced in new API versions. Pin to a specific version to avoid unexpected changes in your integrations.

#### Pagination

```bash
# First page (default 30 items)
curl "https://api.github.com/repos/owner/repo/issues"
# With pagination parameters
curl "https://api.github.com/repos/owner/repo/issues?per_page=100&page=2"
# Response headers contain pagination links
# Link: <https://api.github.com/.../issues?page=2>; rel="next",
#       <https://api.github.com/.../issues?page=5>; rel="last"

```

### 2.2 GraphQL API
GraphQL allows precise data fetching in a single request:

#### Why GraphQL?

| REST Challenge | GraphQL Solution |
|---------------|------------------|
| Multiple requests for related data | Single query for nested data |
| Over-fetching (too much data) | Request exactly what you need |
| Under-fetching (missing data) | Include related objects |
| Multiple endpoints | Single endpoint |

#### GraphQL Query Structure

```graphql
# Basic query
query {
  viewer {
    login
    name
    email
  }
}
# Query with variables
query GetRepository($owner: String!, $name: String!) {
  repository(owner: $owner, name: $name) {
    name
    description
    stargazerCount
    issues(first: 10, states: OPEN) {
      totalCount
      nodes {
        title
        author {
          login
        }
      }
    }
  }
}

```

#### GraphQL Endpoint

```bash
# All GraphQL requests go to single endpoint
curl -X POST https://api.github.com/graphql \
  -H "Authorization: bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"query": "{ viewer { login } }"}'

```

#### Useful GraphQL Queries

```graphql
# Get organization members and teams
query OrgOverview($org: String!) {
  organization(login: $org) {
    membersWithRole(first: 100) {
      totalCount
      nodes {
        login
        name
      }
    }
    teams(first: 50) {
      nodes {
        name
        members {
          totalCount
        }
      }
    }
  }
}
# Get PR review status
query PRReviews($owner: String!, $repo: String!, $number: Int!) {
  repository(owner: $owner, name: $repo) {
    pullRequest(number: $number) {
      title
      state
      reviews(first: 20) {
        nodes {
          author {
            login
          }
          state
          submittedAt
        }
      }
      reviewRequests(first: 10) {
        nodes {
          requestedReviewer {
            ... on User {
              login
            }
            ... on Team {
              name
            }
          }
        }
      }
    }
  }
}

```

### 2.3 Webhooks
Webhooks provide real-time notifications when events occur:

#### How Webhooks Work

<div class="mermaid-container">
<div class="mermaid">
sequenceDiagram
    participant Event as Event Triggered<br/>(push, PR)
    participant Config as Webhook Config<br/>• Payload URL<br/>• Secret<br/>• Events
    participant Server as YOUR SERVER
    
    rect rgb(240, 240, 240)
    Note over Event,Config: GITHUB
    Event->>Config: 1. Event Occurs
    end
    
    Config->>Server: 2. HTTP POST with payload
    
    rect rgb(230, 245, 230)
    Note over Server: Process Request
    Server->>Server: Verify Signature
    Server->>Server: Process Event
    Server-->>Config: Respond 200 OK
    end
</div>
</div>

#### Common Webhook Events

| Event | Trigger | Common Use Case |
|-------|---------|-----------------|
| `push` | Code pushed | Trigger CI builds |
| `pull_request` | PR opened/updated/merged | Code review automation |
| `issues` | Issue created/updated | Triage automation |
| `check_run` | CI status changed | Status notifications |
| `deployment` | Deployment created | CD orchestration |
| `member` | Team membership changed | Access notifications |

#### Webhook Payload Example

```json
{
  "action": "opened",
  "pull_request": {
    "number": 42,
    "title": "Add new feature",
    "user": {
      "login": "developer"
    },
    "head": {
      "ref": "feature-branch",
      "sha": "abc123..."
    },
    "base": {
      "ref": "main"
    }
  },
  "repository": {
    "full_name": "owner/repo"
  },
  "sender": {
    "login": "developer"
  }
}

```

#### Webhook Security

```python
# Verify webhook signature (Python example)
import hmac
import hashlib
def verify_signature(payload_body, secret, signature_header):
    """Verify that the payload was sent by GitHub."""
    if not signature_header:
        return False
    
    hash_object = hmac.new(
        secret.encode('utf-8'),
        msg=payload_body,
        digestmod=hashlib.sha256
    )
    expected_signature = "sha256=" + hash_object.hexdigest()
    
    return hmac.compare_digest(expected_signature, signature_header)
# In your webhook handler
@app.route('/webhook', methods=['POST'])
def webhook():
    signature = request.headers.get('X-Hub-Signature-256')
    if not verify_signature(request.data, WEBHOOK_SECRET, signature):
        return 'Invalid signature', 403
    
    # Process webhook...

```

### 2.4 GitHub Apps
GitHub Apps are the recommended way to build integrations:

#### GitHub Apps vs. OAuth Apps

| Feature | GitHub Apps | OAuth Apps |
|---------|-------------|------------|
| **Installation** | Per org/repo | Per user |
| **Permissions** | Granular, declared upfront | Broad scopes |
| **Authentication** | App + Installation tokens | User OAuth tokens |
| **Rate Limits** | Higher (5,000/hr/install) | User-based (5,000/hr) |
| **Webhooks** | App receives all events | Per-user configuration |
| **Best For** | Automation, bots | User-facing integrations |

#### GitHub App Architecture

<div class="mermaid-container">
<div class="mermaid">
flowchart TD
  subgraph App["GITHUB APP"]
    subgraph Reg["APP REGISTRATION"]
      R1["• App ID"]
      R2["• Private Key"]
      R3["• Client ID/Secret"]
      R4["• Webhook URL"]
      R5["• Permissions"]
      R6["• Event Subscriptions"]
    end
    
    Reg -->|"Installation Flow"| OrgA & OrgB & UserC
    
    subgraph OrgA["Install in Org A"]
      A["Token: ghs_a"]
    end
    
    subgraph OrgB["Install in Org B"]
      B["Token: ghs_b"]
    end
    
    subgraph UserC["Install in User C"]
      C["Token: ghs_c"]
    end
  end
</div>
</div>

#### GitHub App Permissions

```yaml
# Example permission set for a CI/CD app
permissions:
  contents: read          # Clone repos
  pull_requests: write    # Comment on PRs
  checks: write           # Create check runs
  statuses: write         # Set commit statuses
  deployments: write      # Manage deployments
  
# Events to subscribe to
events:
  - push
  - pull_request
  - check_run
  - deployment

```

#### Authentication Flow for GitHub Apps

```python
import jwt
import time
import requests
# Step 1: Generate JWT from private key
def generate_jwt(app_id, private_key):
    payload = {
        'iat': int(time.time()),
        'exp': int(time.time()) + 600,  # 10 minutes max
        'iss': app_id
    }
    return jwt.encode(payload, private_key, algorithm='RS256')
# Step 2: Exchange JWT for installation token
def get_installation_token(jwt_token, installation_id):
    headers = {
        'Authorization': f'Bearer {jwt_token}',
        'Accept': 'application/vnd.github+json'
    }
    response = requests.post(
        f'https://api.github.com/app/installations/{installation_id}/access_tokens',
        headers=headers
    )
    return response.json()['token']
# Step 3: Use installation token for API calls
def create_check_run(token, owner, repo, name, head_sha):
    headers = {
        'Authorization': f'Bearer {token}',
        'Accept': 'application/vnd.github+json'
    }
    data = {
        'name': name,
        'head_sha': head_sha,
        'status': 'in_progress'
    }
    return requests.post(
        f'https://api.github.com/repos/{owner}/{repo}/check-runs',
        headers=headers,
        json=data
    )

```

### 2.5 Rate Limiting
Understanding rate limits is crucial for integration reliability:

#### Rate Limit Overview

| Authentication Type | Limit | Reset Period |
|--------------------|-------|--------------|
| Unauthenticated | 60/hour | Per IP |
| Personal Access Token | 5,000/hour | Per token |
| OAuth App | 5,000/hour | Per user |
| GitHub App Install | 5,000/hour | Per installation |
| GitHub App (server-to-server) | 15,000/hour | Per app |

#### Checking Rate Limits

```bash
# Check rate limit status
curl -H "Authorization: token YOUR_TOKEN" \
  https://api.github.com/rate_limit
# Response headers on every request
# X-RateLimit-Limit: 5000
# X-RateLimit-Remaining: 4999
# X-RateLimit-Reset: 1620000000 (Unix timestamp)

```

#### Rate Limit Best Practices

```python
import time
def api_call_with_retry(func, *args, max_retries=3, **kwargs):
    """Make API call with rate limit handling."""
    for attempt in range(max_retries):
        response = func(*args, **kwargs)
        
        # Check remaining rate limit
        remaining = int(response.headers.get('X-RateLimit-Remaining', 0))
        
        if remaining < 10:
            reset_time = int(response.headers.get('X-RateLimit-Reset', 0))
            sleep_time = max(reset_time - time.time(), 0) + 1
            print(f"Rate limit low. Sleeping {sleep_time} seconds")
            time.sleep(sleep_time)
        
        if response.status_code == 403 and 'rate limit' in response.text.lower():
            reset_time = int(response.headers.get('X-RateLimit-Reset', 0))
            sleep_time = max(reset_time - time.time(), 0) + 1
            print(f"Rate limited. Sleeping {sleep_time} seconds")
            time.sleep(sleep_time)
            continue
            
        return response
    
    raise Exception("Max retries exceeded")

```

{% endraw %}
---
<div class="section-nav">
  <a href="{{ page.prev_section.url }}" class="prev">← {{ page.prev_section.title }}</a>
  <a href="{{ page.next_section.url }}" class="next">{{ page.next_section.title }} →</a>
</div>
