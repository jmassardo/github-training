---
layout: training-module
title: "Module 10, Section 3: Guided Walkthrough"
description: "Step-by-step guides for building API integrations and GitHub Apps"
permalink: /advanced/module-10/walkthrough/
module_number: 10
section_number: 3
phase: advanced
prev_section:
  url: /advanced/module-10/concepts/
  title: "Core Concepts"
next_section:
  url: /advanced/module-10/labs/
  title: "Hands-On Labs"
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

## Section 3: Guided Walkthrough

### Walkthrough 3.1: Building a Repository Dashboard with GraphQL
Create a dashboard showing organization repository metrics:

#### Step 1: Define the Query

```graphql
query OrgDashboard($org: String!, $first: Int!) {
  organization(login: $org) {
    name
    repositories(first: $first, orderBy: {field: UPDATED_AT, direction: DESC}) {
      totalCount
      nodes {
        name
        description
        isPrivate
        stargazerCount
        forkCount
        updatedAt
        primaryLanguage {
          name
          color
        }
        defaultBranchRef {
          target {
            ... on Commit {
              history(first: 1) {
                nodes {
                  committedDate
                  author {
                    user {
                      login
                    }
                  }
                }
              }
            }
          }
        }
        pullRequests(states: OPEN) {
          totalCount
        }
        issues(states: OPEN) {
          totalCount
        }
      }
    }
  }
}

```

#### Step 2: Execute the Query (Python)

```python
import requests
def fetch_org_dashboard(org_name, token):
    query = """
    query OrgDashboard($org: String!, $first: Int!) {
      organization(login: $org) {
        # ... full query from above
      }
    }
    """
    
    variables = {
        "org": org_name,
        "first": 50
    }
    
    response = requests.post(
        'https://api.github.com/graphql',
        headers={
            'Authorization': f'Bearer {token}',
            'Content-Type': 'application/json'
        },
        json={'query': query, 'variables': variables}
    )
    
    return response.json()
# Generate dashboard report
def generate_report(data):
    org = data['data']['organization']
    repos = org['repositories']['nodes']
    
    print(f"Organization: {org['name']}")
    print(f"Total Repositories: {org['repositories']['totalCount']}")
    print("\n" + "="*60)
    
    for repo in repos:
        print(f"\n{repo['name']}")
        print(f"  Language: {repo['primaryLanguage']['name'] if repo['primaryLanguage'] else 'N/A'}")
        print(f"  Stars: {repo['stargazerCount']} | Forks: {repo['forkCount']}")
        print(f"  Open PRs: {repo['pullRequests']['totalCount']}")
        print(f"  Open Issues: {repo['issues']['totalCount']}")

```

### Walkthrough 3.2: Setting Up Webhooks for Slack Notifications
Create a webhook that sends Slack notifications on PR events:

#### Step 1: Create Webhook Server

```python
from flask import Flask, request, jsonify
import hmac
import hashlib
import requests
import os
app = Flask(__name__)
WEBHOOK_SECRET = os.environ['GITHUB_WEBHOOK_SECRET']
SLACK_WEBHOOK_URL = os.environ['SLACK_WEBHOOK_URL']
def verify_signature(payload, signature):
    """Verify GitHub webhook signature."""
    expected = 'sha256=' + hmac.new(
        WEBHOOK_SECRET.encode(),
        payload,
        hashlib.sha256
    ).hexdigest()
    return hmac.compare_digest(expected, signature)
def send_slack_notification(message):
    """Send notification to Slack."""
    requests.post(SLACK_WEBHOOK_URL, json={'text': message})
@app.route('/webhook', methods=['POST'])
def webhook():
    # Verify signature
    signature = request.headers.get('X-Hub-Signature-256', '')
    if not verify_signature(request.data, signature):
        return 'Invalid signature', 403
    
    event_type = request.headers.get('X-GitHub-Event')
    payload = request.json
    
    # Handle pull request events
    if event_type == 'pull_request':
        action = payload['action']
        pr = payload['pull_request']
        repo = payload['repository']['full_name']
        
        if action == 'opened':
            message = f"🆕 New PR in {repo}: [{pr['title']}]({pr['html_url']}) by {pr['user']['login']}"
            send_slack_notification(message)
        
        elif action == 'closed' and pr['merged']:
            message = f"✅ PR merged in {repo}: [{pr['title']}]({pr['html_url']})"
            send_slack_notification(message)
        
        elif action == 'closed' and not pr['merged']:
            message = f"❌ PR closed without merge in {repo}: [{pr['title']}]({pr['html_url']})"
            send_slack_notification(message)
    
    return jsonify({'status': 'ok'})
if __name__ == '__main__':
    app.run(port=3000)

```

#### Step 2: Configure Webhook in GitHub
1. Navigate to repository **Settings** → **Webhooks** → **Add webhook**
2. Configure:
   - **Payload URL**: `https://your-server.com/webhook`
   - **Content type**: `application/json`
   - **Secret**: Generate a secure secret
   - **Events**: Select "Pull requests"

#### Step 3: Test the Webhook

```bash
# GitHub provides test delivery
# Go to webhook settings and click "Recent Deliveries"
# Use "Redeliver" to test with past events

```

### Walkthrough 3.3: Building a Simple GitHub App
Create a GitHub App that auto-labels issues:

#### Step 1: Register the App
1. Go to **Settings** → **Developer settings** → **GitHub Apps** → **New GitHub App**
2. Configure:
   - **Name**: `issue-labeler`
   - **Homepage URL**: Your documentation URL
   - **Webhook URL**: `https://your-server.com/webhook`
   - **Webhook secret**: Generate secure secret
   - **Permissions**:
     - Issues: Read & Write
   - **Events**: Issues
3. Generate and download private key

#### Step 2: Implement the App

```python
from flask import Flask, request
import jwt
import time
import requests
import hmac
import hashlib
import os
app = Flask(__name__)
APP_ID = os.environ['GITHUB_APP_ID']
PRIVATE_KEY = os.environ['GITHUB_PRIVATE_KEY']
WEBHOOK_SECRET = os.environ['GITHUB_WEBHOOK_SECRET']
def generate_jwt():
    """Generate JWT for GitHub App authentication."""
    payload = {
        'iat': int(time.time()),
        'exp': int(time.time()) + 600,
        'iss': APP_ID
    }
    return jwt.encode(payload, PRIVATE_KEY, algorithm='RS256')
def get_installation_token(installation_id):
    """Get installation access token."""
    jwt_token = generate_jwt()
    response = requests.post(
        f'https://api.github.com/app/installations/{installation_id}/access_tokens',
        headers={
            'Authorization': f'Bearer {jwt_token}',
            'Accept': 'application/vnd.github+json'
        }
    )
    return response.json()['token']
def add_label(token, owner, repo, issue_number, label):
    """Add label to an issue."""
    requests.post(
        f'https://api.github.com/repos/{owner}/{repo}/issues/{issue_number}/labels',
        headers={
            'Authorization': f'Bearer {token}',
            'Accept': 'application/vnd.github+json'
        },
        json={'labels': [label]}
    )
def categorize_issue(title, body):
    """Determine label based on issue content."""
    content = (title + ' ' + (body or '')).lower()
    
    if 'bug' in content or 'error' in content or 'broken' in content:
        return 'bug'
    elif 'feature' in content or 'enhancement' in content or 'request' in content:
        return 'enhancement'
    elif 'docs' in content or 'documentation' in content:
        return 'documentation'
    elif 'question' in content or 'help' in content:
        return 'question'
    else:
        return 'triage'
@app.route('/webhook', methods=['POST'])
def webhook():
    # Verify signature
    signature = request.headers.get('X-Hub-Signature-256', '')
    expected = 'sha256=' + hmac.new(
        WEBHOOK_SECRET.encode(),
        request.data,
        hashlib.sha256
    ).hexdigest()
    
    if not hmac.compare_digest(expected, signature):
        return 'Invalid signature', 403
    
    event_type = request.headers.get('X-GitHub-Event')
    payload = request.json
    
    if event_type == 'issues' and payload['action'] == 'opened':
        issue = payload['issue']
        repo = payload['repository']
        installation_id = payload['installation']['id']
        
        # Get label based on content
        label = categorize_issue(issue['title'], issue['body'])
        
        # Get installation token and add label
        token = get_installation_token(installation_id)
        add_label(
            token,
            repo['owner']['login'],
            repo['name'],
            issue['number'],
            label
        )
        
        print(f"Added '{label}' label to issue #{issue['number']}")
    
    return 'ok'
if __name__ == '__main__':
    app.run(port=3000)

```

#### Step 3: Install the App
1. Navigate to your app's public page
2. Click "Install" and select organizations/repositories
3. Test by creating a new issue
{% endraw %}
