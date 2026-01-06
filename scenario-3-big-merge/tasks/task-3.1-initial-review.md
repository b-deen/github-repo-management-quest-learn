# Task 3.1: Initial PR Review with GitHub Copilot

**Duration:** 15 minutes
**Difficulty:** Intermediate
**GitHub Copilot Features:** Native Copilot Code Review, @workspace, Copilot Chat

## Objective

Perform an initial high-level review of Pull Request #127 using GitHub Copilot's **native code review feature** as your first step, then supplement with @workspace analysis for deeper investigation.

## Context

When reviewing a large PR (+847/-312 lines across 18 files), you can't read every line carefully. GitHub now provides **native Copilot code review** that should be your starting point:

- **Native Copilot PR Review**: Add Copilot as a reviewer directly on GitHub.com - it provides inline feedback with suggested fixes
- **Automatic Reviews**: Configure Copilot to automatically review all PRs
- **Custom Instructions**: Tailor Copilot's review focus with `.github/copilot-instructions.md`
- **@workspace Analysis**: Use VS Code for deeper investigation of specific concerns

## Setup

### Prerequisites

**Before starting this task:** Ensure you have run the "Setup Quest PR" workflow to create the sample PR in your repository.

1. Go to your repository's **Actions** tab
2. Select "Setup Quest PR" workflow
3. Click "Run workflow"
4. Navigate to **Pull Requests** tab to find the created PR

### For This Task You'll Use:

1. **Native Copilot PR Review (Primary Method):**
   - Open the PR on GitHub.com
   - Click the **Reviewers** dropdown
   - Select **Copilot** to request a code review
   - Wait ~30 seconds for Copilot to analyze the PR

2. **VS Code with Copilot Chat (Secondary Method):**
   - Open your repository in VS Code
   - Press `Ctrl+Shift+I` (Windows/Linux) or `Cmd+Shift+I` (Mac)

3. **PR Content (created by the workflow):**
   - New content files: `copilot-advanced-scenarios.md`, `copilot-best-practices.md`, `copilot-troubleshooting.md`
   - Modified files: Updated `index.yml` with new units for the data agents module
   - All content relates to `learn-pr/wwl/implement-fabric-data-agents`

**Context:** This PR adds advanced Copilot content to the `implement-fabric-data-agents` Learn module, including new tutorials on advanced scenarios, best practices, and troubleshooting.

## Tasks

### 1. Use Native Copilot PR Review (Recommended First Step)

**On GitHub.com:**

1. Navigate to your PR titled `[Quest Sample] Add advanced Copilot data agent content`
2. Click the **Reviewers** gear icon in the right sidebar
3. Select **Copilot** from the dropdown
4. Wait approximately 30 seconds for Copilot to analyze the PR
5. Scroll through the PR to see Copilot's inline review comments

**What Native Copilot Review Provides:**
- Inline comments on potential issues in the code/content
- Suggested fixes you can apply with one click
- Feedback organized by file
- Comments behave like human review comments (you can react, reply, resolve)

**Key Benefits:**
- ✅ No prompts to write - Copilot analyzes automatically
- ✅ Suggested changes can be applied directly
- ✅ Works on any PR without VS Code setup
- ✅ Review comments are visible to all collaborators

> **Note:** Copilot always leaves a "Comment" review, not "Approve" or "Request Changes", so it won't block merging.

**Deliverable:** Note which files Copilot flagged and what types of issues it found

---

### 2. (Optional) Configure Custom Review Instructions

If you want Copilot to focus on specific aspects, add custom instructions:

**Create `.github/copilot-instructions.md`:**

```markdown
When performing a code review, apply the following checks:

- Verify YAML frontmatter follows Microsoft Learn schema (title, description, ms.date, author)
- Check that ms.date values are within the last 12 months
- Ensure all internal links use relative paths
- Verify code blocks specify language (python, sql, yaml)
- Check for consistent terminology (Microsoft Fabric, not just Fabric)
- Flag any placeholder text like TODO, TBD, or [INSERT]
```

Once saved, Copilot will use these instructions for all code reviews in the repository.

---

### 3. Generate Deep Analysis with @workspace

**After reviewing Copilot's native feedback**, use @workspace in VS Code for deeper investigation:

**GitHub Copilot Chat Prompt:**

```
@workspace I'm reviewing a pull request with these changes:

[Open and share PR_DESCRIPTION.md and FILE_LIST.md with Copilot]

Analyze this PR and provide:
1. A 2-3 sentence summary of what this PR accomplishes
2. The main categories of changes (new features, updates, refactoring, deletions)
3. Why this change is needed (based on the description)
4. Whether the scope seems appropriate for a single PR or should be split
5. The overall complexity level (low/medium/high)

Format as a structured markdown summary.
```

**What Copilot Does:**
- Analyzes the PR description and file changes
- Identifies the core objectives
- Assesses scope appropriateness
- Provides context about the change's purpose

**Using GitHub.com PR Summary Feature:**

In a real-world scenario with an actual PR on GitHub.com:
1. Navigate to the PR page on GitHub
2. Look for the "Copilot" button or "Summary" option in the PR header
3. Click to get an AI-generated PR summary
4. Use this as your starting point for review

**Deliverable:** Create `pr-review.md` with "PR Overview" section

---

### 2. Analyze Change Scope and Impact

**GitHub Copilot Chat Prompt:**

```
@workspace For this PR, analyze the file changes:

New Files (3):
- copilot-advanced-scenarios.md
- copilot-best-practices.md
- copilot-troubleshooting.md

Modified Files (8):
[Paste the modified files list from FILE_LIST.md - includes index.yml, data-agents.md, summary.md, etc.]

Deleted Files (1):
- legacy-copilot-overview.md

Provide:
1. Categorization by change type (new content, content updates, restructuring, deletions)
2. Risk assessment for each category (low/medium/high risk)
3. Files that are high-risk and need careful review
4. Potential breaking changes (links, navigation, module structure)
5. Is this PR appropriately scoped or trying to do too much?
6. What areas need the most careful review?

Present as a table and risk assessment.
```

**What Copilot Does:**
- Categorizes changes by type and risk
- Identifies high-risk areas
- Flags potential breaking changes
- Assesses PR scope appropriateness

**Follow-up to Understand Dependencies:**

```
@workspace If we delete legacy-copilot-overview.md, what other files in this Learn module might reference it? What [!include[]] references or links will break?
```

**Deliverable:** Add "Change Scope Analysis" section to `pr-review.md`

---

### 3. Spot Check Content Quality

**GitHub Copilot Chat Prompt:**

```
@workspace I'm spot-checking quality for this PR. Review these sample files:

[Share 2-3 files from new-files/ and modified-files/ - one new tutorial like copilot-advanced-scenarios.md, one modified doc like data-agents.md]

For each file, check:
1. Writing quality (clarity, grammar, professionalism)
2. Formatting consistency (headings, code blocks, lists follow Learn standards)
3. Technical accuracy (Fabric code examples, PySpark/SQL correctness)
4. Completeness (no TODO markers, all sections finished)
5. Code examples (properly formatted, appear functional for Fabric notebooks)

Provide:
- Individual assessment for each file
- Overall quality rating
- Patterns of issues that might exist across the PR
- Specific examples of good and bad practices found
```

**What Copilot Does:**
- Reviews writing quality and clarity
- Checks formatting standards
- Identifies technical issues
- Finds patterns that may affect other files

**Using Inline Copilot for Quick Checks:**

1. Open a sample file from the PR
2. Select a section you want to check
3. Press `Ctrl+I` (Cmd+I)
4. Ask: "Is this technically accurate and well-written?"
5. Review Copilot's feedback

**Deliverable:** Add "Initial Quality Assessment" section to `pr-review.md`

---

### 4. Identify Cross-File Impact and Risks

**GitHub Copilot Chat Prompt:**

```
@workspace This PR makes significant changes across 18 files, including restructuring and deletions.

Changed files:
[Reference FILE_LIST.md]

What potential issues should I watch for during detailed review?

Analyze for:
1. Broken links (deleted/moved files, references not updated)
2. Navigation issues (TOCs, menus, cross-references out of sync)
3. Inconsistent style (mixing old and new formatting conventions)
4. Missing coordinated updates (updating A but not related B)
5. Version mismatches (code examples using different versions)
6. Orphaned content (references to deleted content)
7. Impact on existing documentation outside this PR

For each risk, specify:
- What to check
- Which files are affected
- How to validate
- Priority level (P0-P3)
```

**What Copilot Does:**
- Identifies cross-file dependencies
- Flags potential broken links
- Finds navigation inconsistencies
- Highlights missing updates

**Advanced @workspace Query:**

```
@workspace Are there any files in the repository (not in this PR) that link to or depend on guides/legacy-setup.md which is being deleted?
```

**Deliverable:** Add "Risk Areas to Review" section to `pr-review.md`

---

### 5. Create a Time-Boxed Review Strategy

**GitHub Copilot Chat Prompt:**

```
@workspace Based on my analysis so far, help me create an efficient review strategy.

High-risk files identified:
[List files Copilot identified as high-risk]

New content files:
[List new tutorials/docs]

Minor update files:
[List files with small changes]

I have 45 minutes for detailed review. Create a prioritized review plan:

1. Which files MUST be reviewed in detail? (list in priority order)
2. Which files only need a quick scan?
3. What should I check with automated tools vs manual review?
4. What specific items require manual validation?
5. How should I allocate my 45 minutes across these activities?

Provide a concrete time-boxed plan with checkboxes I can follow.
```

**What Copilot Does:**
- Prioritizes files by risk and importance
- Allocates time based on complexity
- Separates automated vs manual checks
- Creates actionable review checklist

**Deliverable:** Add "Review Strategy" section to `pr-review.md`

---

### 6. Make Preliminary Assessment

**GitHub Copilot Chat Prompt:**

```
@workspace Based on this high-level review:

PR Objectives:
[Summarize from overview]

Scope Analysis:
[Key findings]

Quality Spot Check:
[Quality assessment]

Risk Areas:
[Major risks identified]

Provide a preliminary PR assessment:

1. Initial recommendation (Approve / Request Changes / Needs Major Work)
2. Confidence level in this assessment (High/Medium/Low)
3. Top 3 concerns that could change the recommendation
4. What would make this an easy approval?
5. Deal-breakers that MUST be fixed before merge
6. Estimated effort for the PR author to address feedback

Note: This is preliminary - detailed review may change this assessment.
```

**What Copilot Does:**
- Synthesizes all findings into a recommendation
- Identifies blockers vs nice-to-haves
- Sets clear expectations
- Helps you communicate findings

**Polish Your Assessment:**

1. Select your preliminary assessment text
2. Press `Ctrl+I` (Cmd+I)
3. Ask: "Make this more diplomatic and constructive"
4. Review and accept improvements

**Deliverable:** Add "Preliminary Assessment" section to `pr-review.md`

---

## Using GitHub.com's Native PR Features

### GitHub Copilot PR Summary (Real-World Workflow)

When reviewing actual PRs on GitHub.com:

1. **Navigate to the PR**
   ```bash
   gh pr view 127 --web
   ```

2. **Click the "Copilot" or "Summary" Button**
   - Located in the PR header or sidebar
   - Generates AI-powered summary of changes

3. **Review the Auto-Generated Summary:**
   - Main changes overview
   - Files modified by category
   - Potential issues flagged
   - Testing suggestions

4. **Use Copilot Chat for Deeper Analysis**
   - Click "Open in Copilot Chat" for interactive Q&A
   - Ask follow-up questions about specific changes
   - Request impact analysis

**Example Questions in GitHub PR Copilot:**
- "What are the main changes in this PR?"
- "What files have the highest risk?"
- "Are there any breaking changes?"
- "What should I focus on during review?"

---

## Output Format

Your `pr-review.md` should follow this structure:

```markdown
# Pull Request #127 Review

**Reviewer:** [Your Name]
**Date:** [Date]
**PR Title:** Major documentation update - new features and restructuring
**Status:** Initial Review Complete
**Reviewed with:** GitHub Copilot @workspace

---

## PR Overview

### Summary
[2-3 sentences describing what this PR does - from Copilot]

### Objectives
- [Objective 1]
- [Objective 2]
- [Objective 3]

### Justification
[Why is this change needed? - Copilot's analysis]

### Scope Assessment
[Copilot's assessment of whether PR is appropriately scoped]

**Complexity Level:** [Low/Medium/High]

---

## Change Scope Analysis

### Changes by Type

| Type | Count | Risk Level | Files |
|------|-------|------------|-------|
| New Files | 3 | Medium | tutorials/... |
| Modified Files | 8 | High | docs/api-reference.md, ... |
| Deleted Files | 1 | High | guides/legacy-setup.md |
| **Total** | **12** | | |

### Change Categories
- **New Content (40%):** 3 new tutorials on integrations, error handling, workflow optimization
- **Content Updates (45%):** API reference, getting started, quick start guides
- **Restructuring (10%):** Navigation and organization changes
- **Deletions (5%):** Removing outdated legacy setup guide

### High-Risk Changes

**File Deletions:**
- `guides/legacy-setup.md` - May have existing links
- **Action:** Validate no broken references

**Major Content Changes:**
- `docs/api-reference.md` (+123, -67 lines)
- **Risk:** Breaking changes to API documentation
- **Action:** Verify technical accuracy

---

## Initial Quality Assessment

### Sample Files Reviewed

#### 1. tutorials/advanced-integrations.md (New)
- **Writing Quality:** ✅ Professional, clear
- **Formatting:** ✅ Consistent, proper headings
- **Technical Accuracy:** ⚠️  Need to verify code examples
- **Completeness:** ✅ All sections complete

#### 2. docs/api-reference.md (Modified - Major)
- **Writing Quality:** ✅ Good
- **Formatting:** ⚠️  Some inconsistent code block languages
- **Technical Accuracy:** ❓ Needs validation
- **Completeness:** ✅ Complete

#### 3. docs/quick-start.md (Modified - Minor)
- **Writing Quality:** ✅ Excellent
- **Formatting:** ✅ Perfect
- **Technical Accuracy:** ✅ Appears accurate
- **Completeness:** ✅ Complete

### Overall Quality
- **Writing Quality:** 4/5 - Generally professional and clear
- **Technical Accuracy:** 3/5 - Needs verification in detailed review
- **Formatting Consistency:** 3.5/5 - Some minor issues
- **Completeness:** 5/5 - No incomplete sections found

### Observed Patterns
- Code blocks occasionally missing language specifiers
- Generally high quality content
- Some sections could be more concise
- Good use of examples and diagrams

---

## Risk Areas to Review

### 1. Broken Links from Deleted Files (Priority: P0)
- **What:** `guides/legacy-setup.md` is deleted
- **Why it matters:** Other docs may link to this file
- **Files potentially affected:** All guides, getting started, FAQs
- **Validation:** Search for "legacy-setup" references across all docs
- **Tool:** @workspace to find all references

### 2. API Documentation Accuracy (Priority: P0)
- **What:** Major changes to api-reference.md
- **Why it matters:** Incorrect API docs break developer trust
- **Files affected:** docs/api-reference.md, related examples
- **Validation:** Test code examples, verify endpoints
- **Tool:** Manual testing + Copilot review

### 3. Cross-Reference Consistency (Priority: P1)
- **What:** Restructuring may break internal links
- **Why it matters:** Navigation and user experience
- **Files affected:** All modified docs
- **Validation:** Link checker + manual verification
- **Tool:** markdown-link-check, @workspace

### 4. Version Consistency (Priority: P1)
- **What:** Code examples may reference different versions
- **Why it matters:** Confuses users about which version to use
- **Files affected:** All tutorials and examples
- **Validation:** Check all version references match
- **Tool:** @workspace search

### 5. Navigation and TOC Updates (Priority: P2)
- **What:** New tutorials may not be in navigation
- **Why it matters:** Users won't find new content
- **Files affected:** README.md, navigation files
- **Validation:** Check all nav elements updated
- **Tool:** Manual review

---

## Review Strategy

### Time Allocation (45 minutes total)

#### Priority 1: Critical Reviews (25 min)
- [ ] **docs/api-reference.md** (10 min) - Verify technical accuracy of major changes
- [ ] **Search for broken links** (8 min) - @workspace query for deleted file references
- [ ] **tutorials/advanced-integrations.md** (7 min) - Review new content quality

#### Priority 2: Important Reviews (12 min)
- [ ] **docs/getting-started.md** (4 min) - Check consistency
- [ ] **tutorials/error-handling-guide.md** (4 min) - Review new content
- [ ] **tutorials/workflow-optimization.md** (4 min) - Review new content

#### Priority 3: Quick Scans (5 min)
- [ ] Minor update files (quick-start.md, etc.)
- [ ] Formatting consistency check across all files

#### Automated Checks (3 min)
- [ ] Run markdown link checker on all changed files
- [ ] Run markdown linter to check formatting
- [ ] Search for TODO/FIXME markers

### Manual Validation Required
- [ ] Test API code examples
- [ ] Verify all cross-references
- [ ] Check navigation includes new tutorials
- [ ] Confirm version numbers consistent

### Copilot-Assisted Validation
```
@workspace Find all references to guides/legacy-setup.md
@workspace Check if navigation files include the new tutorials
@workspace Search for version number references - are they consistent?
```

---

## Preliminary Assessment

**Initial Recommendation:** Request Changes

**Confidence:** Medium (need detailed review to confirm)

**Main Concerns:**
1. **Deleted file impact:** Must verify no broken links from deleting legacy-setup.md
2. **API accuracy:** Major API reference changes need validation
3. **Missing navigation updates:** New tutorials may not be discoverable

**Potential Deal-Breakers:**
- Broken links from deleted file (if many exist)
- Technically incorrect API documentation
- Untested code examples that don't work

**Path to Approval:**
- Fix all broken links
- Validate API documentation accuracy
- Update navigation to include new tutorials
- Test all code examples
- Address formatting inconsistencies

**Estimated Effort for Author:**
- If only minor issues: 1-2 hours
- If broken links widespread: 3-4 hours
- If API docs have errors: 4-6 hours

---

## Next Steps

1. ✅ High-level review complete
2. ⏭️ Proceed with detailed content review (Task 2.2)
3. ⏭️ Validate cross-references and links (Task 2.3)
4. ⏭️ Compile comprehensive feedback (Task 2.4)

---

## Copilot Queries Used

Document the @workspace queries that were most helpful:

1. `@workspace Analyze this PR and summarize the changes`
2. `@workspace Find all references to guides/legacy-setup.md`
3. `@workspace Are there version inconsistencies in code examples?`
4. `@workspace Which files would be impacted by these changes?`

---

*This is a preliminary assessment based on high-level review with GitHub Copilot. Detailed analysis may reveal additional issues or confirm this assessment.*
```

---

## GitHub Copilot Tips for This Task

### Effective @workspace Usage for PR Review

**Do:**
- Start with broad questions, then narrow down
- Ask about cross-file impacts and dependencies
- Request specific file lists and references
- Use @workspace to understand repository context

**Example Progression:**
1. `@workspace Summarize this PR`
2. `@workspace What files reference the deleted file?`
3. `@workspace Check if navigation is updated`
4. `@workspace Find version number inconsistencies`

### Using GitHub's Native PR Summary

**When available on GitHub.com:**
- Use as your starting point (saves 5-10 minutes)
- Validate AI summary with your own spot checks
- Ask follow-up questions in Copilot Chat
- Compare AI findings with your analysis

### Iterative Review Approach

Don't try to catch everything at once:
1. **First pass:** Understand what changed (use Copilot)
2. **Second pass:** Identify risks (use @workspace)
3. **Third pass:** Spot check quality (sample files)
4. **Fourth pass:** Plan detailed review (prioritize)

---

## Success Criteria

You've completed this task when you:

- ✅ Used @workspace to understand PR scope and objectives
- ✅ Identified high-risk files requiring detailed review
- ✅ Conducted quality spot checks on sample files
- ✅ Found potential cross-file issues and broken links
- ✅ Created a time-boxed review strategy
- ✅ Made a preliminary recommendation with clear reasoning
- ✅ Can explain the PR to someone else in 2 minutes

---

## Time Management

- **Minutes 0-3:** Use @workspace to generate PR overview and understand objectives
- **Minutes 4-7:** Analyze change scope and assess risk levels with Copilot
- **Minutes 8-11:** Spot check 2-3 files with Copilot assistance
- **Minutes 11-13:** Identify cross-file risks and impacts with @workspace
- **Minutes 14-15:** Create review strategy and make preliminary assessment

---

## What's Next?

After completing your high-level review, you'll move to **Task 2.2** where you'll use GitHub Copilot to conduct detailed content review of the high-priority files you've identified.

---

## Troubleshooting

**Problem:** @workspace doesn't see the PR files
**Solution:** Make sure you've opened the `pr-content` folder in VS Code, not just individual files

**Problem:** Copilot's PR summary is too generic
**Solution:** Provide more context in your prompts:
- Share the PR description
- List specific files changed
- Ask targeted questions

**Problem:** Can't access GitHub.com PR summary feature
**Solution:** For this quest, use @workspace in VS Code with the provided PR files. In real scenarios, ensure you're logged into GitHub Copilot on GitHub.com.

---

**Ready for the next step?** Continue to [Task 3.2: Detailed Content Review](task-3.2-detailed-review.md).
