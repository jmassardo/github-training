---
layout: training-module
title: "Context & Overview"
permalink: /beginner/module-2/overview/
module_number: 2
module_title: "Git Fundamentals & Version Control"
section_number: 1
total_sections: 7
phase: beginner
estimated_time: "15 min"
module_index: /beginner/module-2/
sections:
  - title: "Context & Overview"
    url: "/beginner/module-2/overview/"
    short_title: "Overview"
    icon: "🎯"
  - title: "Core Concepts"
    url: "/beginner/module-2/concepts/"
    short_title: "Concepts"
    icon: "📚"
  - title: "Guided Walkthrough"
    url: "/beginner/module-2/walkthrough/"
    short_title: "Walkthrough"
    icon: "🚶"
  - title: "Hands-On Labs"
    url: "/beginner/module-2/labs/"
    short_title: "Labs"
    icon: "💻"
  - title: "Knowledge Check"
    url: "/beginner/module-2/quiz/"
    short_title: "Quiz"
    icon: "✅"
  - title: "Resources"
    url: "/beginner/module-2/resources/"
    short_title: "Resources"
    icon: "📖"
  - title: "CSM Scenarios"
    url: "/beginner/module-2/scenarios/"
    short_title: "Scenarios"
    icon: "💼"
next_section:
  url: /beginner/module-2/concepts/
  title: "Core Concepts"
---

## The Version Control Problem

### The Problem Without Version Control

Imagine you're writing a novel with three co-authors. Without version control, you'd face:

- **The Overwrite Problem**: Alice makes changes while Bob is editing the same chapter. Bob saves last, and Alice's work disappears.
- **The Version Chaos**: Files named `draft.doc`, `draft_final.doc`, `draft_final_v2.doc`, `draft_REALLY_final.doc`
- **The Blame Game**: A mistake appears in chapter 5. Who wrote it? When? Why?
- **The Fear Factor**: Nobody wants to refactor the introduction because they might break something.

Now imagine this with code, where thousands of files change daily, dozens of people contribute, and one mistake can crash production systems. **That's why version control exists.**

---

## How Git Solves These Problems

Git is a **distributed version control system** that:

1. **Tracks Every Change**: Every edit, by whom, when, and why
2. **Enables Safe Experimentation**: Work on new features without risking working code
3. **Facilitates Collaboration**: Multiple people edit the same codebase simultaneously
4. **Provides Time Travel**: Rewind to any previous state of your code
5. **Creates Accountability**: Clear audit trail of who changed what


## Git vs GitHub: What's the Difference?

This confuses everyone at first:

| **Git** | **GitHub** |
|---------|-----------|
| Version control tool (software) | Cloud platform/service |
| Runs locally on your computer | Hosted in the cloud |
| Command-line or GUI tools | Web interface + APIs |
| Created by Linus Torvalds (2005) | Founded in 2008 (acquired by Microsoft 2018) |
| Free and open source | Free and paid plans |
| Manages your code history | Adds collaboration, CI/CD, security, project management |

**Analogy**: Git is like Microsoft Word (the software that tracks changes). GitHub is like Microsoft 365 (the cloud service that adds sharing, comments, and collaboration).

---

## A Brief History of Version Control

Understanding the evolution helps you appreciate why Git became dominant:

### Generation 1: Local Only (1970s-80s)
- **RCS, SCCS**: Single-file tracking on one machine
- Problem: No collaboration, no remote backup

### Generation 2: Centralized (1990s-2000s)
- **CVS, SVN (Subversion), Perforce**: One central server
- Problem: Single point of failure, slow for remote teams, branching was painful

### Generation 3: Distributed (2005-present)
- **Git, Mercurial**: Every developer has full history
- Benefit: Work offline, fast operations, easy branching

### Why Git Won

In 2005, Linus Torvalds (creator of Linux) needed a new version control system after a licensing dispute. His requirements:

1. **Speed**: Linux kernel had massive history
2. **Distributed**: Contributors worldwide
3. **Strong safeguards against corruption**: SHA-1 hashes for integrity
4. **Support for non-linear development**: Thousands of parallel branches

He wrote Git in about 2 weeks. The Linux kernel team adopted it immediately. Open source projects followed. Then companies. Today, **over 90% of developers use Git**.


## Where Git Fits in SDLC

Git is used throughout the entire software development lifecycle:

| SDLC Phase | Git Usage |
|------------|-----------|
| **Planning** | Branch for each feature based on requirements |
| **Development** | Commit code changes frequently with clear messages |
| **Testing** | Merge branches when tests pass |
| **Deployment** | Tag releases, track what's deployed where |
| **Maintenance** | Revert bugs, cherry-pick fixes to release branches |
| **Monitoring** | Track which commits introduced issues |

---

## Why CSMs Need to Understand Git

<div class="callout callout-info" markdown="1">
<div class="callout-title">💡 CSM Perspective</div>

You don't need to be a Git expert, but understanding the basics helps you:

- **Speak developer language**: "Are you using feature branches?" vs. "How do teams organize work?"
- **Diagnose problems**: When a customer says "we have lots of merge conflicts," you understand the underlying issue
- **Position value**: GitHub's features (Actions, code review, security scanning) all build on Git workflows
- **Guide adoption**: Understanding Git workflows helps you suggest appropriate GitHub features
</div>

### Customer Questions You'll Now Understand

After this module, you'll be able to engage with:

- "We're evaluating GitHub vs. GitLab vs. Bitbucket" (all Git platforms)
- "Our developers can't resolve merge conflicts" (training opportunity)
- "We need to audit who changed what and when" (Git history + GitHub audit log)
- "Feature branches are causing integration pain" (workflow discussion)


## Coming Up Next

In the next section, we'll build your mental model of Git's core concepts:

- Repositories (local vs. remote)
- Commits (snapshots of your code)
- Branches (parallel development)
- Merging (combining work)
