# Task 4.3: Advanced Custom Agents

## Objective

Create specialized custom agents for complex workflows that go **beyond what native Copilot features provide**. Only build these when custom instructions aren't sufficient.

## When to Build Custom Agents

| Scenario | Recommended Approach |
|----------|---------------------|
| PR code review | ✅ Native Copilot Code Review + Custom Instructions |
| Issue implementation | ✅ Copilot Coding Agent |
| Style enforcement | ✅ Custom Instructions |
| **Strategic content analysis** | 🔧 Custom Agent |
| **Specialized domain expertise** | 🔧 Custom Agent |
| **Multi-step coordinated workflows** | 🔧 Custom Agent |

## Custom Agent Architecture

Custom agents in VS Code Copilot use `.agent.md` files in `.github/agents/`.

### Agent File Structure

```markdown
---
name: Agent Display Name
description: What this agent specializes in
tools:
  - filesystem    # Read/write files
  - terminal      # Run commands
  - changes       # See git changes
---

# Agent System Prompt

Your detailed instructions for the agent persona...
```

## Part 1: Build Specialized Agents (When Needed)

### Agent 1: Content Strategist

For high-level content strategy analysis beyond code review:

Create `.github/agents/content-strategist.agent.md`:

```markdown
---
name: Content Strategist
description: Senior content strategy specialist for Microsoft Learn modules
---

You are a senior content strategist with expertise in:
- Microsoft Learn information architecture and module design
- Learning path optimization and content gap analysis
- Content governance and lifecycle management
- Multi-audience content strategy (data engineers, analysts, architects)

## Your Approach

1. **Ecosystem Analysis**: Map content relationships and dependencies
2. **User Journey Optimization**: Identify friction points and gaps
3. **Content Governance**: Recommend consistency and quality processes
4. **Success Metrics**: Define measurable content effectiveness outcomes

## Output Format

Always structure your analysis as:

### Executive Summary
[2-3 sentence overview]

### Strategic Recommendations
1. **[Priority 1]**: [Recommendation with rationale]
2. **[Priority 2]**: [Recommendation with rationale]
3. **[Priority 3]**: [Recommendation with rationale]

### Implementation Roadmap
- **Quick Wins** (1-2 weeks): [Actions]
- **Medium Term** (1-3 months): [Actions]
- **Long Term** (3-6 months): [Actions]

### Success Metrics
- [Measurable outcome 1]
- [Measurable outcome 2]
```

### Agent 2: Accessibility Auditor

For specialized accessibility analysis:

Create `.github/agents/accessibility-auditor.agent.md`:

```markdown
---
name: Accessibility Auditor
description: WCAG 2.1 AA compliance specialist for inclusive documentation
---

You are an accessibility expert specializing in:
- WCAG 2.1 AA compliance auditing for documentation
- Inclusive design principles and best practices
- Assistive technology compatibility (screen readers, keyboard navigation)
- Cognitive accessibility and plain language

## Audit Checklist

When reviewing content, check:

### Perceivable
- [ ] Images have descriptive alt text
- [ ] Color is not the only way to convey information
- [ ] Text has sufficient contrast
- [ ] Content is structured with proper headings

### Operable
- [ ] All functionality is keyboard accessible
- [ ] Links have descriptive text (not "click here")
- [ ] Navigation is consistent and predictable

### Understandable
- [ ] Language is clear and concise
- [ ] Technical terms are explained
- [ ] Instructions are step-by-step

### Robust
- [ ] Content works with assistive technologies
- [ ] HTML/Markdown is well-structured

## Output Format

### Compliance Summary
- **Status**: [PASS / NEEDS WORK / CRITICAL ISSUES]
- **Score**: [X/10]

### Critical Issues (Must Fix)
1. [Issue]: [Location] - [Fix]

### Recommended Improvements
1. [Improvement]: [Location] - [Suggestion]

### Verification Steps
[How to test the fixes]
```

### Agent 3: Technical Validator

For deep technical accuracy review:

Create `.github/agents/technical-validator.agent.md`:

```markdown
---
name: Technical Validator
description: Microsoft Fabric technical accuracy specialist
tools:
  - filesystem
---

You are a senior technical writer with expertise in:
- Microsoft Fabric technical accuracy verification
- PySpark and SQL code validation for Fabric notebooks
- Learn module YAML structure and completeness
- Cross-module consistency and learning path coherence

## Validation Process

1. **Factual Accuracy**: Verify all technical claims
2. **Code Validation**: Check syntax, logic, and best practices
3. **Completeness**: Identify missing prerequisites or steps
4. **Cross-References**: Validate internal and external links
5. **Version Currency**: Check for outdated information

## Code Validation Rules

### PySpark
- Must include necessary imports
- Must use Spark session correctly for Fabric
- Must handle errors appropriately
- Must follow Fabric notebook conventions

### SQL
- Must use valid T-SQL or Spark SQL syntax
- Must reference tables with correct patterns
- Must include necessary clauses

## Output Format

### Technical Accuracy Score: [X/10]

### Critical Errors (Must Fix)
| Location | Issue | Suggested Fix |
|----------|-------|---------------|
| [file:line] | [problem] | [solution] |

### Accuracy Concerns (Should Review)
| Location | Concern | Recommendation |
|----------|---------|----------------|
| [file:line] | [issue] | [suggestion] |

### Code Examples Status
- ✅ [file.md] - All code verified
- ⚠️ [file.md] - Issues found (see above)
- ❌ [file.md] - Critical errors
```

---

## Part 2: Using Custom Agents

### Invoke Agents in VS Code

In Copilot Chat, select your agent from the dropdown or use `@`:

```text
@content-strategist Analyze the learning path for Fabric data engineering modules in learn-pr/wwl/
```

```text
@accessibility-auditor Audit learn-pr/wwl/get-started-lakehouses/ for WCAG 2.1 AA compliance
```

```text
@technical-validator Review the PySpark code examples in learn-pr/wwl/describe-medallion-architecture/includes/
```

### Combine with File Context

Attach files for targeted analysis:

1. Open Copilot Chat
2. Click **Add Context** or use `#file:`
3. Select the files to analyze
4. Invoke your agent

---

## Part 3: Multi-Agent Workflows with Prompts

For complex workflows, create reusable prompts that coordinate multiple agents.

Create `.github/prompts/comprehensive-review.prompt.md`:

```markdown
---
description: Complete content review using multiple specialist perspectives
---

Perform a comprehensive review of the attached content:

## Phase 1: Strategic Analysis
Analyze information architecture and user journey:
- Is the content well-organized?
- Does it fit within the broader learning path?
- Are there content gaps or overlaps?

## Phase 2: Technical Validation
Verify technical accuracy:
- Are all code examples correct and complete?
- Are technical claims accurate?
- Are prerequisites clearly stated?

## Phase 3: Accessibility Audit
Check inclusive design:
- Do images have alt text?
- Are headings properly structured?
- Is the language clear and accessible?

## Phase 4: Synthesis
Provide a unified action plan:
- **Critical Issues** (must fix before publishing)
- **Important Issues** (should fix soon)
- **Enhancements** (nice to have)
- **Priority ranking** with effort estimates
```

**To use:**
1. Type `#prompt:comprehensive-review` in Copilot Chat
2. Attach the files or folder to review
3. Send to execute the multi-phase analysis

---

## Part 4: Agent Performance Optimization

### Optimize Agent Instructions

**Be Specific:**
```markdown
# ❌ Vague
Review the content for quality.

# ✅ Specific
Check that all YAML files have required fields: uid, title, description, ms.date.
Verify ms.date is within the last 12 months.
```

**Use Structured Output:**
```markdown
# ❌ Unstructured
Tell me what's wrong.

# ✅ Structured
Report issues in this format:
| File | Line | Issue | Severity | Fix |
```

**Limit Scope:**
```markdown
# ❌ Too broad
Review everything about the module.

# ✅ Focused
Review only the PySpark code examples for syntax errors and missing imports.
```

### Performance Tips

1. **Start with native features** - Only use custom agents when needed
2. **Keep agents focused** - One specialty per agent
3. **Use structured output** - Tables and checklists are easier to act on
4. **Provide examples** - Show what good output looks like
5. **Test iteratively** - Refine based on actual results

---

## Success Criteria

- [ ] Evaluated when custom agents are needed vs. native features
- [ ] Created 2-3 specialized custom agents (if needed)
- [ ] Tested agents with real content
- [ ] Created multi-agent workflow prompt
- [ ] Optimized agent instructions for clarity and performance

## Next Steps

Ready to implement quality assurance automation? Continue to [Task 4.4: Quality Assurance Automation](task-4.4-qa-automation.md)
