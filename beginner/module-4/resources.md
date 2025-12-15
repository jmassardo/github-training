---
layout: training-module
title: "Resources"
permalink: /beginner/module-4/resources/
module_number: 4
module_title: "SCM Process"
section_number: 6
total_sections: 7
phase: beginner
estimated_time: "10 min"
module_index: /beginner/module-4/
sections:
  - title: "Context & Overview"
    url: "/beginner/module-4/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/beginner/module-4/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/beginner/module-4/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/beginner/module-4/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/beginner/module-4/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/beginner/module-4/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/beginner/module-4/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
toc: true
prev_section:
  url: /beginner/module-4/quiz/
  title: "Knowledge Check"
next_section:
  url: /beginner/module-4/scenarios/
  title: "CSM Scenarios"
---

# Resources

## Official Documentation
- [Managing branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-branches-in-your-repository)
- [Branch protection rules](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/about-protected-branches)
- [Repository rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets)
- [CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)
- [Commit signature verification](https://docs.github.com/en/authentication/managing-commit-signature-verification)
- [Managing releases](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository)

## Best Practices Guides
- [GitHub Flow](https://docs.github.com/en/get-started/quickstart/github-flow)
- [Trunk-Based Development](https://trunkbaseddevelopment.com/)
- [Git Flow](https://nvie.com/posts/a-successful-git-branching-model/)
- [Semantic Versioning](https://semver.org/)

## GitHub CLI Commands

```bash
# Branch protection
gh api repos/{owner}/{repo}/branches/{branch}/protection

# Rulesets
gh api repos/{owner}/{repo}/rulesets

# Releases
gh release list
gh release create
gh release view

# CODEOWNERS validation
gh api repos/{owner}/{repo}/codeowners/errors

```

## Third-Party Tools
- [Allstar](https://github.com/ossf/allstar) - Policy enforcement for GitHub orgs
- [GitHub Terraform Provider](https://registry.terraform.io/providers/integrations/github/latest) - IaC for GitHub
- [Probot](https://probot.github.io/) - GitHub Apps framework
