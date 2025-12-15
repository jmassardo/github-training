---
layout: training-module
title: "GitHub Discussions"
permalink: /beginner/module-3/discussions/
module_number: 3
module_title: "GitHub Platform Essentials"
section_number: 9
total_sections: 9
phase: beginner
estimated_time: "15 min"
module_index: /beginner/module-3/
sections:
  - title: "Context & Overview"
    url: "/beginner/module-3/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/beginner/module-3/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/beginner/module-3/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/beginner/module-3/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/beginner/module-3/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/beginner/module-3/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/beginner/module-3/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
  - title: "GitHub Codespaces"
    url: "/beginner/module-3/codespaces/"
    short_title: "Codespaces"
    icon: "☁️"
  - title: "GitHub Discussions"
    url: "/beginner/module-3/discussions/"
    short_title: "Discussions"
    icon: "💬"
prev_section:
  url: /beginner/module-3/codespaces/
  title: "GitHub Codespaces"
next_section:
  url: /beginner/module-4/
  title: "Module 4: SCM Process"
---

## What is GitHub Discussions?

**GitHub Discussions** is a collaborative communication forum built directly into GitHub repositories. It provides a space for community conversations that don't fit into Issues or Pull Requests.

> 💡 **Think of it as:** A Q&A forum + community space + announcement board, all integrated with your repository.

---

## Discussions vs. Issues

| Aspect | Issues | Discussions |
|--------|--------|-------------|
| **Purpose** | Track actionable work | Open-ended conversations |
| **Structure** | Linear comments | Threaded replies |
| **Resolution** | Close when fixed | Mark answer as accepted |
| **Best for** | Bugs, tasks, features | Questions, ideas, announcements |
| **Workflow** | Kanban, sprints | Community engagement |

### When to Use Each

**Use Issues for:**

- 🐛 Bug reports
- ✨ Feature requests with clear scope
- 📋 Trackable tasks
- 🔗 Work linked to code changes

**Use Discussions for:**

- ❓ Questions and support
- 💡 Ideas and brainstorming
- 📢 Announcements
- 🎉 Show and tell
- 📊 Polls and feedback

---

## Discussion Categories

Discussions are organized into categories that you can customize:

### Default Categories

| Category | Icon | Purpose |
|----------|------|---------|
| **Announcements** | 📣 | Official updates (maintainers only) |
| **General** | 💬 | Open conversations |
| **Ideas** | 💡 | Feature suggestions and brainstorming |
| **Polls** | 📊 | Community voting |
| **Q&A** | ❓ | Questions with accepted answers |
| **Show and Tell** | 🙌 | Share projects and accomplishments |

### Q&A Category Features

The Q&A category has special functionality:

- ✅ Mark an answer as **accepted**
- 🔝 Accepted answer pinned to top
- 🏷️ "Answered" label shows in list
- 🔍 Helps others find solutions quickly

---

## Anatomy of a Discussion

```
┌─────────────────────────────────────────────────────────────┐
│ 💬 Discussion #123                                          │
│ Category: Q&A                                               │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│ How do I configure GitHub Actions for monorepos?            │
│ ─────────────────────────────────────────────────           │
│ @developer · Posted 2 days ago                              │
│                                                             │
│ I'm trying to set up CI for our monorepo but only want      │
│ to run tests for changed packages...                        │
│                                                             │
│ 👍 12  ❤️ 3  🎉 1                         💬 5 replies      │
├─────────────────────────────────────────────────────────────┤
│ Replies                                                     │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ ✅ Accepted Answer                                      │ │
│ │ @maintainer · 1 day ago                                 │ │
│ │                                                         │ │
│ │ You can use path filters in your workflow...            │ │
│ │                                                         │ │
│ │ 👍 8  ❤️ 2                                              │ │
│ │   └─ 💬 2 replies                                       │ │
│ └─────────────────────────────────────────────────────────┘ │
│ ┌─────────────────────────────────────────────────────────┐ │
│ │ @contributor · 1 day ago                                │ │
│ │ Another approach is to use Nx or Turborepo...           │ │
│ └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
```

---

## Enabling Discussions

### For a Repository

1. Go to repository **Settings**
2. Scroll to **Features** section
3. Check **Discussions**
4. Click **Set up discussions** to create welcome post

### For an Organization

Discussions can be enabled at the organization level for cross-repo conversations:

1. Create a `.github` repository in your organization
2. Enable Discussions on that repository
3. This becomes your organization-wide discussion forum

---

## Managing Discussions

### Moderation Tools

| Action | Purpose | Who Can Do It |
|--------|---------|---------------|
| **Pin** | Highlight important discussions | Maintainers |
| **Lock** | Prevent new comments | Maintainers |
| **Transfer** | Move to another repo | Maintainers |
| **Convert to Issue** | Turn discussion into trackable work | Maintainers |
| **Delete** | Remove inappropriate content | Maintainers |
| **Mark as Answer** | Highlight correct response (Q&A) | Author, Maintainers |

### Converting Discussions to Issues

When a discussion reveals actionable work:

1. Open the discussion
2. Click **Create issue from discussion** in sidebar
3. The issue links back to the original discussion
4. Discussion can be closed or kept open for follow-up

---

## Best Practices

### For Maintainers

1. **Create a welcome discussion** - Set community expectations
2. **Use category descriptions** - Help users post in the right place
3. **Pin important discussions** - FAQs, roadmaps, announcements
4. **Respond promptly** - Even if just to acknowledge
5. **Convert to issues** - When discussions become actionable

### For Participants

1. **Search first** - Check if your question was already asked
2. **Choose the right category** - Q&A for questions, Ideas for suggestions
3. **Provide context** - Include relevant details and code
4. **Mark answers** - Help others find solutions
5. **Be respectful** - Follow the code of conduct

---

## Discussions Analytics

### Available Metrics

- 📊 **Total discussions** - Open and closed
- 💬 **Participation** - Unique contributors
- ⏱️ **Response time** - Average time to first reply
- ✅ **Answer rate** - Percentage of Q&A discussions answered
- 📈 **Trending** - Most active discussions

### Accessing Insights

1. Go to repository **Insights** tab
2. Select **Community** from sidebar
3. View discussion metrics and trends

---

## Discussions + GitHub Features

### Integration with Other Features

| Feature | Integration |
|---------|-------------|
| **Issues** | Convert discussions to issues |
| **Pull Requests** | Reference discussions in PRs |
| **Projects** | Add discussions to project boards |
| **Actions** | Automate responses to new discussions |
| **Search** | Discussions included in repo search |
| **Notifications** | Subscribe to categories or discussions |

### Automation with GitHub Actions

```yaml
# .github/workflows/discussion-welcome.yml
name: Welcome New Discussion

on:
  discussion:
    types: [created]

jobs:
  welcome:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/github-script@v7
        with:
          script: |
            github.rest.discussions.createComment({
              discussion_id: context.payload.discussion.node_id,
              body: 'Thanks for starting this discussion! A maintainer will respond soon.'
            })
```

---

## CSM Talking Points

### Value Propositions

**For Open Source Maintainers:**
> "Discussions reduces issue noise. Questions and ideas go to Discussions, keeping Issues focused on actionable bugs and features."

**For Enterprise Teams:**
> "Create a knowledge base that grows organically. Q&A discussions become searchable documentation for common questions."

**For Community Managers:**
> "Build community around your project. Discussions provides the forum experience users expect, without leaving GitHub."

**For Support Teams:**
> "Reduce support ticket volume. Users can search existing discussions and help each other, with maintainers stepping in when needed."

### Discovery Questions

- "How do you currently handle community questions about your projects?"
- "What percentage of your Issues are actually questions, not bugs?"
- "Do you have a separate forum or Discord for your community?"
- "How do new users learn best practices for your project?"
- "Is support ticket volume a concern for your team?"

### Common Objections & Responses

| Objection | Response |
|-----------|----------|
| "We already use Slack/Discord" | "Discussions is searchable and indexed. Conversations become permanent knowledge, not lost in chat history." |
| "It's just more noise" | "Categories and search help organize. Plus, community members can help each other, reducing maintainer burden." |
| "Our community is on Stack Overflow" | "Discussions keeps the community in your repo. No context-switching, and you control the experience." |

---

## Hands-On Exercise

### Enable and Explore (5 minutes)

1. Create a new repository (or use an existing one)
2. Go to **Settings** → **Features** → Enable **Discussions**
3. Click **Set up discussions**
4. Customize the welcome post
5. Create a test discussion in the Q&A category
6. Reply to your own discussion and mark it as answered

### Explore a Live Example

Visit these repositories to see Discussions in action:

- [vercel/next.js](https://github.com/vercel/next.js/discussions) - Large community Q&A
- [github/docs](https://github.com/github/docs/discussions) - Documentation feedback
- [microsoft/vscode](https://github.com/microsoft/vscode/discussions) - Feature ideas

---

## Key Takeaways

1. **Discussions ≠ Issues** - Use Discussions for conversations, Issues for work
2. **Categories matter** - Q&A, Ideas, Announcements serve different purposes
3. **Answers are powerful** - Q&A category creates searchable knowledge base
4. **Moderation tools** - Pin, lock, transfer, convert to keep things organized
5. **Community building** - Discussions foster engagement beyond code contributions

---

## Additional Resources

- 📖 [GitHub Discussions Documentation](https://docs.github.com/en/discussions)
- 🎓 [GitHub Skills: Community Discussions](https://github.com/skills/community-discussions)
- 📝 [Best Practices for Community Conversations](https://docs.github.com/en/discussions/guides/best-practices-for-community-conversations-on-github)
- 🔧 [Managing Discussions](https://docs.github.com/en/discussions/managing-discussions-for-your-community)
