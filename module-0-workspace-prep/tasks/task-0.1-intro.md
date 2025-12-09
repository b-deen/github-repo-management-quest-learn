# Task 0.1: Introduction to Agents and Prompts

## Objective

Learn the fundamentals of GitHub Copilot workspace customization using custom agents and reusable prompts.

## Background

GitHub Copilot's workspace features allow you to create:

- **Custom agents** (`.agent.md` files): Specialized AI assistants with specific roles and expertise
- **Reusable prompts** (`.prompt.md` files): Template prompts that can be easily invoked and customized

These features transform GitHub Copilot from a general-purpose assistant into a tailored toolkit for your specific workflows.

## What You'll Learn

1. **Agent fundamentals** - How agents work and their benefits
2. **Prompt engineering** - Creating effective, reusable prompts
3. **Workspace integration** - How these features work together
4. **Best practices** - Naming conventions and organization strategies

## Step 1: Explore Example Configurations

First, let's see what agents and prompts look like in action.

### Creating Custom Agents

Custom agents are Markdown files with YAML frontmatter stored in the `.github/agents/` directory. Each agent has its own file with a `.agent.md` extension.

#### Agent File Structure

```text
.github/
└── agents/
    ├── documentation-auditor.agent.md
    ├── style-enforcer.agent.md
    └── content-strategist.agent.md
```

#### Example: documentation-auditor.agent.md

Create a file at `.github/agents/documentation-auditor.agent.md`:

```markdown
---
name: documentation-auditor
description: Specialized agent for analyzing documentation repositories and identifying content gaps, inconsistencies, and improvement opportunities
tools: ["read", "search", "edit"]
---

You are a documentation auditor with expertise in:
- Content gap analysis and identification
- Style consistency checking across multiple files
- Information architecture assessment
- User experience evaluation for documentation
- Technical accuracy validation

When analyzing content:
1. Look for completeness and logical flow
2. Check for broken or outdated links
3. Identify missing prerequisites or setup instructions
4. Assess clarity and accessibility of language
5. Suggest structural improvements

Always provide actionable recommendations with specific examples.
```

#### Example: style-enforcer.agent.md

Create a file at `.github/agents/style-enforcer.agent.md`:

```markdown
---
name: style-enforcer
description: Agent focused on maintaining consistent writing style and documentation standards
tools: ["read", "search"]
---

You are a style guide enforcer for documentation teams. Your expertise includes:
- Grammar, spelling, and punctuation consistency
- Tone and voice alignment with brand guidelines
- Formatting consistency (headings, lists, code blocks)
- Terminology standardization
- Accessibility compliance (alt text, heading structure)

When reviewing content:
1. Check adherence to established style guidelines
2. Identify inconsistent terminology or phrasing
3. Suggest improvements for readability and accessibility
4. Ensure proper markdown formatting
5. Maintain professional, helpful tone

Provide specific corrections and explanations for suggested changes.
```

#### Example: content-strategist.agent.md

Create a file at `.github/agents/content-strategist.agent.md`:

```markdown
---
name: content-strategist
description: Senior-level content strategy specialist focused on Microsoft Learn module architecture, learning path optimization, and Fabric content ecosystem design
tools: ["read", "search"]
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

#### Agent YAML Properties Reference

| Property | Required | Description |
|----------|----------|-------------|
| `name` | No | Display name (defaults to filename) |
| `description` | **Yes** | Brief explanation of the agent's purpose |
| `tools` | No | List of tools the agent can use (e.g., `["read", "search", "edit"]`) |
| `model` | No | Specify AI model (VS Code only) |
| `target` | No | `vscode` or `github-copilot` to limit availability |

### Creating Reusable Prompts

Prompts are also Markdown files with YAML frontmatter, stored in `.github/prompts/`:

#### Prompt File Structure

```text
.github/
└── prompts/
    ├── content-audit.prompt.md
    ├── style-check.prompt.md
    └── generate-summary.prompt.md
```

#### Example: content-audit.prompt.md

Create a file at `.github/prompts/content-audit.prompt.md`:

```markdown
---
description: Comprehensive analysis of repository content and structure
---

Please analyze this repository's documentation with focus on:

**Content Analysis:**
- Overall structure and organization
- Completeness of information
- Content gaps or missing sections
- Outdated or inaccurate information

**User Experience:**
- Navigation and findability
- Clarity of instructions
- Appropriate depth for target audience
- Logical flow and sequencing

**Technical Quality:**
- Link validity and accuracy
- Code example functionality
- Prerequisites and setup clarity
- Cross-references and consistency

Provide a structured report with:
1. Executive summary
2. Key findings (prioritized)
3. Specific recommendations
4. Quick wins for immediate improvement
```

#### Example: style-check.prompt.md

Create a file at `.github/prompts/style-check.prompt.md`:

```markdown
---
description: Review content for style consistency and quality
---

Review the provided content for:

**Style Consistency:**
- Grammar, spelling, and punctuation
- Consistent terminology usage
- Appropriate tone and voice
- Proper markdown formatting

**Quality Factors:**
- Clarity and conciseness
- Logical organization
- Accessibility compliance
- Professional presentation

**Output Format:**
1. Overall assessment (1-10 score)
2. Specific issues found (with line references if applicable)
3. Suggested improvements
4. Style guide recommendations
```

#### Example: generate-summary.prompt.md

Create a file at `.github/prompts/generate-summary.prompt.md`:

```markdown
---
description: Create executive summaries of content or changes
---

Create a comprehensive summary of the provided content including:

**Key Information:**
- Main topics and themes
- Important facts or data points
- Key takeaways or conclusions

**Structure:**
- Logical organization of information
- Relationships between topics
- Priority or importance ranking

**Actionable Items:**
- Decisions that need to be made
- Next steps or follow-up actions
- Areas requiring attention

Format as a clear, executive-level summary suitable for stakeholder review.
```

#### Prompt File Features

Prompt files support advanced features:

- **File references**: Use `[filename](relative/path)` or `#file:relative/path` to include other files as context
- **Description**: The YAML `description` field appears in the prompt picker
- **Markdown formatting**: Use any Markdown formatting in the prompt body

> **Note:** Prompt files are currently in public preview. They work in VS Code, Visual Studio, and JetBrains IDEs.

## Step 2: Create the Configuration Files

### Option A: Using VS Code UI (Recommended)

1. **Open Copilot Chat** (Ctrl+Shift+I or Cmd+Shift+I)
2. Click the **agents dropdown** at the bottom of the chat view
3. Select **Configure Custom Agents...**
4. Click the **+** button to **Create new custom agent**
5. Choose **Workspace** to store in `.github/agents/`
6. Enter a filename (e.g., `documentation-auditor`)
7. Configure the agent profile in the newly created `.agent.md` file

### Option B: Manual File Creation

Create the directory structure:

```text
.github/
├── agents/
│   ├── documentation-auditor.agent.md
│   ├── style-enforcer.agent.md
│   └── content-strategist.agent.md
└── prompts/
    ├── content-audit.prompt.md
    ├── style-check.prompt.md
    └── generate-summary.prompt.md
```

Then:

1. Copy the example content from Step 1 into each file
2. **Reload VS Code** (or restart GitHub Copilot extension)

## Step 3: Test the Configuration

Open **Copilot Chat** and verify your agents appear in the agents dropdown.

### Test Your Agents

Try these commands in Copilot Chat:

```text
@documentation-auditor Please analyze the structure of the learn-pr/wwl folder
```

```text
@style-enforcer Review the learn-pr/wwl/get-started-lakehouses/index.yml file for style consistency
```

### Test Your Prompts

To use a prompt file in VS Code:

1. In Copilot Chat, type `#prompt:` to see available prompts
2. Select your prompt file (e.g., `content-audit`)
3. Optionally add more context by typing `#file:` or clicking **Add Context**
4. Type any additional instructions
5. Submit the prompt

Alternatively, in JetBrains IDEs, type `/prompt-name` directly (e.g., `/content-audit`).

**Example workflow:**

1. Attach the `content-audit` prompt
2. Also attach a folder or file for context
3. Submit to get a structured analysis

## Step 4: Understanding the Results

Notice how:

- **Agents respond with their specialized perspective** and expertise
- **Prompts provide structured, consistent output** following your templates
- **Workspace context (@workspace)** gives agents and prompts full repository awareness

## Reflection Questions

1. How do the agent responses differ from standard GitHub Copilot responses?
2. What makes the prompt-generated output more useful than ad-hoc queries?
3. How could you customize these agents and prompts for your specific needs?

## Next Steps

### LLA Live Session

- [Scenario 1: Audit](/scenario-1-inheritance/tasks/task-1.2-audit.md)
- [Scenario 1: Agent Analysis](/scenario-1-inheritance/tasks/task-1.5-agent-analysis.md)
- [Scenario 2: Categorize](/scenario-2-backlog-battle/tasks/task-2.1-categorize.md)
- [Scenario 2: Automated Fixes](/scenario-2-backlog-battle/tasks/task-2.4-automated-fixes.md)


### Self-paced and want to explore

Ready to create your own agents? Continue to [Task 0.2: Creating Your First Agent](task-0.2-custom-agent.md)

---

**Key Takeaway:** Agents and prompts transform GitHub Copilot into a specialized toolkit tailored to your workflow, providing consistent, expert-level assistance across all your documentation tasks.
