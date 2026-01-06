# Task 4.2: Custom Instructions & Configuration

## Objective

Configure Copilot with repository-specific instructions, path-based rules, and review customization to tailor its behavior without building custom agents.

## Why Custom Instructions?

Custom instructions are the **recommended way** to customize Copilot behavior:
- ✅ No code to maintain
- ✅ Works with native Copilot Code Review
- ✅ Works with Copilot Coding Agent
- ✅ Applies automatically to all Copilot interactions
- ✅ Version controlled with your repository

## Instruction File Types

| File | Location | Purpose |
|------|----------|---------|
| `copilot-instructions.md` | `.github/` | Repository-wide instructions |
| `*.instructions.md` | `.github/instructions/` | Path-specific instructions |
| `AGENTS.md` | Any directory | Agent context (nearest file wins) |

## Part 1: Repository-Wide Instructions

### Step 1: Create the Main Instructions File

Create `.github/copilot-instructions.md`:

```markdown
# Copilot Instructions for Microsoft Learn Content

## Project Overview
This repository contains Microsoft Learn training modules for Microsoft Fabric.
Modules are located in `learn-pr/wwl/` and follow the Learn authoring format.

## Repository Structure
- `learn-pr/wwl/*/` - Individual training modules
- `learn-pr/wwl/*/includes/` - Module units (content files)
- `learn-pr/wwl/*/index.yml` - Module metadata and unit definitions

## Build and Validation
Always run these checks before committing:
1. `yamllint **/*.yml` - Validate YAML syntax
2. `markdownlint **/*.md` - Check markdown formatting
3. Verify all internal links resolve correctly

## Content Standards

### YAML Metadata Requirements
Every `index.yml` must include:
- `uid`: Unique identifier matching folder name pattern
- `title`: Module title (sentence case)
- `description`: 150-300 character description
- `ms.date`: Last significant update date (MM/DD/YYYY format)
- `author`: GitHub username of primary author

### Markdown Formatting
- Use sentence case for headings (not Title Case)
- Include alt text for all images
- Specify language in fenced code blocks (```python, ```sql, ```yaml)
- Use relative links for internal references

### Code Examples
- All PySpark code must be valid for Fabric notebooks
- Include necessary imports at the start of code blocks
- Add comments explaining non-obvious logic
- Test code examples before committing

## Code Review Focus
When reviewing PRs, prioritize:
1. YAML structure and required fields
2. ms.date freshness (should be within 12 months)
3. Broken internal links
4. Missing alt text on images
5. Inconsistent terminology

## Terminology
Always use:
- "Microsoft Fabric" (not just "Fabric" on first reference)
- "lakehouse" (lowercase, one word)
- "PySpark" (not "pyspark" or "Pyspark")
- "SQL" (uppercase)
```

### Step 2: Verify Instructions Are Used

1. Make a change to any file in the repository
2. Open Copilot Chat and ask a question about the repository
3. Check the **References** section - you should see `copilot-instructions.md` listed

---

## Part 2: Path-Specific Instructions

Path-specific instructions apply only when Copilot is working on matching files.

### Step 1: Create YAML-Specific Instructions

Create `.github/instructions/yaml-modules.instructions.md`:

```markdown
---
applyTo: "**/index.yml,**/*.yml"
---

# YAML File Instructions

## Required Fields for index.yml
Every module index.yml must have:
```yaml
### YamlMime:Module
uid: learn.wwl.[module-name]
title: [Module Title]
description: [150-300 characters]
ms.date: [MM/DD/YYYY]
author: [github-username]
ms.author: [microsoft-alias]
ms.topic: module
```

## Unit References
When adding units, ensure:
- Unit files exist in the `includes/` folder
- Unit UIDs follow pattern: `learn.wwl.[module-name].[unit-name]`
- Unit order matches intended learning progression

## Common YAML Errors to Avoid
- Missing quotes around strings with colons
- Incorrect indentation (use 2 spaces)
- Trailing whitespace
- Duplicate keys
```

### Step 2: Create Markdown-Specific Instructions

Create `.github/instructions/markdown-content.instructions.md`:

```markdown
---
applyTo: "**/*.md"
---

# Markdown Content Instructions

## Heading Structure
- Use only one H1 (`#`) per file - the title
- Follow sequential heading levels: H1 → H2 → H3 (no skipping)
- Use sentence case for headings

## Code Blocks
Always specify the language:
```python
# Python/PySpark code
```
```sql
-- SQL queries
```
```yaml
# YAML configuration
```

## Links
- Use relative paths for internal links: `[text](../other-module/file.md)`
- Use descriptive link text (not "click here")
- Verify links resolve before committing

## Images
- Include alt text: `![Descriptive alt text](path/to/image.png)`
- Store images in module's `media/` folder
- Use PNG or SVG for diagrams, JPG for photos

## Lists
- Use `-` for unordered lists
- Use `1.` for ordered lists (auto-numbered)
- Indent nested items with 2 spaces
```

### Step 3: Create PySpark-Specific Instructions

Create `.github/instructions/pyspark-code.instructions.md`:

```markdown
---
applyTo: "**/includes/*.md"
excludeAgent: "code-review"
---

# PySpark Code Example Instructions

These instructions apply when Copilot Coding Agent works on unit files.

## PySpark Best Practices for Fabric
When generating PySpark code examples:

```python
# Always include necessary imports
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, when, lit

# Get the Spark session (Fabric provides this automatically)
spark = SparkSession.builder.getOrCreate()
```

## Common Patterns

### Reading from Lakehouse
```python
df = spark.read.format("delta").load("Tables/my_table")
```

### Writing to Lakehouse
```python
df.write.format("delta").mode("overwrite").save("Tables/output_table")
```

## Error Handling
Always show error handling for production code:
```python
try:
    result = df.collect()
except Exception as e:
    print(f"Error: {e}")
```
```

---

## Part 3: Agent Context Files (AGENTS.md)

For more complex projects, use `AGENTS.md` files to provide directory-specific context.

### Step 1: Create Root AGENTS.md

Create `AGENTS.md` in the repository root:

```markdown
# Agent Instructions

This repository contains Microsoft Learn training content for Microsoft Fabric.

## Key Directories
- `learn-pr/wwl/` - All training modules
- `.github/` - Configuration and workflows
- `media/` - Shared images and diagrams

## Working with This Repository

### Before Making Changes
1. Read the relevant module's README if it exists
2. Check the `index.yml` for module structure
3. Verify ms.date is updated when changing content

### After Making Changes
1. Run linting checks
2. Verify all links work
3. Update ms.date if content significantly changed
```

### Step 2: Create Module-Specific AGENTS.md

Create `learn-pr/wwl/AGENTS.md`:

```markdown
# Learn Module Instructions

Each subdirectory is a complete Microsoft Learn module.

## Module Structure
```
module-name/
├── index.yml          # Module metadata and unit list
├── includes/          # Unit content files
│   ├── 1-introduction.md
│   ├── 2-main-content.md
│   └── n-summary.md
├── media/             # Module images
└── knowledge-check.yml # Quiz questions (optional)
```

## Common Tasks

### Adding a New Unit
1. Create the markdown file in `includes/`
2. Add the unit reference to `index.yml`
3. Update the module description if needed
4. Update `ms.date`

### Updating Existing Content
1. Make content changes
2. Update `ms.date` in `index.yml`
3. Verify cross-references still work
```

---

## Part 4: Testing Your Configuration

### Test 1: Copilot Code Review with Custom Instructions

1. Create a PR with intentional issues
2. Add Copilot as a reviewer
3. Verify Copilot references your custom instructions in its feedback

### Test 2: Copilot Coding Agent with Instructions

1. Create an issue: "Add a new unit about data transformation"
2. Assign to Copilot
3. Verify the PR follows your custom instructions

### Test 3: Path-Specific Instructions

1. Ask Copilot Chat about a YAML file
2. Verify it applies YAML-specific guidance
3. Ask about a Markdown file
4. Verify it applies Markdown-specific guidance

---

## Success Criteria

- [ ] Repository-wide instructions created (`.github/copilot-instructions.md`)
- [ ] Path-specific instructions for YAML files
- [ ] Path-specific instructions for Markdown files
- [ ] Instructions verified in Copilot Chat references
- [ ] Copilot Code Review uses custom instructions
- [ ] (Optional) AGENTS.md files for directory context

## Next Steps

Ready to build custom agents for specialized workflows? Continue to [Task 4.3: Advanced Custom Agents](task-4.3-performance-optimization.md)

```markdown
---
description: Comprehensive workflow for validating content before publication
---

Execute a complete content publication validation workflow:

**Stage 1: Strategic Review**
@content-strategist: Evaluate content alignment with:
- Overall content strategy and user journey
- Information architecture and taxonomy
- Audience needs and business objectives

**Stage 2: Technical Validation**
@technical-validator: Verify:
- Technical accuracy and completeness
- Code examples and implementation details
- Cross-references and link validity

**Stage 3: Accessibility Compliance**
@accessibility-auditor: Assess:
- WCAG 2.1 AA compliance status
- Inclusive design implementation
- Multi-modal accessibility support

**Stage 4: Performance Optimization**
@performance-optimizer: Analyze:
- Content discoverability and SEO factors
- User engagement optimization opportunities
- Conversion path effectiveness

**Stage 5: Quality Coordination**
@qa-coordinator: Synthesize findings and create:
- Publication readiness assessment (Go/No-Go decision)
- Priority issue resolution plan
- Success metrics for post-publication monitoring

**Final Output:**
- Publication Decision: [APPROVED/NEEDS REVISION/BLOCKED]
- Critical Issues: [Must-fix items before publication]
- Enhancement Opportunities: [Optional improvements]
- Monitoring Plan: [Post-publication success tracking]
```

### Test the Publication Pipeline

Run this workflow on sample content by attaching the prompt file:

1. Open Copilot Chat
2. Type `#prompt:` and select `content-publication-pipeline`
3. Add your query: `@workspace Evaluate scenario-1-inheritance/challenge-repo/docs/getting-started.md for publication readiness`

## Workflow 2: Repository Health Assessment

Create a file named `repository-health-check.prompt.md` in your `.github/prompts/` folder:

```markdown
---
description: Comprehensive repository analysis using parallel agent assessment
---

Conduct a complete repository health assessment using parallel analysis:

**Parallel Assessment Phase:**

**Track A - Content Strategy Analysis:**
@content-strategist: Evaluate repository-wide:
- Content architecture and organization
- User journey completeness and effectiveness
- Content gap analysis and strategic opportunities

**Track B - Technical Quality Assessment:**
@technical-validator: Assess across all content:
- Technical accuracy and currency
- Code example quality and completeness
- Cross-reference integrity and maintenance needs

**Track C - Accessibility Compliance Review:**
@accessibility-auditor: Audit for:
- Universal design principles implementation
- Compliance gaps and risk assessment
- Inclusive content recommendations

**Track D - Performance Analysis:**
@performance-optimizer: Analyze:
- Content discoverability and search optimization
- User engagement patterns and opportunities
- Performance benchmarks and improvement potential

**Synthesis Phase:**
@qa-coordinator: Create integrated health report:
- Overall Health Score (1-100 with breakdown by category)
- Critical Issues Matrix (impact vs. effort to fix)
- Strategic Improvement Roadmap (6-month plan)
- Resource Allocation Recommendations
- Success Metrics and Monitoring Strategy

**Executive Summary Format:**
- Repository Health Status: [EXCELLENT/GOOD/NEEDS ATTENTION/CRITICAL]
- Top 3 Strategic Priorities
- Resource Requirements
- Timeline for Improvements
```

### Test the Health Assessment

1. Open Copilot Chat
2. Type `#prompt:` and select `repository-health-check`
3. Add: `@workspace Conduct a complete health assessment of this repository`

## Workflow 3: Issue Resolution Workflow

Create a file named `issue-resolution-workflow.prompt.md` in your `.github/prompts/` folder:

```markdown
---
description: Smart triage and resolution planning for repository issues using conditional agent routing
---

Execute intelligent issue resolution workflow with conditional routing:

**Phase 1: Initial Triage**
@qa-coordinator: Analyze the issue and determine routing:
- Issue type classification (content, technical, accessibility, strategic)
- Severity assessment (critical, high, medium, low)
- Complexity evaluation (simple, moderate, complex)
- Stakeholder impact analysis

**Phase 2: Conditional Specialist Routing**

**IF Content Strategy Issue:**
@content-strategist: Develop strategic resolution approach

**IF Technical Accuracy Issue:**
@technical-validator: Create technical resolution plan

**IF Accessibility Issue:**
@accessibility-auditor: Design compliance resolution strategy

**IF Performance Issue:**
@performance-optimizer: Optimize for better user outcomes

**Phase 3: Cross-Impact Analysis**
@qa-coordinator: Assess resolution impact on:
- Related content and dependencies
- User experience and journey flows
- Technical system requirements
- Resource and timeline implications

**Phase 4: Resolution Planning**
Create comprehensive resolution plan:
- Implementation Strategy (step-by-step approach)
- Resource Requirements (time, skills, tools)
- Risk Assessment (potential complications)
- Success Criteria (measurable outcomes)
- Timeline (realistic milestones)

**Output Format:**
- Resolution Recommendation: [IMPLEMENT/DEFER/ESCALATE/CLOSE]
- Implementation Plan: [Detailed action steps]
- Success Metrics: [How to measure resolution success]
- Follow-up Actions: [Monitoring and validation steps]
```

### Test Issue Resolution Workflow

Use with actual repository issues:

1. Open Copilot Chat
2. Attach the `issue-resolution-workflow` prompt
3. Add: `Analyze and create resolution plan for: [paste issue description or URL]`

## Advanced Orchestration Techniques

### 1. Agent Handoff Patterns

Create a prompt file named `agent-handoff-demo.prompt.md` in your `.github/prompts/` folder that explicitly passes context between agents:

```markdown
---
description: Demonstrate explicit context passing between agents
---

**Agent Handoff Workflow Example:**

**Step 1**: @content-strategist
Analyze content strategy and create strategic context for technical review.
Pass to technical validator: [Strategic priorities and user journey context]

**Step 2**: @technical-validator
Using strategic context from Step 1, validate technical accuracy.
Pass to accessibility auditor: [Technical findings and strategic context]

**Step 3**: @accessibility-auditor
Using context from Steps 1-2, assess accessibility compliance.
Pass to performance optimizer: [Combined strategic, technical, and accessibility insights]

**Step 4**: @performance-optimizer
Using all previous context, optimize for performance.
Pass to QA coordinator: [Complete analysis with all specialist inputs]

**Step 5**: @qa-coordinator
Synthesize all specialist inputs into unified action plan.
```

### 2. Feedback Loop Integration

Create a prompt file named `iterative-improvement-cycle.prompt.md` in your `.github/prompts/` folder:

```markdown
---
description: Workflow with built-in feedback loops and continuous improvement
---

**Iterative Improvement Workflow:**

**Cycle 1: Initial Analysis**
@qa-coordinator: Run complete assessment and identify top 3 improvement opportunities:
- Synthesize findings from all specialist perspectives
- Prioritize issues by impact and effort
- Recommend highest-priority fixes

**Cycle 2: Validation and Refinement**
@technical-validator: Re-analyze improved content:
- Verify fixes were implemented correctly
- Measure improvement effectiveness
- Identify any regressions or new issues

@content-strategist: Evaluate strategic impact:
- Assess alignment with content goals
- Identify next iteration opportunities
- Update content roadmap as needed

**Cycle 3: Optimization and Scaling**
@performance-optimizer: Apply successful patterns:
- Document what worked and why
- Identify similar content for improvement
- Create templates for repeatable fixes

@qa-coordinator: Plan next improvement cycle:
- Summarize lessons learned
- Update quality standards based on findings
- Schedule next review cycle

**Output:** Improvement cycle report with metrics, patterns, and next steps.
```

## Success Criteria

- [ ] 3 complex workflow prompt files created in `.github/prompts/`
- [ ] Sequential, parallel, and conditional patterns demonstrated
- [ ] Agent handoff patterns working effectively
- [ ] Workflows produce actionable, integrated outputs
- [ ] Feedback loops and iteration cycles established

## Next Steps

Ready to optimize performance? Continue to [Task 4.3: Performance Optimization](task-4.3-performance-optimization.md)
