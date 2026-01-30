# Skill Structure and Progressive Disclosure

How to organize skill content for efficient context usage.

## Three-Level Loading

Skills use progressive disclosure to manage context:

1. **Metadata** (`name` + `description`) -- always in context (~100 words)
2. **SKILL.md body** -- loaded when skill triggers (< 500 lines)
3. **Bundled resources** -- loaded on demand by the agent (unlimited)

Only level 1 is always present. Level 2 loads when the agent decides the
skill is relevant. Level 3 loads only when the agent needs specific
reference material while working.

## When to Split Content

Move content from SKILL.md to references when:

- A section exceeds 100 lines
- Content supports a specific variant, framework, or domain
- Material is heavy reference (API docs, schemas, specifications)
- Details are needed only for specific sub-tasks

Keep in SKILL.md:
- Core workflow and decision guidance
- Principles and concepts
- Quick reference tables
- Code patterns under 50 lines
- Navigation pointers to reference files

## Progressive Disclosure Patterns

### Pattern 1: High-level guide with references

```markdown
# PDF Processing

## Quick start
Extract text with pdfplumber:
[code example]

## Advanced features
- **Form filling**: See [references/forms.md](references/forms.md)
- **API reference**: See [references/api.md](references/api.md)
```

Agent loads references only when the task requires them.

### Pattern 2: Domain-specific organization

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── references/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    └── product.md (API usage, features)
```

When asked about sales metrics, the agent loads only `sales.md`.
This applies equally to multi-framework skills:

```
cloud-deploy/
├── SKILL.md (workflow + provider selection)
└── references/
    ├── aws.md
    ├── gcp.md
    └── azure.md
```

### Pattern 3: Conditional details

```markdown
# DOCX Processing

## Creating documents
Use docx-js. See [references/docx-js.md](references/docx-js.md).

## Editing documents
For simple edits, modify XML directly.
**For tracked changes**: See [references/redlining.md](references/redlining.md)
```

Agent reads the redlining reference only when the user needs that feature.

## File Organization Guidelines

- **Keep references one level deep** from SKILL.md. All reference files
  should be linked directly from SKILL.md, not nested in sub-references.
- **Add a table of contents** to reference files longer than 100 lines
  so the agent can see the full scope when previewing.
- **Describe when to load** each reference file in SKILL.md. Don't just
  link -- explain what it contains and when it's needed.
- **Avoid duplication** between SKILL.md and references. Information
  should live in one place. Prefer references for detailed material;
  keep only essential workflow guidance in SKILL.md.
- **Include grep patterns for large files** (> 10k words). Add search
  hints in SKILL.md so agents can locate relevant sections without
  loading the entire file into context.

## Attribution

Adapted from the progressive disclosure patterns in
[anthropics/skills/skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator).
