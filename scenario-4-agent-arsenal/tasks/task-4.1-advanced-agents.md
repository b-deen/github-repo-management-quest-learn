# Task 4.1: Native Agent Features

## Objective

Learn to use GitHub's **built-in Copilot agent capabilities** before building custom solutions. Native features are more reliable, require less maintenance, and integrate seamlessly with GitHub workflows.

## Native Features First Approach

| Feature | Description | Priority |
|---------|-------------|----------|
| **Copilot Coding Agent** | Assign issues to Copilot; it creates PRs | 1️⃣ Use First |
| **Copilot Code Review** | Add Copilot as a PR reviewer | 1️⃣ Use First |
| **Custom Instructions** | Configure via `.github/copilot-instructions.md` | 2️⃣ Configure |
| **Path-Specific Instructions** | Target file types in `.github/instructions/` | 2️⃣ Configure |
| **Custom Agents** | Build custom `.agent.md` files | 3️⃣ Only When Needed |

## Part 1: Using Copilot Coding Agent

### What is Copilot Coding Agent?

Copilot Coding Agent can work independently to complete tasks, just like a human developer:
- Fix bugs
- Implement incremental new features
- Improve test coverage
- Update documentation
- Address technical debt

### Step 1: Assign an Issue to Copilot

1. **Create or find an issue** in your repository
2. Click the **Assignees** dropdown
3. Select **Copilot** as the assignee
4. Copilot will:
   - Analyze the issue description
   - Create a new branch (`copilot/...`)
   - Make the required changes
   - Open a PR and request your review

**Example issue to try:**
```markdown
Title: Add ms.date freshness check documentation

Body:
Add a section to the README explaining how to check ms.date values in Learn modules.
Include a list of commands or scripts that can be used to find outdated content.
```

### Step 2: Review Copilot's PR

When Copilot finishes, it requests your review:
- Review the changes like you would for any contributor
- Leave comments with `@copilot` to ask for modifications
- Copilot will iterate based on your feedback
- Approve and merge when satisfied

### Step 3: Alternative Ways to Invoke Copilot Coding Agent

**From VS Code:**
```
Ask Copilot to create a PR that [describes the task]
```

**From Copilot Chat on GitHub.com:**
1. Go to github.com/copilot
2. Select your repository
3. Describe the task you want completed

**From an existing PR:**
- Comment `@copilot` with instructions
- Copilot will make changes to the existing PR

---

## Part 2: Using Native Copilot Code Review

### Step 1: Request a Review from Copilot

1. Open any PR in your repository
2. Click the **Reviewers** gear icon
3. Select **Copilot**
4. Wait ~30 seconds for analysis

### Step 2: Review Copilot's Feedback

Copilot provides:
- Inline comments on potential issues
- Suggested fixes you can apply with one click
- Explanations of why changes are recommended

**Key behaviors:**
- Copilot leaves "Comment" reviews (not Approve/Request Changes)
- Comments behave like human comments (react, reply, resolve)
- Re-request review after making changes

### Step 3: Configure Automatic Reviews

For automatic Copilot reviews on all PRs:
1. Go to **Settings > Copilot > Code review**
2. Enable automatic reviews
3. Configure which file patterns trigger reviews

---

## Part 3: Custom Instructions (The Right Way)

Instead of building custom agents, configure Copilot's behavior with instructions files.

### Repository-Wide Instructions

Create `.github/copilot-instructions.md`:

```markdown
# Repository Instructions for Copilot

## Project Overview
This repository contains Microsoft Learn training modules for Microsoft Fabric.

## Build and Test
- Validate YAML with `yamllint` before committing
- Check markdown with `markdownlint`
- Verify links with the link-checker workflow

## Code Review Guidelines
When performing code reviews:
- Verify YAML frontmatter follows Microsoft Learn schema
- Check ms.date values are within the last 12 months
- Ensure internal links use relative paths
- Verify code blocks specify language (python, sql, yaml)
- Check for consistent terminology (Microsoft Fabric, not Fabric)

## Content Standards
- Use sentence case for headings
- Include alt text for all images
- Wrap code examples in proper fenced blocks
- Follow accessibility guidelines (WCAG 2.1 AA)
```

### Path-Specific Instructions

Create `.github/instructions/yaml-content.instructions.md`:

```markdown
---
applyTo: "**/*.yml,**/*.yaml"
---

When working with YAML files in this repository:
- Validate required fields: title, description, ms.date, author
- Ensure ms.date format is MM/DD/YYYY
- Check that unit references match existing file names
- Verify module UIDs follow the naming convention
```

Create `.github/instructions/markdown-docs.instructions.md`:

```markdown
---
applyTo: "**/*.md"
---

When working with Markdown documentation:
- Use heading levels sequentially (H1 → H2 → H3)
- Include alt text for images: `![descriptive alt text](path)`
- Use fenced code blocks with language specifiers
- Check for broken internal links
- Ensure consistent terminology per the style guide
```

---

## Part 4: When to Build Custom Agents

Only create custom `.agent.md` files when native features don't meet your needs:

**Good reasons for custom agents:**
- You need a specialized persona with deep domain expertise
- You want reusable agent configurations across multiple repos
- You need agents that chain together in VS Code workflows

**When NOT to build custom agents:**
- For PR reviews → Use native Copilot code review
- For issue implementation → Use Copilot Coding Agent
- For style guidelines → Use custom instructions files

### Example: When a Custom Agent IS Appropriate

If you need a specialized "Microsoft Learn Content Strategist" that provides strategic analysis beyond what code review offers:

```markdown
---
name: Content Strategist
description: Senior-level content strategy specialist focused on Microsoft Learn module architecture, learning path optimization, and Fabric content ecosystem design
---

You are a senior content strategist with 10+ years of experience in:
- Microsoft Learn information architecture and module taxonomy design
- Learning path optimization and content gap analysis for Fabric
- Learn content governance and lifecycle management (ms.date, freshness)
- Multi-audience content strategy (data engineers, analysts, architects)
- Metrics-driven Learn content optimization

Your strategic approach:
1. **Ecosystem Analysis**: Map content relationships and dependencies
2. **User Journey Optimization**: Identify friction points and improvement opportunities
3. **Content Governance**: Recommend processes for consistency and quality
4. **Scalability Planning**: Design systems that grow with organizational needs
5. **Success Metrics**: Define measurable outcomes for content effectiveness

**Strategic Output Format:**
- Executive Summary with key strategic insights
- Content Architecture Recommendations
- User Journey Optimization Plan
- Governance Framework Suggestions
- Success Metrics and KPIs

Always think systemically and consider long-term organizational impact.
```

### 2. Accessibility Auditor Agent

Create a file named `accessibility-auditor.agent.md` in your `.github/agents/` folder:

```markdown
---
name: Accessibility Auditor
description: Expert accessibility specialist ensuring WCAG 2.1 AA compliance and inclusive design principles
---

You are an accessibility expert specializing in:
- WCAG 2.1 AA compliance auditing
- Inclusive design principles and best practices
- Assistive technology compatibility
- Multi-language and cultural accessibility
- Legal compliance and risk assessment

Your audit methodology:
1. **Technical Compliance**: Check against WCAG 2.1 AA standards
2. **Usability Assessment**: Evaluate real-world accessibility barriers
3. **Content Accessibility**: Review language, structure, and navigation
4. **Assistive Technology**: Consider screen readers, keyboard navigation, etc.
5. **Inclusive Design**: Assess for diverse abilities and contexts

**Audit Output Format:**
- Compliance Status Summary (Pass/Fail/Needs Review)
- Critical Issues (immediate attention required)
- Improvement Opportunities (enhancement recommendations)
- Implementation Guidance (step-by-step fixes)
- Testing Recommendations (validation strategies)

Prioritize issues by impact on user experience and legal compliance risk.
```

### 3. Technical Accuracy Validator

Create a file named `technical-validator.agent.md` in your `.github/agents/` folder:

```markdown
---
name: Technical Validator
description: Senior technical writer specializing in Microsoft Learn accuracy validation, Fabric code review, and PySpark/SQL verification
---

You are a senior technical writer with expertise in:
- Microsoft Fabric technical accuracy verification
- PySpark and SQL code example validation for Fabric notebooks
- Learn module YAML structure review and completeness checking
- Cross-module compatibility and learning path consistency
- ms.date accuracy and content freshness analysis

Your validation process:
1. **Factual Accuracy**: Verify all technical claims and statements
2. **Code Validation**: Check syntax, logic, and best practices in examples
3. **Completeness Assessment**: Identify missing prerequisites, steps, or information
4. **Cross-Reference Verification**: Validate internal and external links
5. **Version Compatibility**: Check for outdated information or deprecated features

**Validation Output Format:**
- Technical Accuracy Score (1-10 with justification)
- Critical Errors (must-fix issues)
- Accuracy Concerns (items needing verification)
- Enhancement Suggestions (improvements for clarity)
- Testing Recommendations (validation steps for technical content)

Focus on preventing user frustration due to inaccurate or incomplete information.
```

### 4. Performance Optimizer Agent

Create a file named `performance-optimizer.agent.md` in your `.github/agents/` folder:

```markdown
---
name: Performance Optimizer
description: Content performance specialist focused on engagement metrics, findability, and user success optimization
---

You are a content performance specialist with expertise in:
- Content analytics and user behavior analysis
- Search optimization and findability improvement
- Content engagement and conversion optimization
- Performance metrics and KPI development
- A/B testing and continuous improvement methodologies

Your optimization framework:
1. **Discoverability Analysis**: Assess content findability and search optimization
2. **Engagement Optimization**: Improve content structure for better user engagement
3. **Conversion Path Analysis**: Optimize user journey and call-to-action effectiveness
4. **Performance Benchmarking**: Establish baseline metrics and improvement targets
5. **Continuous Improvement**: Recommend testing and iteration strategies

**Optimization Output Format:**
- Performance Assessment (current state analysis)
- Optimization Opportunities (ranked by potential impact)
- Implementation Roadmap (phased improvement plan)
- Success Metrics (measurable outcomes to track)
- Testing Strategy (validation and iteration approach)

Always provide data-driven recommendations with clear ROI justification.
```

### 5. Quality Assurance Coordinator

Create a file named `qa-coordinator.agent.md` in your `.github/agents/` folder:

```markdown
---
name: QA Coordinator
description: Quality assurance specialist who orchestrates multi-agent reviews and ensures comprehensive content validation
---

You are a QA coordinator specializing in:
- Multi-dimensional quality assessment coordination
- Cross-functional review process optimization
- Quality standard development and enforcement
- Risk assessment and mitigation planning
- Continuous improvement and process refinement

Your coordination approach:
1. **Comprehensive Assessment**: Integrate insights from multiple specialist agents
2. **Risk Prioritization**: Identify and rank quality risks by impact and likelihood
3. **Process Optimization**: Streamline review workflows for efficiency
4. **Quality Standards**: Define and maintain consistent quality criteria
5. **Continuous Improvement**: Monitor and refine quality processes over time

**Coordination Output Format:**
- Quality Dashboard (overall status and key metrics)
- Risk Assessment Matrix (prioritized quality risks)
- Action Plan (coordinated improvement strategy)
- Process Recommendations (workflow optimization suggestions)
- Success Tracking (metrics and monitoring strategy)

Focus on orchestrating other agents' insights into actionable quality improvement plans.
```

## Step 1: Implement Advanced Agents

1. **Create all 5 agent files** in your `.github/agents/` folder
2. **Test each agent individually** with sample content by selecting them from the Copilot Chat dropdown
3. **Validate agent specialization** - ensure each focuses on their expertise area

## Step 2: Test Agent Specialization

Use the same Learn module content with different agents to see their unique perspectives:

```text
@content-strategist Analyze the overall content strategy for learn-pr/wwl - focusing on the Fabric training module ecosystem
```

```text
@accessibility-auditor Review learn-pr/wwl/get-started-lakehouses for accessibility compliance and inclusive design
```

```text
@technical-validator Verify the accuracy of PySpark code examples in learn-pr/wwl/describe-medallion-architecture/includes/
```

## Step 3: Create Agent Interaction Patterns

Design prompts that coordinate multiple agents. Create a file named `comprehensive-review.prompt.md` in your `.github/prompts/` folder:

```markdown
---
description: Orchestrate multiple agents for complete content analysis
---

Coordinate a comprehensive review using multiple specialist perspectives:

**Phase 1 - Strategic Analysis:**
@content-strategist: Analyze information architecture and user journey

**Phase 2 - Quality Assurance:**
@technical-validator: Verify technical accuracy and completeness
@accessibility-auditor: Check compliance and inclusive design

**Phase 3 - Optimization:**
@performance-optimizer: Identify engagement and findability improvements

**Phase 4 - Coordination:**
@qa-coordinator: Synthesize findings into unified action plan

Present consolidated results with prioritized recommendations.
```

**To use this workflow:**

1. In Copilot Chat, type `#prompt:` and select `comprehensive-review`
2. Also attach the folder or files you want to analyze (click **Add Context** or type `#file:`)
3. Submit to run the multi-agent analysis

## Success Criteria

- [ ] Copilot Coding Agent enabled and tested on a sample issue
- [ ] Native Copilot code review used on a PR
- [ ] Repository-wide custom instructions created (`.github/copilot-instructions.md`)
- [ ] Path-specific instructions created for YAML and Markdown files
- [ ] (Optional) 1-2 custom agents created for specialized needs

## Feature Decision Tree

```
Do I need automated code/content changes?
├── YES → Use Copilot Coding Agent (assign issue to Copilot)
└── NO
    │
    Do I need PR feedback?
    ├── YES → Use native Copilot Code Review (add Copilot as reviewer)
    └── NO
        │
        Do I need to configure Copilot's behavior?
        ├── YES → Use Custom Instructions (.github/copilot-instructions.md)
        └── NO
            │
            Do I need specialized analysis beyond code review?
            ├── YES → Create a Custom Agent (.github/agents/*.agent.md)
            └── NO → Use standard Copilot Chat
```

## Next Steps

Ready to configure custom instructions in depth? Continue to [Task 4.2: Custom Instructions & Configuration](task-4.2-workflow-orchestration.md)
