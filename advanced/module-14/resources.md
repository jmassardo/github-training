---
layout: training-module
title: "Resources"
permalink: /advanced/module-14/resources/
module_number: 14
module_title: "GitHub Copilot Advanced"
section_number: 10
total_sections: 10
phase: advanced
estimated_time: "10 min"
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
  url: /advanced/module-14/quiz/
  title: "Knowledge Check"
---

## Documentation & Learning Resources

This section provides curated links to official documentation, tools, and learning materials for GitHub Copilot advanced features.

---

## Official Documentation

### Core Copilot

| Resource | Description |
|----------|-------------|
| [GitHub Copilot Documentation](https://docs.github.com/en/copilot) | Complete official documentation |
| [Copilot Features Overview](https://docs.github.com/en/copilot/about-github-copilot/github-copilot-features) | Feature comparison and capabilities |
| [Getting Started Guide](https://docs.github.com/en/copilot/using-github-copilot/getting-code-suggestions-in-your-ide-with-github-copilot) | IDE setup and basic usage |
| [Best Practices](https://docs.github.com/en/copilot/using-github-copilot/best-practices-for-using-github-copilot) | Tips for effective usage |

### Copilot Chat

| Resource | Description |
|----------|-------------|
| [Chat Overview](https://docs.github.com/en/copilot/github-copilot-chat/about-github-copilot-chat) | Chat capabilities and usage |
| [Chat in Your IDE](https://docs.github.com/en/copilot/github-copilot-chat/using-github-copilot-chat-in-your-ide) | VS Code and JetBrains usage |
| [Chat on GitHub.com](https://docs.github.com/en/copilot/github-copilot-chat/using-github-copilot-chat-on-githubcom) | Web interface features |
| [Chat Slash Commands](https://docs.github.com/en/copilot/github-copilot-chat/copilot-chat-slash-commands) | Command reference |

### Enterprise & Administration

| Resource | Description |
|----------|-------------|
| [Enterprise Overview](https://docs.github.com/en/copilot/copilot-enterprise-overview) | Enterprise features and capabilities |
| [Managing Copilot](https://docs.github.com/en/copilot/managing-github-copilot-in-your-organization) | Organization management |
| [Policy Configuration](https://docs.github.com/en/copilot/managing-copilot-for-business/managing-policies-for-copilot-for-business) | Policy settings reference |
| [Audit Log Events](https://docs.github.com/en/organizations/keeping-your-organization-secure/managing-security-settings-for-your-organization/reviewing-the-audit-log-for-your-organization) | Audit logging guide |

---

## Advanced Features

### Copilot Agents

| Resource | Description |
|----------|-------------|
| [Coding Agent Overview](https://docs.github.com/en/copilot/using-github-copilot/using-copilot-coding-agent) | Agent capabilities and usage |
| [Agent Best Practices](https://docs.github.com/en/copilot/using-github-copilot/using-copilot-coding-agent/best-practices-for-copilot-coding-agent) | Optimization tips |
| [Workspace Agent](https://docs.github.com/en/copilot/github-copilot-chat/copilot-chat-in-your-ide/using-github-copilot-chat-in-your-ide#using-participants-in-copilot-chat) | @workspace usage |

### Model Context Protocol (MCP)

| Resource | Description |
|----------|-------------|
| [MCP Specification](https://modelcontextprotocol.io/) | Official protocol documentation |
| [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk) | TypeScript server SDK |
| [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk) | Python server SDK |
| [Example MCP Servers](https://github.com/modelcontextprotocol/servers) | Reference implementations |
| [MCP Inspector](https://github.com/modelcontextprotocol/inspector) | Testing and debugging tool |

### Copilot Extensions

| Resource | Description |
|----------|-------------|
| [Building Extensions](https://docs.github.com/en/copilot/building-copilot-extensions) | Extension development guide |
| [Extension SDK (JavaScript)](https://github.com/copilot-extensions/preview-sdk.js) | JavaScript/TypeScript SDK |
| [Extension Marketplace](https://github.com/marketplace?type=apps&copilot_app=true) | Available extensions |
| [Skillsets Reference](https://docs.github.com/en/copilot/building-copilot-extensions/building-a-copilot-skillset) | Skillset development |

### Custom Instructions

| Resource | Description |
|----------|-------------|
| [Custom Instructions Guide](https://docs.github.com/en/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot) | Configuration reference |
| [Repository Instructions](https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot) | Repository-level setup |
| [Knowledge Bases](https://docs.github.com/en/copilot/customizing-copilot/managing-copilot-knowledge-bases) | Enterprise knowledge bases |

---

## APIs & Integration

### REST API

```bash
# Key API endpoints
GET  /orgs/{org}/copilot/billing           # Billing/seat summary
GET  /orgs/{org}/copilot/billing/seats     # Individual seats
GET  /orgs/{org}/copilot/usage             # Usage metrics
POST /orgs/{org}/copilot/billing/selected_users  # Assign seats

```

| Resource | Description |
|----------|-------------|
| [Copilot REST API](https://docs.github.com/en/rest/copilot) | Complete API reference |
| [Rate Limits](https://docs.github.com/en/rest/rate-limit) | API rate limiting |
| [Authentication](https://docs.github.com/en/rest/authentication) | API auth methods |

### GitHub CLI

```bash
# Useful Copilot CLI commands
gh copilot explain "command"    # Explain a command
gh copilot suggest "task"       # Get command suggestions
gh api /orgs/{org}/copilot/...  # API access

```

| Resource | Description |
|----------|-------------|
| [GitHub CLI](https://cli.github.com/) | CLI installation and docs |
| [Copilot in CLI](https://docs.github.com/en/copilot/github-copilot-in-the-cli) | CLI-specific features |

---

## Learning Resources

### Official Training

| Resource | Description |
|----------|-------------|
| [GitHub Skills](https://skills.github.com/) | Interactive courses |
| [GitHub Certifications](https://resources.github.com/learn/certifications/) | Professional certification |
| [GitHub Universe](https://githubuniverse.com/) | Annual conference |

### Community

| Resource | Description |
|----------|-------------|
| [GitHub Blog](https://github.blog/category/engineering/) | Engineering announcements |
| [GitHub Changelog](https://github.blog/changelog/) | Feature updates |
| [GitHub Community](https://github.community/) | Discussion forums |
| [Stack Overflow](https://stackoverflow.com/questions/tagged/github-copilot) | Q&A |

### Third-Party Resources

| Resource | Description |
|----------|-------------|
| [VS Code Copilot Guide](https://code.visualstudio.com/docs/copilot/overview) | VS Code documentation |
| [Prompt Engineering Guide](https://www.promptingguide.ai/) | General prompt techniques |

---

## Tools & Extensions

### VS Code Extensions

| Extension | Purpose |
|-----------|---------|
| [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) | Core functionality |
| [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) | Chat interface |
| [GitHub Actions](https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-github-actions) | Workflow integration |
| [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) | Git insights |

### MCP Development Tools

| Tool | Purpose |
|------|---------|
| @modelcontextprotocol/sdk | Server development SDK |
| @modelcontextprotocol/inspector | Server testing |
| ts-node | TypeScript execution |
| zod | Schema validation |

---

## Security & Compliance

### Trust & Privacy

| Resource | Description |
|----------|-------------|
| [GitHub Trust Center](https://github.com/trust) | Security practices |
| [Copilot Trust Center](https://docs.github.com/en/site-policy/privacy-policies/github-copilot-trust-center) | Copilot-specific privacy |
| [Data Protection Agreement](https://github.com/customer-terms) | GDPR compliance |
| [SOC 2 Report](https://github.com/security) | Compliance certification |

### Data Handling

- Code is **NOT** used for model training
- Code is **NOT** stored after processing
- Prompts retained for limited time (configurable for Enterprise)
- All data encrypted in transit
- SOC 2 Type 2 certified

---

## Troubleshooting

### Common Issues

| Issue | Resource |
|-------|----------|
| Copilot not working | [Troubleshooting Guide](https://docs.github.com/en/copilot/troubleshooting-github-copilot) |
| Network/proxy issues | [Firewall Configuration](https://docs.github.com/en/copilot/troubleshooting-github-copilot/troubleshooting-firewall-settings-for-github-copilot) |
| Extension issues | [VS Code Logs](https://code.visualstudio.com/docs/copilot/copilot-troubleshooting) |

### Support Channels

| Channel | Use Case |
|---------|----------|
| [GitHub Status](https://www.githubstatus.com/) | Service outages |
| [GitHub Support](https://support.github.com/) | Account/billing issues |
| [Community Forum](https://github.community/) | General questions |

---

## What's Next?

### Continue Your Learning

- Explore other modules in this training program
- Practice with the hands-on labs
- Build a custom MCP server or extension
- Share your learnings with your team

### Stay Updated

- Follow [@GitHubCopilot](https://twitter.com/GitHubCopilot) on Twitter/X
- Subscribe to the [GitHub Blog](https://github.blog/)
- Watch the [GitHub Changelog](https://github.blog/changelog/)
- Attend [GitHub Universe](https://githubuniverse.com/)

### Contribute

- Share feedback with GitHub through the product
- Build and share custom extensions
- Help teammates adopt and optimize Copilot
- Contribute to open-source MCP servers

---

## Module Complete! 🎉

Congratulations on completing **Module 14: GitHub Copilot Advanced**!

### Key Takeaways

1. **Copilot Agents** enable autonomous development workflows from issues to pull requests
2. **Model Context Protocol** connects Copilot to custom tools and external data sources
3. **Copilot Extensions** add specialized chat capabilities through @mentions
4. **Custom Instructions** shape Copilot behavior for your team's patterns
5. **Enterprise Administration** enables organization-wide policy and access control
6. **Metrics** drive continuous improvement and demonstrate ROI

### Recommended Next Steps

1. **Practice**: Complete any remaining hands-on labs
2. **Apply**: Use advanced features in your daily work
3. **Share**: Help teammates leverage these capabilities
4. **Explore**: Build custom integrations for your team

---

### Return to Training

- [Module 14 Index]({{ site.baseurl }}/advanced/module-14/)
- [Training Home](/)

---

*Thank you for completing this module!*
