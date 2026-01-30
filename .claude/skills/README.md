# Agent Skills

Reusable skill definitions for AI coding agents. Compatible with both
[Claude Code](https://docs.anthropic.com/en/docs/claude-code/skills) and
[OpenCode](https://opencode.ai/docs/skills/) via the shared
`.claude/skills/<name>/SKILL.md` discovery path.

## Pipeline

The core planning-to-execution pipeline transforms ideas into working
software through five stages:

```
PLANNING LAYER (human-co-creative, markdown artifacts)
  [1] vision-workshop ──────> planning/vision.md
  [2] feature-discovery ────> planning/features.md
  [3] journey-mapping ──────> planning/journeys/<slug>.md
                              (each journey → one or more epics)

EXECUTION LAYER (agent-autonomous, beads database)
  [4] beads-plan ───────────> bead DAG (tasks + dependencies per epic)
  [5] beads-execute ────────> completed code/tests/docs
```

**Planning layer** produces markdown artifacts through collaborative
dialogue. Each stage has a defined methodology, output format, and
quality checklist. The human validates each artifact before proceeding.

**Execution layer** decomposes epics into fine-grained tasks (beads)
arranged in a dependency graph (DAG), then executes them autonomously.
The handoff from planning to execution is at the epic level — journey-
mapping defines epics, beads-plan decomposes them.

### Supporting skills

| Skill | Role in pipeline |
|-------|-----------------|
| `validate-pipeline` | Quality gate per bead during execution |
| `epic-validation` | Quality gate at 100% epic completion |
| `iterative-planning-context` | Codebase context for beads-plan decomposition |

### BDD skills (pipeline-adjacent)

Journey-mapping produces Gherkin scenarios. These BDD skills implement
and maintain them:

| Skill | Purpose |
|-------|---------|
| `bdd-scenario-design` | Transform acceptance criteria into Gherkin |
| `bdd-step-implementation` | Write step definitions wired to fixtures |
| `bdd-red-green-refactor` | TDD inner loop: red, green, refactor |
| `bdd-scenario-evolution` | Maintain scenarios as code evolves |

### Pipeline status

The pipeline was imported from [beadsmith](https://github.com/user/beadsmith)
with light adaptations (org-mode references changed to markdown). Medium
adaptations are pending — see TODO sections in each skill's SKILL.md for
details. Key pending work:

- journey-mapping needs to absorb journey-to-tasks functionality (epic
  identification, readiness gating, INVEST methodology)
- beads-plan needs to accept epic definitions from journey markdown files
  instead of abstract specs
- beads-execute needs org-mode callback references removed
- Markdown artifact templates (vision.md, features.md, journeys/*.md)
  need to be finalized

## Skills

### Pipeline core

| Skill | Description | Source | Status |
|-------|-------------|--------|--------|
| `vision-workshop` | Product vision using Product Vision Board methodology | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/vision-workshop) | Adapted |
| `feature-discovery` | Feature discovery using OST + Story Mapping + Job Stories | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/feature-discovery) | Adapted |
| `journey-mapping` | User journeys with Gherkin scenarios and epic definitions | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/journey-mapping) | Adapted |
| `beads-plan` | Decompose epics into executable bead DAGs | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/beads-plan) | Adapted |
| `beads-execute` | Execute beads with claim/work/close workflow | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/beads-execute) | Adapted |

### Pipeline supporting

| Skill | Description | Source | Status |
|-------|-------------|--------|--------|
| `validate-pipeline` | Quality gate per bead (tests, lint, format) | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/validate-pipeline) | Vendored |
| `epic-validation` | Epic-level quality gate at 100% completion | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/epic-validation) | Vendored |
| `iterative-planning-context` | Codebase context snapshots for planning | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/iterative-planning-context) | Vendored |

### BDD

| Skill | Description | Source | Status |
|-------|-------------|--------|--------|
| `bdd-scenario-design` | Gherkin scenario design from acceptance criteria | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/bdd-scenario-design) | Vendored |
| `bdd-step-implementation` | Step definitions wired to test fixtures | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/bdd-step-implementation) | Vendored |
| `bdd-red-green-refactor` | TDD inner loop discipline | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/bdd-red-green-refactor) | Vendored |
| `bdd-scenario-evolution` | Scenario maintenance as code evolves | [beadsmith](https://github.com/user/beadsmith/tree/main/skills/bdd-scenario-evolution) | Vendored |

### Cross-cutting

| Skill | Description | Source | Status |
|-------|-------------|--------|--------|
| `writing-clearly-and-concisely` | Strunk's Elements of Style for prose | [obra/the-elements-of-style](https://github.com/obra/the-elements-of-style) | Adapted |

### Development tooling

| Skill | Description | Source | Status |
|-------|-------------|--------|--------|
| `commit` | Conventional commits with SourceHut issue trailers | [getsentry/skills](https://github.com/getsentry/skills/tree/main/plugins/sentry-skills/skills/commit) | Adapted |
| `find-bugs` | Branch diff review for bugs and security issues | [getsentry/skills](https://github.com/getsentry/skills/tree/main/plugins/sentry-skills/skills/find-bugs) | Adapted |
| `doc-coauthoring` | Structured doc co-authoring workflow | [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/doc-coauthoring) | Vendored |
| `mcp-builder` | MCP server development guide | [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/mcp-builder) | Vendored |
| `webapp-testing` | Playwright-based web app testing | [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/webapp-testing) | Vendored |
| `creating-skills` | Guide for creating skills using BDD | [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/skill-creator) + [obra/superpowers-skills](https://github.com/obra/superpowers-skills/tree/main/skills/meta/writing-skills) | Blended |

**Status key:**
- **Vendored**: Copied from upstream with attribution. Safe to update.
- **Adapted**: Modified from upstream. Check diff before updating.
- **Blended**: Synthesized from multiple upstreams. Maintained independently.

## Customizations

The following skills were modified from their upstream sources:

- **vision-workshop, feature-discovery, journey-mapping**: Org-mode output
  references changed to markdown (planning/*.md). Medium adaptations
  pending — see TODO sections in each SKILL.md.
- **beads-plan**: Org-mode layer references noted for future rewrite.
  Medium adaptation pending — needs to accept epic input from journey
  markdown instead of abstract specs.
- **beads-execute**: Org-mode callback and agent-specific references noted.
- **writing-clearly-and-concisely**: Reference file moved to `references/`
  subdirectory. Frontmatter description rewritten for ASO.
- **commit**: Issue references use SourceHut `todo.sr.ht` trailers
  instead of GitHub/Sentry refs.
- **find-bugs**: Default branch detection uses `git` instead of `gh`
  (GitHub CLI) since the project is hosted on SourceHut.
- **creating-skills**: Blended from Anthropic's `skill-creator` and obra's
  `writing-skills`. See the skill's attribution section for details.

## Not Imported

The following beadsmith skills were deliberately not imported:

| Skill | Reason |
|-------|--------|
| `journey-to-tasks` | Methodology absorbed into journey-mapping (readiness gating, epic identification) and beads-plan (INVEST, walking skeleton, Gherkin mapping) |
| `spawn-to-beads` | Org-mode plumbing. Delegation criteria absorbed into journey-mapping. |
| `spawn-readiness` | Org-mode plumbing. Readiness checking absorbed into journey-mapping gate. |
| `complete-to-org` | Org-mode plumbing. Not needed without org layer. |
| `org-planning` | Entirely org-mode specific. |

## Architecture

### Philosophy

Skills serve two purposes:

1. **Knowledge packages** -- give agents domain-specific information they
   lack (API docs, schemas, tool references).
2. **Behavior enforcement** -- define specific workflows and discipline
   that agents would otherwise shortcut or rationalize away.

Both are valid. The skill type determines which applies and how to design
and test the skill.

### Skill Types

| Type | Purpose | Examples |
|------|---------|----------|
| **Technique** | Concrete method with steps to follow | condition-based-waiting, root-cause-tracing |
| **Pattern** | Mental model for approaching problems | flatten-with-flags, test-invariants |
| **Reference** | API docs, syntax guides, tool documentation | mcp-builder, webapp-testing |
| **Discipline** | Behavior enforcement with compliance requirements | commit, BDD workflows |

Skill type determines testing approach and degree of freedom. See
`creating-skills` for full guidance.

### Directory Structure

Each skill is a directory with `SKILL.md` as the entrypoint:

```
skill-name/
├── SKILL.md           # Required, < 500 lines
├── scripts/           # Executable code (Python, Bash, etc.)
├── references/        # Documentation loaded on demand
└── assets/            # Files used in output (templates, icons, etc.)
```

Keep `SKILL.md` focused on the core workflow. Move heavy reference
material (> 100 lines) to `references/` with clear descriptions of when
to load each file. This keeps context usage low -- only `SKILL.md` loads
on trigger; references load when the agent determines it needs them.

### Frontmatter Standards

Required fields (for cross-tool compatibility):

```yaml
---
name: skill-name          # Lowercase hyphenated, matches directory name
description: >-           # Rich description with triggers and symptoms
  What this skill does. Use when [trigger conditions].
  Relevant for [symptoms, keywords, situations].
---
```

Optional fields (Claude Code-specific, ignored by OpenCode):

```yaml
allowed-tools: Read, Grep, Glob    # Tools permitted without approval
model: sonnet                       # Model override
context: fork                       # Run in sub-agent
```

The `description` field is the primary discovery mechanism. Agents read it
to decide whether to load the skill. Write it for Agent Search Optimization
(ASO) -- include trigger conditions, symptoms, and keywords that an agent
would search for. See `creating-skills/references/discovery.md` for guidance.

### Writing Style

Use imperative mood consistently in SKILL.md body text. Write
instructions as commands: "Run the tests" not "You should run the tests"
or "The tests are run." This matches how agents process instructions
most effectively.

### Naming Conventions

- **Verb-first, active voice**: `creating-skills` not `skill-creation`
- **Lowercase hyphenated**: `root-cause-tracing` not `rootCauseTracing`
- **Descriptive over generic**: `condition-based-waiting` not `async-helpers`
- **Directory name = frontmatter name**: must match exactly
- **1-64 characters**, no leading/trailing hyphens, no consecutive hyphens

### Provider Agnosticism

All naming, conventions, and documentation should avoid lock-in to any
specific LLM provider or agentic harness:

- Use "agent" not "Claude" in skill instructions
- Use "ASO" (Agent Search Optimization) not "CSO" (Claude Search Optimization)
- Use "sub-agent" not "Claude Code Task tool" when describing dispatch
- Use standard frontmatter fields that work across tools
- Test skills in multiple environments when practical

## Compatibility

Both Claude Code and OpenCode discover skills from
`.claude/skills/<name>/SKILL.md`. Each tool reads the frontmatter fields
it recognizes and ignores the rest, so a single SKILL.md works for both
without loss of functionality.

## Vendor Tracking

Skills copied from upstream repos include attribution links in their
`SKILL.md` files. The **Status** column in the skills table above
indicates how each skill relates to its upstream:

- **Vendored** skills can be updated by re-fetching from upstream.
- **Adapted** skills should be diffed against upstream before updating.
- **Blended** skills are maintained independently; upstream changes are
  evaluated but not automatically incorporated.
