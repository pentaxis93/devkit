# Agent Search Optimization (ASO)

How agents discover and select skills. Optimize for this flow:

1. Agent encounters problem ("tests are flaky")
2. Agent scans available skill descriptions
3. Description matches → agent loads SKILL.md
4. Agent scans overview (is this relevant?)
5. Agent reads patterns and instructions
6. Agent loads references only when implementing

## Writing the Description

The `description` frontmatter field is the primary discovery mechanism.
It must answer: "Should I read this skill right now?"

**Include:**
- Concrete triggers and situations that signal this skill applies
- Problem symptoms, not implementation-specific language
- Keywords agents would search for (error messages, tool names, synonyms)
- Technology-specific markers only if the skill itself is technology-specific

```yaml
# BAD: Too abstract
description: For async testing

# BAD: Technology-specific but skill isn't
description: Use when tests use setTimeout/sleep and are flaky

# GOOD: Problem-oriented triggers and symptoms
description: >-
  Use when tests have race conditions, timing dependencies, or
  pass/fail inconsistently. Covers condition-based waiting as an
  alternative to arbitrary timeouts.

# GOOD: Technology-specific skill with explicit scope
description: >-
  Use when building MCP servers to integrate external APIs or
  services, whether in Python (FastMCP) or Node/TypeScript (MCP SDK).
```

## Keyword Coverage

Use words agents would search for. Distribute them across the
description, overview, and section headers:

- **Error messages**: "Hook timed out", "ENOTEMPTY", "race condition"
- **Symptoms**: "flaky", "hanging", "zombie", "pollution"
- **Synonyms**: "timeout/hang/freeze", "cleanup/teardown/afterEach"
- **Tools**: actual commands, library names, file types

Mentioning key concepts in multiple places (description, overview,
headers) increases the chance of a search hit.

## Naming for Discovery

Skill directory names should be:

- **Verb-noun for actions**: `find-bugs`, `validate-pipeline`
- **Noun-compound for capabilities**: `feature-discovery`, `vision-workshop`
- **Tool-first for tool-scoped**: `beads-plan`, `mcp-builder`
- **Descriptive over generic**: `feature-discovery` not `utils`

## Token Efficiency

Skills loaded frequently must be concise. Every token in the description
and SKILL.md competes with conversation context.

**Target word counts:**
- Description: 30-80 words (always in context)
- SKILL.md body: < 500 lines (loaded on trigger)
- References: unlimited (loaded on demand)

**Techniques:**
- Reference `--help` instead of documenting all flags
- Cross-reference other skills instead of repeating their content
- Use one excellent example instead of multiple mediocre ones
- Eliminate redundancy between SKILL.md and references

## Attribution

Adapted from the Claude Search Optimization (CSO) section of
[obra/superpowers-skills/writing-skills](https://github.com/obra/superpowers-skills/tree/main/skills/meta/writing-skills),
generalized to Agent Search Optimization (ASO) for provider-agnostic use.
