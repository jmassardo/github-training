---
layout: training-module
title: "Best Practices"
permalink: /advanced/module-13/best-practices/
module_number: 13
module_title: "GitHub Copilot Fundamentals"
section_number: 7
total_sections: 10
phase: advanced
estimated_time: "25 min"
module_index: /advanced/module-13/
sections:
  - title: "Context & Overview"
    url: "/advanced/module-13/overview/"
    short_title: "Overview"
    icon: "📋"
  - title: "Core Concepts"
    url: "/advanced/module-13/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "IDE Setup"
    url: "/advanced/module-13/setup/"
    short_title: "Setup"
    icon: "⚙️"
  - title: "Prompt Crafting"
    url: "/advanced/module-13/prompts/"
    short_title: "Prompts"
    icon: "✍️"
  - title: "Copilot Chat"
    url: "/advanced/module-13/chat/"
    short_title: "Chat"
    icon: "💬"
  - title: "IDE Differences"
    url: "/advanced/module-13/ide-differences/"
    short_title: "IDE Differences"
    icon: "🔀"
  - title: "Best Practices"
    url: "/advanced/module-13/best-practices/"
    short_title: "Best Practices"
    icon: "✅"
  - title: "Hands-On Labs"
    url: "/advanced/module-13/labs/"
    short_title: "Labs"
    icon: "🧪"
  - title: "Knowledge Check"
    url: "/advanced/module-13/quiz/"
    short_title: "Quiz"
    icon: "📝"
  - title: "Resources"
    url: "/advanced/module-13/resources/"
    short_title: "Resources"
    icon: "📖"
toc: true
prev_section:
  url: /advanced/module-13/ide-differences/
  title: "IDE Differences"
next_section:
  url: /advanced/module-13/labs/
  title: "Hands-On Labs"
---

## The Copilot Mindset

Success with Copilot requires a shift in how you approach coding. Think of Copilot as a knowledgeable pair programmer—helpful, but requiring your oversight and guidance.

### The Human-AI Partnership

<div class="mermaid-container">
<div class="mermaid">
flowchart LR
    A[You: Intent & Design] --> B[Copilot: Implementation]
    B --> C[You: Review & Refine]
    C --> D[Together: Quality Code]
    
    style A fill:#0969da,color:#fff
    style B fill:#8250df,color:#fff
    style C fill:#0969da,color:#fff
    style D fill:#1a7f37,color:#fff
</div>
</div>

**Your Role:**

- Define what needs to be built
- Provide clear context and requirements
- Review all suggestions critically
- Ensure code meets quality standards

**Copilot's Role:**

- Generate implementation options
- Handle boilerplate and patterns
- Suggest solutions to common problems
- Accelerate routine coding tasks

---

## Code Quality Best Practices

### Always Review Before Accepting

Never blindly accept suggestions. Every `Tab` should be a conscious decision.

| Review Checklist | Why It Matters |
|------------------|----------------|
| Does it do what I need? | Copilot may solve a different problem |
| Is it correct? | AI can generate plausible but wrong code |
| Is it efficient? | May not be the optimal solution |
| Is it secure? | Could introduce vulnerabilities |
| Does it match our style? | Consistency matters |

### Test Generated Code

```python
# Copilot generated this function
def find_duplicates(items: list) -> list:
    seen = set()
    duplicates = []
    for item in items:
        if item in seen:
            duplicates.append(item)
        seen.add(item)
    return duplicates

# YOU should add tests
def test_find_duplicates():
    assert find_duplicates([1, 2, 2, 3]) == [2]
    assert find_duplicates([]) == []
    assert find_duplicates([1, 1, 1]) == [1, 1]  # Note: multiple duplicates
    assert find_duplicates(["a", "b", "a"]) == ["a"]

```

### Security Considerations

> **⚠️ Warning:** Copilot is not a security tool. Always review generated code for vulnerabilities.

**Common Security Issues to Watch For:**

| Issue | Example | What to Check |
|-------|---------|---------------|
| **SQL Injection** | String concatenation in queries | Use parameterized queries |
| **XSS** | Unescaped user input in HTML | Sanitize output |
| **Hardcoded Secrets** | API keys in code | Use environment variables |
| **Path Traversal** | User-controlled file paths | Validate and sanitize paths |
| **Insecure Defaults** | `verify=False` in requests | Enable security features |

---

## Productivity Best Practices

### Know When to Use Copilot

| Great for Copilot | Better Done Manually |
|-------------------|---------------------|
| Boilerplate code | Critical security code |
| Unit tests | Complex business logic |
| Documentation | Architecture decisions |
| Data transformations | Performance-critical code |
| API implementations | Debugging existing issues |

### Maximize Context Quality

**Good Context Setup:**

```python
# File: user_service.py
# Part of the authentication module
# Uses PostgreSQL for storage

from dataclasses import dataclass
from datetime import datetime
from typing import Optional

@dataclass
class User:
    id: int
    email: str
    created_at: datetime
    last_login: Optional[datetime] = None

# Copilot now understands your data model
def find_inactive_users(days_threshold: int = 30) -> list[User]:
    """Find users who haven't logged in for the specified number of days."""
    # Great suggestions incoming!

```

### Use Incremental Development

Build up complexity gradually:

```python
# Step 1: Basic function
def send_notification(user, message):
    pass

# Step 2: Add details with comment
# Send notification via email, with retry logic

# Step 3: Accept and refine suggestion
def send_notification(user, message, max_retries=3):
    for attempt in range(max_retries):
        try:
            # Copilot's implementation
            return True
        except Exception as e:
            if attempt == max_retries - 1:
                raise
            time.sleep(2 ** attempt)

# Step 4: Add what Copilot missed
# Add logging, metrics, etc.

```

---

## Team and Organization Practices

### Establish Team Guidelines

Create shared expectations for Copilot usage:

```markdown
# Team Copilot Guidelines

## DO
- Use Copilot for boilerplate and tests
- Review all suggestions before accepting
- Add comments explaining complex generated code
- Report false positives or security issues

## DON'T
- Accept suggestions without understanding them
- Use Copilot for security-critical code without extra review
- Rely on Copilot for architectural decisions
- Share internal code patterns outside the team

```

### Code Review Considerations

When reviewing code that used Copilot:

- Focus on correctness and security
- Don't assume AI-generated code is error-free
- Check for edge cases the AI might have missed
- Verify that the solution matches the requirement

### Onboarding New Developers

Teach Copilot usage alongside coding standards:

1. **Start with basics** - Show how completions work
2. **Demonstrate Chat** - Explain code queries
3. **Practice prompt crafting** - Good habits early
4. **Review together** - Model critical evaluation
5. **Discuss limitations** - Set realistic expectations

---

## Common Pitfalls and How to Avoid Them

### Pitfall 1: Over-Reliance

**Symptom:** Accepting every suggestion without thought

**Solution:** 
- Read every suggestion before accepting
- Ask yourself: "Would I write this myself?"
- If unsure, reject and try a different approach

### Pitfall 2: Context Confusion

**Symptom:** Suggestions are irrelevant or wrong

**Solution:**

- Check that the right files are open
- Add clarifying comments
- Break complex tasks into smaller pieces

### Pitfall 3: Outdated Patterns

**Symptom:** Copilot suggests deprecated APIs or old practices

**Solution:**

- Verify against official documentation
- Add comments specifying versions/standards
- Report significant issues to GitHub

### Pitfall 4: Copy-Paste Programming

**Symptom:** Using AI as a crutch without learning

**Solution:**

- Use `/explain` to understand generated code
- Modify suggestions to learn how they work
- Implement simple things yourself for practice

---

## Measuring Success

### Personal Metrics

Track your own Copilot effectiveness:

| Metric | How to Measure | Target |
|--------|----------------|--------|
| Acceptance Rate | Suggestions accepted / shown | 30-50% (quality over quantity) |
| Time to Task | Before and after Copilot | Improvement over time |
| Bug Rate | Bugs in AI vs manual code | No increase |
| Learning | New patterns discovered | Regular discoveries |

### Team Metrics

For organizations tracking Copilot ROI:

| Metric | Description |
|--------|-------------|
| **Developer Satisfaction** | Survey scores for productivity tools |
| **PR Velocity** | Time from first commit to merge |
| **Code Churn** | How often AI-generated code is changed |
| **Security Findings** | Vulnerabilities in AI-assisted PRs |

> **📚 Learn More:** [Copilot Metrics](https://docs.github.com/en/copilot/rolling-out-github-copilot-at-scale/analyzing-copilot-usage-in-your-organization/analyzing-usage-data-for-github-copilot)

---

## Best Practices Checklist

### Before You Start

- [ ] Open relevant files for context
- [ ] Have clear requirements in mind
- [ ] Know what pattern you're implementing

### While Coding

- [ ] Write descriptive comments first
- [ ] Use meaningful variable/function names
- [ ] Review each suggestion critically
- [ ] Reject suggestions that don't fit

### After Accepting

- [ ] Test the generated code
- [ ] Add error handling if missing
- [ ] Document complex logic
- [ ] Check for security issues

### For Teams

- [ ] Establish shared guidelines
- [ ] Include Copilot in code review discussions
- [ ] Share learnings and patterns
- [ ] Track metrics over time

---

## Summary

| Principle | Action |
|-----------|--------|
| **Review Everything** | Never accept without understanding |
| **Context Matters** | Better context = better suggestions |
| **Test Generated Code** | AI isn't bug-free |
| **Security First** | Extra scrutiny for sensitive code |
| **Learn Continuously** | Use Copilot to grow, not just speed up |

---

## Next Steps

Time to put these best practices into action with hands-on labs.
