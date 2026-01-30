# Output Patterns

Use these patterns when skills need to produce consistent, high-quality
output.

## Template Pattern

Provide templates for output format. Match strictness to the skill's
needs.

**Strict requirements** (API responses, data formats, commit messages):

```markdown
## Report structure

ALWAYS use this exact template structure:

# [Analysis Title]

## Executive summary
[One-paragraph overview of key findings]

## Key findings
- Finding 1 with supporting data
- Finding 2 with supporting data
- Finding 3 with supporting data

## Recommendations
1. Specific actionable recommendation
2. Specific actionable recommendation
```

**Flexible guidance** (when adaptation is useful):

```markdown
## Report structure

Sensible default format -- use your best judgment:

# [Analysis Title]

## Executive summary
[Overview]

## Key findings
[Adapt sections based on what you discover]

## Recommendations
[Tailor to the specific context]

Adjust sections as needed for the specific analysis type.
```

## Examples Pattern

For skills where output quality depends on seeing examples, provide
input/output pairs:

```markdown
## Commit message format

Generate commit messages following these examples:

**Example 1:**
Input: Added user authentication with JWT tokens
Output:
feat(auth): Implement JWT-based authentication

Add login endpoint and token validation middleware

**Example 2:**
Input: Fixed bug where dates displayed incorrectly in reports
Output:
fix(reports): Correct date formatting in timezone conversion

Use UTC timestamps consistently across report generation

Follow this style: type(scope): brief description, then detailed
explanation.
```

Examples help agents understand the desired style and level of detail
more clearly than descriptions alone.

## Attribution

Adapted from
[anthropics/skills/skill-creator/references/output-patterns.md](https://github.com/anthropics/skills/tree/main/skills/skill-creator).
