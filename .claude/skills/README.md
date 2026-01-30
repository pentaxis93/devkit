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
| `coherence-audit` | Systematic audit when discrepancies reveal systemic issues |
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

Each planning-layer stage (vision, features, journeys) produces a
markdown artifact with a defined template (see `references/` in each
skill's directory). Each artifact's frontmatter references its upstream
source and names its downstream consumer. Each skill names its
predecessor and successor (vision-workshop is the pipeline root). The
journey file's Epics section serves as the handoff contract between the
planning and execution layers.

### Pipeline coherence checklist

**Run this checklist after every pipeline skill edit.** When adding or
editing a pipeline skill, verify:

- [ ] Skill names its upstream skill by name (or "pipeline root")
- [ ] Skill names its downstream skill by name
- [ ] Skill has a Related Skills table with at least predecessor and successor
- [ ] Output template (if any) references its upstream artifact in frontmatter
- [ ] Output template (if any) names its downstream consumer
- [ ] Quality checklist mentions the next pipeline stage
- [ ] All cross-references to other files resolve (no broken links)
- [ ] README skill tables are updated

## Skills

### Pipeline core

| Skill | Description |
|-------|-------------|
| `vision-workshop` | Product vision using Product Vision Board methodology |
| `feature-discovery` | Feature discovery using OST + Story Mapping + Job Stories |
| `journey-mapping` | User journeys with Gherkin scenarios and epic definitions |
| `beads-plan` | Decompose epics into executable bead DAGs |
| `beads-execute` | Execute beads with claim/work/close workflow |

### Pipeline supporting

| Skill | Description |
|-------|-------------|
| `validate-pipeline` | Quality gate per bead (tests, lint, format) |
| `epic-validation` | Epic-level quality gate at 100% completion |
| `coherence-audit` | Systematic audit when discrepancies reveal systemic issues |
| `iterative-planning-context` | Codebase context snapshots for planning |

### BDD

| Skill | Description |
|-------|-------------|
| `bdd-scenario-design` | Gherkin scenario design from acceptance criteria |
| `bdd-step-implementation` | Step definitions wired to test fixtures |
| `bdd-red-green-refactor` | TDD inner loop discipline |
| `bdd-scenario-evolution` | Scenario maintenance as code evolves |

### Cross-cutting

| Skill | Description | Source | Status |
|-------|-------------|--------|--------|
| `writing-clearly-and-concisely` | Strunk's Elements of Style for prose | [obra/the-elements-of-style](https://github.com/obra/the-elements-of-style) | Adapted |

### Development tooling

| Skill | Description | Source | Status |
|-------|-------------|--------|--------|
| `commit` | Conventional commits with SourceHut issue trailers | [getsentry/skills](https://github.com/getsentry/skills/tree/main/plugins/sentry-skills/skills/commit) | Adapted |
| `find-bugs` | Branch diff review for bugs and security issues | [getsentry/skills](https://github.com/getsentry/skills/tree/main/plugins/sentry-skills/skills/find-bugs) | Adapted |
| `doc-coauthoring` | Structured doc co-authoring workflow | [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/doc-coauthoring) | Upstream |
| `mcp-builder` | MCP server development guide | [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/mcp-builder) | Upstream |
| `webapp-testing` | Playwright-based web app testing | [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/webapp-testing) | Upstream |
| `skill-creator` | Guide for creating skills using BDD | [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/skill-creator) + [obra/superpowers-skills](https://github.com/obra/superpowers-skills/tree/main/skills/meta/writing-skills) | Blended |

**Status key:**
- **Upstream**: Copied from source with attribution. Safe to update from source.
- **Adapted**: Modified from source. Check diff before updating.
- **Blended**: Synthesized from multiple sources. Maintained independently.

## Customizations

The following external skills were modified from their upstream sources:

- **writing-clearly-and-concisely**: Reference file moved to `references/`
  subdirectory. Frontmatter description rewritten for ASO.
- **commit**: Issue references use SourceHut `todo.sr.ht` trailers
  instead of GitHub/Sentry refs.
- **find-bugs**: Default branch detection uses `git` instead of `gh`
  (GitHub CLI) since the project is hosted on SourceHut.
- **skill-creator**: Blended from Anthropic's `skill-creator` and obra's
  `writing-skills`. See the skill's attribution section for details.

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
`skill-creator` for full guidance.

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
would search for. See `skill-creator/references/discovery.md` for guidance.

### Writing Style

Use imperative mood consistently in SKILL.md body text. Write
instructions as commands: "Run the tests" not "You should run the tests"
or "The tests are run." This matches how agents process instructions
most effectively.

### Naming Conventions

Skill names are primarily for human readability. Agent discovery depends
on the `description` field (see Frontmatter Standards below), not the
name. Choose names that are scannable in a directory listing and
unambiguous when a user types `/skill-name`.

**Name by skill type:**

| Skill Type | Pattern | Examples |
|------------|---------|----------|
| Imperative action | verb-noun | `find-bugs`, `create-pr`, `validate-pipeline` |
| Methodology or capability | noun-compound | `vision-workshop`, `code-review`, `feature-discovery` |
| Tool-scoped | tool-first | `beads-plan`, `mcp-builder`, `beads-execute` |
| Related family | shared prefix | `bdd-scenario-design`, `bdd-step-implementation` |

See `docs/decisions/0004-skill-naming-convention.md` for the ecosystem
research behind this convention.

**General rules:**

- **Lowercase hyphenated**: `root-cause-tracing` not `rootCauseTracing`
- **Descriptive over generic**: `feature-discovery` not `utils`
- **Directory name = frontmatter name**: must match exactly
- **8-20 characters typical**, max 64, no leading/trailing or consecutive hyphens

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

## Attribution and Upstream Tracking

Some development tooling and cross-cutting skills are imported from
open-source repos. These include attribution links in their `SKILL.md`
files. The **Status** column in the development tooling and cross-cutting
tables indicates how each relates to its source:

- **Upstream** skills can be updated by re-fetching from the source repo.
- **Adapted** skills should be diffed against source before updating.
- **Blended** skills are maintained independently; source changes are
  evaluated but not automatically incorporated.

Pipeline, supporting, and BDD skills are original project code and have
no external upstream.
