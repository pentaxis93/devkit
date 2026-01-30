# Use Noun-Compound Naming Convention for Skills

## Context and Problem Statement

Skills need consistent naming conventions. Our initial rule was
"verb-first, active voice" (e.g., `creating-skills` not
`skill-creation`). However, applying this rule to imported skills
revealed widespread violations and raised the question: does the
ecosystem actually use verb-first naming?

## Considered Options

* Gerund-first naming (Anthropic's documented recommendation):
  `processing-pdfs`, `creating-skills`, `mapping-journeys`
* Verb-first imperative: `create-skills`, `map-journeys`, `find-bugs`
* Noun-compound naming (ecosystem practice): `skill-creator`,
  `journey-mapping`, `mcp-builder`
* Name by skill type (hybrid): verb-first for actions, noun-compound
  for capabilities

## Decision Outcome

Chosen option: "Name by skill type (hybrid)," because ecosystem
research showed no single form dominates, but clear patterns emerge
by skill type.

| Skill Type | Pattern | Examples |
|------------|---------|----------|
| Imperative action | verb-noun | `find-bugs`, `create-pr`, `validate-pipeline` |
| Methodology or capability | noun-compound | `vision-workshop`, `code-review`, `feature-discovery` |
| Tool-scoped | tool-first | `beads-plan`, `mcp-builder`, `beads-execute` |
| Related family | shared prefix | `bdd-scenario-design`, `bdd-step-implementation` |

### Research findings

**Ecosystem survey** (Anthropic's shipped skills, Sentry's skills,
obra's superpowers-skills, agentskills.io standard):

- Anthropic recommends gerund-first in docs but ships zero gerund-first
  skills. Their actual names: `mcp-builder`, `skill-creator`, `pdf`,
  `doc-coauthoring`, `webapp-testing` — all noun compounds.
- Sentry uses verb-first only for imperative actions (`create-pr`,
  `find-bugs`) and noun compounds for everything else (`code-review`,
  `brand-guidelines`).
- No major skill repository uses gerund-first naming.

**ASO (Agent Search Optimization) implications**: The `description`
field does 90%+ of the discovery work. At startup, agents load name +
description; the description is 6-16x more tokens than the name. The
name serves human readability and explicit `/slash-command` invocation,
not agent matching.

### Consequences

* Good, because names match established ecosystem conventions
* Good, because the hybrid approach lets the name signal what kind of
  skill it is (action vs. capability vs. tool-scoped)
* Good, because existing names mostly already conform — minimal renames
  needed (only `creating-skills` → `skill-creator`)
* Neutral, because the name matters less than we initially assumed; the
  description field is the primary ASO mechanism
* Bad, because the convention requires judgment (which type is this
  skill?) rather than a single mechanical rule
