---
name: skill-creator
description: >-
  Guide for creating effective agent skills using BDD (Behavior-Driven
  Development). Use when creating new skills, editing existing skills,
  verifying skills work before deployment, or improving skill
  discoverability. Use when agents fail to follow workflows, when a skill
  doesn't get discovered, or when discipline skills get rationalized away.
  Covers skill anatomy, agent search optimization (ASO), behavioral testing
  with sub-agents, and hardening against rationalization.
---

# Creating Skills

Guide for creating effective agent skills using BDD.

## What Skills Are

Skills are modular packages that extend agent capabilities. They serve
two distinct purposes:

1. **Knowledge packages** -- domain-specific information agents lack
   (API docs, schemas, tool workflows)
2. **Behavior enforcement** -- specific workflows and discipline that
   agents would otherwise shortcut or rationalize away

**Skills are:** reusable techniques, patterns, tools, reference guides,
enforceable workflows.

**Skills are not:** narratives about how a problem was solved once,
project-specific conventions (those go in CLAUDE.md / AGENTS.md), or
standard practices well-documented elsewhere.

## Skill Types

| Type | Purpose | Freedom | Testing |
|------|---------|---------|---------|
| **Technique** | Concrete method with steps | Medium | Application scenarios, edge cases |
| **Pattern** | Mental model for problems | High | Recognition, counter-examples |
| **Reference** | API docs, syntax, tools | High | Retrieval accuracy, application |
| **Discipline** | Behavior enforcement | Low | Pressure scenarios, rationalization capture |

Identify the type early -- it determines how prescriptive to write, how
to test, and whether hardening is needed.

## Core Principles

**Context budget is finite.** Skills share the context window with
conversation history, system prompts, and other skills. Only add what
agents don't already know. Challenge each paragraph: does this justify
its token cost?

**Test before shipping.** BDD for documentation: observe agent behavior
without the skill (RED), write skill addressing specific failures (GREEN),
close loopholes (REFACTOR). See [references/testing.md](references/testing.md).

**Portability.** Use standard frontmatter (`name` + `description`). Avoid
framework-specific paths or variables. Write for "agents" not a specific
provider.

**Progressive disclosure.** Description always in context (~100 words).
SKILL.md body loads on trigger (< 500 lines). References load on demand
(unlimited). See [references/structure.md](references/structure.md).

## Skill Anatomy

```
skill-name/
├── SKILL.md           # Required, < 500 lines
├── scripts/           # Executable code
├── references/        # Docs loaded on demand
└── assets/            # Files used in output
```

### SKILL.md

Required. Two parts:

- **Frontmatter** (YAML): `name` and `description`. These are the only
  fields agents read to decide whether to load the skill. Write them
  for discoverability. See [references/discovery.md](references/discovery.md).
- **Body** (Markdown): Instructions and guidance. Loaded only after the
  skill triggers.

### Bundled Resources

**scripts/** -- Executable code for tasks that need deterministic
reliability or would be rewritten repeatedly. May be executed without
loading into context.

**references/** -- Documentation loaded on demand. For heavy reference
material (> 100 lines), API docs, detailed guides. Keep SKILL.md lean
by moving details here. Include clear descriptions of when to load each
file.

**assets/** -- Files used in output (templates, images, fonts). Not
loaded into context; used by agents when producing output.

**Do not include:** README.md, CHANGELOG.md, INSTALLATION_GUIDE.md, or
other auxiliary documentation. The skill should contain only what an
agent needs to do the job.

## Writing Effective Descriptions (ASO)

The `description` field is the primary discovery mechanism. Agents read
it to decide whether to load the skill. Optimize for Agent Search
Optimization (ASO):

- **Include trigger conditions**: "Use when creating new skills..."
- **Include symptoms**: "...when agents fail to follow workflows..."
- **Include keywords**: error messages, tool names, synonyms
- **Describe the problem**, not the implementation

```yaml
# BAD: Too abstract
description: For skill development

# GOOD: Triggers, symptoms, keywords
description: >-
  Guide for creating effective agent skills using BDD. Use when
  creating new skills, editing existing skills, or verifying skills
  work before deployment.
```

See [references/discovery.md](references/discovery.md) for the full ASO guide.

## Degrees of Freedom

Match prescriptiveness to the skill type:

**High freedom** (patterns, references): multiple valid approaches,
decisions depend on context, heuristics guide the work. Use text
instructions and examples.

**Medium freedom** (techniques): preferred pattern exists, some variation
acceptable. Use pseudocode or parameterized scripts.

**Low freedom** (discipline): operations are fragile, consistency is
critical, specific sequence must be followed. Use explicit rules, red
flags, rationalization counters.

Think of the agent as walking a path: a narrow bridge needs guardrails
(low freedom), an open field allows many routes (high freedom).

## Creation Process (BDD Cycle)

This cycle applies to **new skills and edits to existing skills**. Editing
a skill without testing is the same violation as creating one without
testing. If the change is substantial, return to RED.

### 1. Understand with Concrete Examples

Before creating a skill, understand how it will be used:

- What triggers the skill? What would an agent search for?
- What does good output look like?
- What are the failure modes?
- Who is the audience? (Other agents, not humans)

Ask for concrete examples. If working with a user, ask:
- "Can you give examples of how this skill would be used?"
- "What would an agent be doing when it should reach for this skill?"

### 2. Identify Skill Type

Classify as technique, pattern, reference, or discipline. This
determines:
- How prescriptive to write (degrees of freedom)
- How to test (application vs. pressure scenarios)
- Whether hardening is needed (discipline only)

### 3. RED: Baseline Without Skill

Dispatch a sub-agent with a realistic scenario. Do NOT load the skill.
Observe natural behavior:

```
Task tool:
  prompt: |
    [Realistic scenario that the skill addresses]
    [No mention of the skill or its rules]
    [Force a concrete decision or output]
```

Document:
- What choices did the agent make?
- What rationalizations did it use? (Capture verbatim)
- Where did it fail or take shortcuts?

**This is mandatory.** If you didn't watch an agent fail without the
skill, you don't know what the skill needs to prevent.

For discipline skills, use pressure scenarios combining 3+ pressures
(time, sunk cost, authority, exhaustion). See
[references/testing.md](references/testing.md).

### 4. Plan Reusable Contents

Based on the baseline failures, identify what the skill needs:
- What would agents re-derive every time without the skill?
- What scripts would be rewritten repeatedly?
- What reference material is needed but hard to find?
- What workflow steps get skipped under pressure?

### 5. GREEN: Write Minimal Skill

Address the specific baseline failures observed in RED. Do not add
content for hypothetical cases -- write just enough to fix what you saw.

Implement reusable resources (`scripts/`, `references/`, `assets/`)
before writing SKILL.md -- this may require user input (e.g., brand
assets, API docs). Test bundled scripts by running them before
committing.

Follow the anatomy above. Keep SKILL.md under 500 lines. Move heavy
reference material to `references/`. Use imperative mood throughout.

Consult these references based on the skill's needs:
- **Multi-step processes**: See [references/workflows.md](references/workflows.md)
- **Formatted output**: See [references/output-patterns.md](references/output-patterns.md)
- **File organization**: See [references/structure.md](references/structure.md)

#### Recommended SKILL.md Body Sections

```markdown
# Skill Name

## Overview
Core principle in 1-2 sentences. What is this?

## When to Use
Symptoms and use cases (bullet list).
When NOT to use.
Small inline flowchart IF decision is non-obvious.

## Core Pattern (for techniques/patterns)
Before/after comparison or key workflow.

## Quick Reference
Table or bullets for scanning common operations.

## Implementation
Detailed steps, inline code for simple patterns,
links to reference files for heavy material.

## Common Mistakes
What goes wrong + how to fix it.
```

Not every section is needed for every skill. Adapt to the skill type.

### 6. Verify GREEN

Dispatch sub-agent WITH the skill loaded. Same scenarios as RED:

```
Task tool:
  prompt: |
    You have access to: [skill path]
    [Same scenario as RED phase]
```

Agent should now succeed or comply. If not, the skill is unclear --
revise and re-test.

### 7. REFACTOR: Close Loopholes

If the agent found new ways to fail or rationalize:

- Capture new rationalizations verbatim
- Add explicit counters for each loophole
- For discipline skills: build rationalization table, red flags list
- See [references/persuasion-principles.md](references/persuasion-principles.md)
- Re-test until bulletproof

### 8. Iterate from Real Usage

After deployment, use the skill on real tasks:
1. Notice struggles or inefficiencies
2. Identify how SKILL.md or resources should change
3. Implement changes
4. Re-test (return to RED if the change is substantial)

## Naming Conventions

- **Descriptive over generic**: `feature-discovery` not `utils`
- **Lowercase hyphenated**: `root-cause-tracing` not `rootCauseTracing`
- **Descriptive over generic**: `condition-based-waiting` not `async-helpers`
- **Directory name = frontmatter name**: must match exactly

## Flowchart Usage

Use flowcharts only for non-obvious decision points, process loops where
you might stop too early, or "when to use A vs B" decisions.

Never use flowcharts for reference material (use tables), code examples
(use markdown blocks), or linear instructions (use numbered lists).

See [references/graphviz-conventions.dot](references/graphviz-conventions.dot)
for style rules.

## Anti-Patterns

| Anti-Pattern | Why It's Bad |
|-------------|--------------|
| Narrative storytelling | "In session X, we found..." -- too specific, not reusable |
| Multi-language dilution | One excellent example beats five mediocre ones |
| Code in flowcharts | Can't copy-paste, hard to read |
| Generic labels | step1, helper2 -- labels need semantic meaning |
| Auxiliary docs | README.md, CHANGELOG.md add clutter |
| Skipping RED phase | "Obviously clear" -- clear to you ≠ clear to agents |
| Stopping after first GREEN | First pass ≠ bulletproof |
| Hypothetical content | Write for observed failures, not imagined ones |

## Attribution

Adapted from:
- [anthropics/skills/skill-creator](https://github.com/anthropics/skills/tree/main/skills/skill-creator) -- structure, progressive disclosure, degrees of freedom, skill anatomy
- [obra/superpowers-skills/writing-skills](https://github.com/obra/superpowers-skills/tree/main/skills/meta/writing-skills) -- BDD methodology, ASO, skill types, hardening, anti-patterns
- [obra/superpowers-skills/testing-skills-with-subagents](https://github.com/obra/superpowers-skills/tree/main/skills/meta/testing-skills-with-subagents) -- pressure testing, rationalization capture, RED-GREEN-REFACTOR for docs
