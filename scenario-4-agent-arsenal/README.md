# Scenario 4: The Agent Arsenal

## Overview

You've mastered the basics of repository management with GitHub Copilot. Now it's time to leverage GitHub's **native agent capabilities** and learn when to extend them with custom configurations. This scenario teaches you to use built-in features first, then customize when needed.

**Duration:** 45-60 minutes  
**Difficulty:** Advanced

## Native GitHub Features First

Before building custom agents, understand what GitHub provides out-of-the-box:

| Native Feature | What It Does | When to Use |
|----------------|--------------|-------------|
| **Copilot Coding Agent** | Assign issues to Copilot; it creates PRs automatically | Issue implementation, bug fixes, documentation updates |
| **Copilot Code Review** | Add Copilot as a PR reviewer | Automated PR feedback with suggested fixes |
| **Custom Instructions** | Configure Copilot's behavior via `.github/copilot-instructions.md` | Repository-specific guidelines |
| **Path-Specific Instructions** | Target specific file types in `.github/instructions/*.instructions.md` | Language/framework-specific rules |

## The Challenge

Your organization is scaling rapidly, and you need to establish automated workflows for:
- **Content quality assurance** across all Microsoft Learn Fabric modules
- **Automated issue resolution** using Copilot Coding Agent
- **Custom review workflows** tailored to Learn content standards
- **Performance optimization** for large-scale module operations

## Learning Objectives

By completing this scenario, you will:

1. **Use Copilot Coding Agent** - Assign issues directly to Copilot for automated implementation
2. **Configure native code review** - Set up automatic Copilot reviews with custom instructions
3. **Create custom instructions** - Tailor Copilot's behavior to your repository's needs
4. **Build specialized agents** - Extend native features when built-in options aren't enough
5. **Design agent workflows** - Combine native and custom agents for comprehensive analysis

## Real-World Context

This scenario simulates managing the Microsoft Learn Fabric training modules with:
- Multiple Fabric workloads (lakehouses, medallion, real-time, Copilot)
- Diverse content types (conceptual units, hands-on exercises, knowledge checks)
- Global audience requiring accessibility compliance
- ms.date freshness requirements for accuracy
- Consistent quality across 55+ Fabric learning modules

## Scenario Structure

### Task 4.1: Native Agent Features
Learn to use GitHub's built-in Copilot Coding Agent and Code Review features before building custom solutions.

### Task 4.2: Custom Instructions & Configuration
Configure Copilot with repository-specific instructions, path-based rules, and review customization.

### Task 4.3: Advanced Custom Agents
Create specialized agents for complex workflows that go beyond native capabilities.

### Task 4.4: Quality Assurance Automation
Build automated quality pipelines using GitHub Actions integrated with Copilot.

## Prerequisites

- Completed Module 0: Workspace Preparation
- Basic understanding of workflow automation concepts
- Experience with Scenarios 1-3 (recommended)

## Files You'll Work With

```
.github/
├── copilot-instructions.md     # Repository-wide custom instructions
├── instructions/               # Path-specific instruction files
│   ├── yaml-content.instructions.md
│   └── markdown-docs.instructions.md
├── agents/                     # Custom agent configurations (when needed)
└── workflows/                  # GitHub Actions for automation

learn-pr/wwl/                   # Real Fabric training modules to practice with
├── get-started-lakehouses/
├── describe-medallion-architecture/
├── introduction-to-copilot-fabric/
└── ... (55+ modules)
```

## Success Metrics

By the end of this scenario, you should have:
- **Copilot Coding Agent enabled** and tested on sample issues
- **Automatic code review configured** with custom instructions
- **Path-specific instructions** for different content types
- **2-3 custom agents** for specialized workflows (when needed)
- **Quality automation pipeline** using GitHub Actions

## Real-World Applications

The skills you develop here apply directly to:
- **Enterprise Learn content management** at scale
- **Automated issue resolution** with Copilot Coding Agent
- **Consistent PR reviews** with custom review instructions
- **Multi-team collaboration** on shared documentation
- **Automated freshness checking** for ms.date compliance

## Getting Started

Ready to build your agent arsenal? Begin with [Task 4.1: Native Agent Features](tasks/task-4.1-advanced-agents.md).

---

**Note:** This scenario prioritizes native GitHub features. Only create custom agents (`.agent.md` files) when built-in capabilities don't meet your needs.
