# Task 4.4: Quality Assurance Automation

## Objective

Build practical quality assurance workflows using **GitHub Actions** combined with **native Copilot features** for comprehensive content validation.

## Automation Strategy

| Layer | Tool | Purpose |
|-------|------|---------|
| **Automated Checks** | GitHub Actions | Linting, link checking, spell check |
| **AI Review** | Native Copilot Code Review | Content quality feedback |
| **AI Implementation** | Copilot Coding Agent | Automated fixes |
| **Custom Analysis** | Custom Agents (optional) | Specialized workflows |

## Part 1: Automated Linting with GitHub Actions

### Step 1: Create a Content Quality Workflow

Create `.github/workflows/content-qa.yml`:

```yaml
name: Content Quality Checks

on:
  pull_request:
    paths:
      - '**.md'
      - '**.yml'
      - '**.yaml'

jobs:
  markdown-lint:
    name: Markdown Linting
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run markdownlint
        uses: DavidAnson/markdownlint-cli2-action@v19
        with:
          globs: '**/*.md'

  yaml-lint:
    name: YAML Validation
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Validate YAML
        uses: ibiqlik/action-yamllint@v3
        with:
          file_or_dir: '**/*.yml'
          strict: false

  link-check:
    name: Link Validation
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Check for broken links
        uses: lycheeverse/lychee-action@v2
        with:
          args: --verbose --no-progress '**/*.md'
          fail: true

  spell-check:
    name: Spell Check
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Run cspell
        uses: streetsidesoftware/cspell-action@v6
        with:
          files: '**/*.md'
```

### Step 2: Configure Markdownlint Rules

Create `.markdownlint.json`:

```json
{
  "default": true,
  "MD013": false,
  "MD033": false,
  "MD041": false,
  "MD024": {
    "siblings_only": true
  }
}
```

**Rule explanations:**

- `MD013`: Disable line length limits (not practical for documentation)
- `MD033`: Allow inline HTML when needed
- `MD041`: Don't require first line to be a heading
- `MD024`: Allow duplicate headings in different sections

### Step 3: Add a Custom Words Dictionary

Create `.cspell.json` for spell-check configuration:

```json
{
  "version": "0.2",
  "language": "en",
  "words": [
    "Copilot",
    "GitHub",
    "frontmatter",
    "workflow",
    "repo",
    "markdown"
  ],
  "ignorePaths": [
    "node_modules/**",
    ".git/**"
  ]
}
```

## Part 2: Native Copilot Code Review Integration

### Step 1: Enable Automatic Copilot Reviews

Configure Copilot to automatically review all PRs:

1. Go to **Settings > Copilot > Code review** in your repository
2. Enable **Automatic reviews**
3. Configure file patterns if needed (e.g., only `**/*.md` files)

### Step 2: Customize Review Behavior with Instructions

Native Copilot code review uses your custom instructions automatically!

Ensure `.github/copilot-instructions.md` includes review guidance:

```markdown
## Code Review Guidelines

When performing code reviews on this repository:

### YAML Validation
- Verify required fields: uid, title, description, ms.date, author
- Check ms.date is within the last 12 months
- Validate unit references match actual files

### Markdown Quality
- Ensure proper heading hierarchy (H1 → H2 → H3)
- Verify all images have alt text
- Check code blocks specify language
- Validate internal links resolve correctly

### Content Standards
- Use "Microsoft Fabric" on first reference
- Use sentence case for headings
- Include prerequisites for hands-on content

### Priority Issues to Flag
1. Broken internal links (CRITICAL)
2. Missing or outdated ms.date (HIGH)
3. Code examples without language specifier (MEDIUM)
4. Missing image alt text (MEDIUM)
```

### Step 3: Use Copilot Coding Agent to Fix Issues

When Copilot code review finds issues, you can ask Copilot Coding Agent to fix them:

1. Review Copilot's feedback on the PR
2. Click "Implement suggestion" on any comment
3. Or comment `@copilot Please fix the issues identified in this review`
4. Copilot will create commits addressing the feedback

---

## Part 3: Custom PR Review Agent (Optional)

Only create a custom agent if native Copilot code review doesn't meet your needs.

**When you might need a custom PR review agent:**
- You want a specific output format not provided by native review
- You need to coordinate with other custom agents
- You want interactive VS Code-based review

Create `.github/agents/pr-content-reviewer.agent.md`:

```markdown
---
name: PR Content Reviewer
description: Specialized PR reviewer for Microsoft Learn content
tools:
  - changes
---

You are a content quality reviewer for Microsoft Learn pull requests.

## Review Checklist

### YAML Validation
- [ ] All required fields present (uid, title, description, ms.date)
- [ ] ms.date is current (within 12 months)
- [ ] Unit references match actual files

### Markdown Quality  
- [ ] Heading hierarchy is correct
- [ ] Images have descriptive alt text
- [ ] Code blocks have language specifiers
- [ ] Internal links are valid

### Content Standards
- [ ] Terminology is consistent
- [ ] Prerequisites are stated
- [ ] Code examples are complete

## Output Format

### Summary
**Recommendation:** [APPROVE / REQUEST CHANGES]
**Quality Score:** [X/10]

### Issues Found
| File | Issue | Severity | Suggested Fix |
|------|-------|----------|---------------|

### Positive Observations
- [What's done well]
```

**To use:** In VS Code Copilot Chat, select `@pr-content-reviewer` and attach the PR files.

---

### Step 2: Use the Agent for PR Reviews

When reviewing a pull request, invoke the agent:

```text
@pr-content-reviewer Review the changes in this PR for content quality
```

The agent will use the `changes` tool to see the diff and provide structured feedback.

## Part 3: Pre-Commit Quality Checks

### Step 1: Create a Quick Review Prompt

Create a file named `quick-quality-check.prompt.md` in your `.github/prompts/` folder:

```markdown
---
description: Quick quality check before committing changes
---

Review the current file for quality issues:

**Check for:**
- [ ] Spelling and grammar errors
- [ ] Broken or placeholder links
- [ ] Incomplete sections or TODOs
- [ ] Code blocks have language specified
- [ ] Headings follow logical hierarchy

**Report format:**
- ✅ [What's correct]
- ⚠️ [Warning - should fix]
- ❌ [Error - must fix]

Focus only on issues, skip what's already good.
```

### Step 2: Use Before Committing

Before committing documentation changes:

1. Open the file you're editing
2. Type `#prompt:quick-quality-check` in Copilot Chat
3. Fix any issues found
4. Commit with confidence

## Part 4: Automated Issue Templates

### Step 1: Create a Content Issue Template

Create `.github/ISSUE_TEMPLATE/content-improvement.yml`:

```yaml
name: Content Improvement
description: Suggest improvements to documentation
labels: ["documentation", "content"]
body:
  - type: dropdown
    id: type
    attributes:
      label: Improvement Type
      options:
        - Fix error or outdated information
        - Clarify confusing content
        - Add missing information
        - Improve formatting or structure
    validations:
      required: true

  - type: input
    id: file
    attributes:
      label: File Path
      description: Which file needs improvement?
      placeholder: docs/getting-started.md
    validations:
      required: true

  - type: textarea
    id: current
    attributes:
      label: Current Content
      description: What does it say now?
    validations:
      required: true

  - type: textarea
    id: suggested
    attributes:
      label: Suggested Change
      description: What should it say instead?
    validations:
      required: true
```

### Step 2: Create a PR Template

Create `.github/pull_request_template.md`:

```markdown
## Summary

Brief description of changes.

## Content Checklist

- [ ] Spelling and grammar checked
- [ ] Links verified working
- [ ] Code examples tested
- [ ] Follows style guide
- [ ] No placeholder content remaining

## Files Changed

- `path/to/file.md` - Description of change

## Testing

Describe how you verified the changes work correctly.
```

## Part 5: Quality Dashboard (Manual Tracking)

### Create a Quality Tracking Issue

Since GitHub doesn't have built-in dashboards, use a pinned issue to track quality metrics.

Create an issue titled "📊 Content Quality Tracker" with this template:

```markdown
# Content Quality Tracker

**Last Updated:** [Date]

## Quality Metrics

| Metric | Current | Target | Status |
|--------|---------|--------|--------|
| Markdown lint errors | 12 | 0 | 🟡 |
| Broken links | 0 | 0 | 🟢 |
| Spell check errors | 5 | 0 | 🟡 |
| Open content issues | 3 | <5 | 🟢 |

## Recent Improvements

- [Date]: Fixed broken links in getting-started.md
- [Date]: Updated outdated screenshots

## Priority Issues

- [ ] #123 - Fix API documentation
- [ ] #124 - Update installation guide

---
*Updated weekly by content team*
```

Pin this issue to keep it visible.

## Implementation Checklist

Complete these steps to set up your QA automation:

### GitHub Actions Setup

- [ ] Create `.github/workflows/content-qa.yml`
- [ ] Add `.markdownlint.json` configuration
- [ ] Add `.cspell.json` with custom words
- [ ] Verify workflow runs on a test PR

### Native Copilot Integration

- [ ] Enable automatic Copilot code reviews in repository settings
- [ ] Update `.github/copilot-instructions.md` with review guidelines
- [ ] Test native Copilot review on a PR

### Optional Custom Agent Setup

- [ ] Create `pr-content-reviewer.agent.md` (only if native review insufficient)
- [ ] Create `quick-quality-check.prompt.md` for VS Code workflow
- [ ] Test custom agent alongside native review

### Templates Setup

- [ ] Create issue template for content improvements
- [ ] Create PR template with quality checklist
- [ ] Create pinned quality tracking issue

### Verification

- [ ] Submit a test PR with intentional issues
- [ ] Confirm GitHub Actions catches linting errors
- [ ] Verify native Copilot review provides feedback
- [ ] Test Copilot Coding Agent fixing identified issues

## Success Criteria

- [ ] GitHub Actions workflow runs on every PR
- [ ] Native Copilot code review enabled and tested
- [ ] Custom instructions enhance review quality
- [ ] Copilot Coding Agent can fix review feedback
- [ ] Quality metrics are tracked and visible

## Congratulations

You've built a comprehensive QA automation system that leverages **native GitHub features first**:

| Layer | Tool | Status |
|-------|------|--------|
| **Automated Checks** | GitHub Actions (lint, links, spelling) | ✅ Runs on every PR |
| **AI Review** | Native Copilot Code Review | ✅ Automatic feedback |
| **AI Fixes** | Copilot Coding Agent | ✅ Implements suggestions |
| **Custom Analysis** | Custom Agents (when needed) | ✅ Specialized workflows |

This approach:
- **Prioritizes built-in features** that require less maintenance
- **Uses custom solutions** only when native features aren't sufficient
- **Automates the entire quality pipeline** from lint to AI review to fix

## Next Steps

With your automated QA system in place:

1. **Monitor the workflow** - Check Actions tab for failures
2. **Refine lint rules** - Adjust `.markdownlint.json` based on your team's preferences
3. **Expand the dictionary** - Add project-specific terms to `.cspell.json`
4. **Review quality trends** - Update your tracking issue weekly

---

**Achievement Unlocked**: QA Automation Master! You've built enterprise-grade quality automation using GitHub Actions and Copilot agents.
