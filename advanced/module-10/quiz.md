---
layout: training-module
title: "Module 10, Section 5: Knowledge Check"
description: "Test your understanding of GitHub APIs and Apps"
permalink: /advanced/module-10/quiz/
module_number: 10
section_number: 5
phase: advanced
prev_section:
  url: /advanced/module-10/labs/
  title: "Hands-On Labs"
next_section:
  url: /advanced/module-10/resources/
  title: "Resources"
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

## Section 5: Knowledge Check

### Quiz Questions
**Question 1:** What is the main advantage of GraphQL over REST for GitHub API usage?
A) GraphQL is faster  
B) GraphQL requires no authentication  
C) GraphQL allows fetching exactly the data needed in one request  
D) GraphQL has no rate limits
**Question 2:** What is the maximum lifetime of a GitHub App JWT?
A) 1 minute  
B) 10 minutes  
C) 1 hour  
D) 24 hours
**Question 3:** How should you verify that a webhook payload came from GitHub?
A) Check the IP address  
B) Validate the X-Hub-Signature-256 header using your webhook secret  
C) Check the User-Agent header  
D) Verify the payload format
**Question 4:** What is the rate limit for authenticated REST API requests?
A) 60 requests per hour  
B) 1,000 requests per hour  
C) 5,000 requests per hour  
D) Unlimited
**Question 5:** Which authentication method is recommended for automation that needs to act on behalf of an installation?
A) Personal Access Token  
B) OAuth App  
C) GitHub App  
D) Basic Authentication
**Question 6:** What HTTP status code indicates rate limiting?
A) 401  
B) 403  
C) 429  
D) 500
**Question 7:** Which header should your webhook server check to determine the event type?
A) Content-Type  
B) X-GitHub-Event  
C) X-Hub-Signature  
D) Authorization
**Question 8:** What is the correct way to paginate REST API results?
A) Use offset parameter  
B) Use page and per_page parameters  
C) Use cursor parameter  
D) Use limit and skip parameters

**Question 9:** When building a GitHub App, what determines which repositories the app can access?
A) The permissions requested at app creation  
B) Where the app is installed and the permissions granted  
C) The app's client ID  
D) The app owner's permissions

**Question 10:** What is a key advantage of using a GitHub App over OAuth Apps for automation?
A) GitHub Apps don't require any configuration  
B) GitHub Apps have higher rate limits and don't consume a user's rate limit  
C) GitHub Apps work without webhooks  
D) GitHub Apps can only be installed organization-wide

### Answer Key
1. **C** - GraphQL allows precise data fetching, reducing over-fetching and multiple requests
2. **B** - GitHub App JWTs can be valid for up to 10 minutes
3. **B** - Use HMAC-SHA256 with your secret to verify the signature header
4. **C** - Authenticated requests are limited to 5,000 per hour
5. **C** - GitHub Apps provide installation-based authentication with granular permissions
6. **B** - GitHub returns 403 for rate limiting (check response body for details)
7. **B** - X-GitHub-Event header contains the event type
8. **B** - Use page and per_page query parameters for pagination
9. **B** - App access depends on installation location and the permissions granted during installation
10. **B** - GitHub Apps get 5,000 requests per installation, separate from user quotas

---

## Self-Assessment

After completing this quiz, assess your understanding:

- **9-10 correct**: Excellent! You have a strong grasp of GitHub APIs and Apps.
- **7-8 correct**: Good understanding. Review the concepts you missed.
- **5-6 correct**: Fair understanding. Consider reviewing the Core Concepts section.
- **Below 5**: Review the entire module before proceeding.

{% endraw %}
<div class="section-nav">
  <a href="{{ '/advanced/module-10/labs/' | relative_url }}" class="prev">← Hands-On Labs</a>
  <a href="{{ '/advanced/module-10/resources/' | relative_url }}" class="next">Resources →</a>
</div>

