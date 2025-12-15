---
layout: training-module
title: "Module 10, Section 4: Hands-On Labs"
description: "Practical exercises for API integrations and GitHub Apps"
permalink: /advanced/module-10/labs/
module_number: 10
section_number: 4
phase: advanced
prev_section:
  url: /advanced/module-10/walkthrough/
  title: "Guided Walkthrough"
next_section:
  url: /advanced/module-10/quiz/
  title: "Knowledge Check"
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

## Section 4: Hands-On Labs

### Lab 10.1: API Exploration
**Objective:** Familiarize yourself with GitHub's APIs using different tools.
**Tasks:**

1. **REST API with curl:**
   ```bash
   # Exercise 1: Get your user info
   curl -H "Authorization: token YOUR_TOKEN" \
     https://api.github.com/user
   
   # Exercise 2: List your repositories
   curl -H "Authorization: token YOUR_TOKEN" \
     https://api.github.com/user/repos?per_page=5
   
   # Exercise 3: Get repository details
   curl -H "Authorization: token YOUR_TOKEN" \
     https://api.github.com/repos/OWNER/REPO
   
   # Exercise 4: Create an issue (POST)
   curl -X POST \
     -H "Authorization: token YOUR_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"title": "Test Issue", "body": "Created via API"}' \
     https://api.github.com/repos/OWNER/REPO/issues
   ```

2. **GraphQL with GitHub CLI:**
   ```bash
   # Exercise 1: Simple query
   gh api graphql -f query='{ viewer { login name } }'
   
   # Exercise 2: Repository info
   gh api graphql -f query='
   query {
     repository(owner: "OWNER", name: "REPO") {
       name
       description
       stargazerCount
     }
   }'
   
   # Exercise 3: Organization members
   gh api graphql -f query='
   query {
     organization(login: "ORG") {
       membersWithRole(first: 10) {
         nodes {
           login
           name
         }
       }
     }
   }'
   ```

3. **Compare REST vs GraphQL:**
   ```markdown
   ## REST vs GraphQL Comparison
   
   ### Task: Get repository name, description, and open issue count
   
   #### REST Approach
   - Requests needed:
   - Data returned:
   - Rate limit impact:
   
   #### GraphQL Approach
   - Requests needed:
   - Data returned:
   - Rate limit impact:
   
   ### Conclusion
   [Your analysis]
   ```

### Lab 10.2: Webhook Design
**Objective:** Design a webhook integration for a common use case.
**Scenario:** A customer wants automatic notifications when:
- A pull request is opened
- A pull request is approved
- A deployment succeeds or fails
**Tasks:**

1. **Document Webhook Requirements:**
   ```markdown
   ## Webhook Integration Design
   
   ### Events to Subscribe

   | Event | Actions | Data Needed |
   |-------|---------|-------------|
   | pull_request | | |
   | pull_request_review | | |
   | deployment_status | | |
   
   ### Payload Processing
   For each event, document:
   - Key fields to extract
   - Notification format
   - Error handling
   ```

2. **Create Payload Handlers:**
   ```python
   # Pseudo-code for each event type
   
   def handle_pull_request(payload):
       """Process pull_request event."""
       # Your implementation plan
       pass
   
   def handle_pull_request_review(payload):
       """Process pull_request_review event."""
       # Your implementation plan
       pass
   
   def handle_deployment_status(payload):
       """Process deployment_status event."""
       # Your implementation plan
       pass
   ```

3. **Design Error Handling:**
   ```markdown
   ## Error Handling Strategy
   
   ### Webhook Delivery Failures
   - GitHub retries failed deliveries
   - Implement idempotency
   - Log all received events
   
   ### Processing Errors
   - Return 200 to acknowledge receipt
   - Queue for retry if needed
   - Alert on repeated failures
   ```

### Lab 10.3: GitHub App Planning
**Objective:** Design a GitHub App for a customer use case.
**Scenario:** A customer wants an app that:
- Enforces PR title format (e.g., must start with ticket number)
- Blocks merge if title doesn't match pattern
- Allows exceptions for specific users
**Tasks:**

1. **Define App Requirements:**
   ```markdown
   ## App Requirements Document
   
   ### Functional Requirements
   - [ ] Check PR title on PR open/edit
   - [ ] Create check run with pass/fail status
   - [ ] Support configurable title pattern
   - [ ] Support exception list
   
   ### Permissions Needed

   | Permission | Access | Reason |
   |------------|--------|--------|
   | | | |
   
   ### Events to Subscribe
   - [ ] pull_request
   - [ ] ?
   
   ### Configuration
   [How will users configure the app?]
   ```

2. **Design Configuration File:**
   ```yaml
   # .github/pr-title-checker.yml
   
   # Title pattern (regex)
   pattern: "^[A-Z]+-[0-9]+: .+"
   
   # Example: "JIRA-123: Add new feature"
   
   # Exempted users
   exempt_users:
     - dependabot[bot]
     - renovate[bot]
   
   # Custom error message
   error_message: |
     PR title must start with a JIRA ticket number.
     Example: JIRA-123: Your description here
   ```

3. **Sketch Implementation:**
   ```markdown
   ## Implementation Sketch
   
   ### Webhook Handler
   1. Receive pull_request event
   2. Load configuration from repo
   3. Check title against pattern
   4. Create/update check run
   
   ### Check Run Details
   - Name: "PR Title Check"
   - Status: completed
   - Conclusion: success/failure
   - Output: Title matched/didn't match pattern
   
   ### Branch Protection Integration
   - Users configure branch protection to require this check
   - PR cannot merge until check passes
   ```

### Lab 10.4: Integration Troubleshooting
**Objective:** Diagnose common API integration issues.
**Scenarios to troubleshoot:**

1. **Rate Limiting:**
   ```markdown
   ## Issue: API calls failing with 403
   
   ### Diagnosis Steps
   1. Check response headers for rate limit info
   2. Check if using authenticated requests
   3. Review request patterns
   
   ### Solutions
   - [ ] Implement exponential backoff
   - [ ] Cache responses where appropriate
   - [ ] Use conditional requests (ETags)
   - [ ] Consider using GraphQL for complex queries
   ```

2. **Webhook Not Receiving Events:**
   ```markdown
   ## Issue: Webhook not triggered
   
   ### Diagnosis Steps
   1. Check webhook configuration
   2. Verify URL is accessible
   3. Check recent deliveries in GitHub
   4. Verify event subscriptions
   
   ### Common Causes
   - [ ] URL not publicly accessible
   - [ ] Wrong events selected
   - [ ] Signature verification failing
   - [ ] Server returning non-2xx response
   ```

3. **GitHub App Authentication Failing:**
   ```markdown
   ## Issue: Installation token not working
   
   ### Diagnosis Steps
   1. Verify JWT generation
   2. Check private key format
   3. Verify installation ID
   4. Check token expiration
   
   ### Common Causes
   - [ ] Private key mismatch
   - [ ] JWT expired (10 min max)
   - [ ] Installation token expired (1 hour)
   - [ ] Wrong installation ID
   ```

{% endraw %}

