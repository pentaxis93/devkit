# Five-Stage Planning-to-Execution Pipeline

## Context and Problem Statement

The project needs a structured pipeline from product vision to working
code. We evaluated building one from first principles (using Spolsky,
Amazon, Google design doc practices) and researched existing frameworks
(BMAD, Spec Kit, Lean Inception, GIST, Shape Up). No off-the-shelf
framework covers the full vision-to-code pipeline — each covers only
parts. We designed a 5-stage pipeline with markdown artifacts as the
planning layer and beads as the execution layer.

## Considered Options

* Build a custom pipeline assembling elements from multiple frameworks
* Use a single existing framework (Shape Up, Lean Inception, etc.)
* Design a 5-stage pipeline with clear planning/execution boundary

## Decision Outcome

Chosen option: "5-stage pipeline with planning/execution boundary."

### Pipeline design

```
PLANNING LAYER (human-co-creative, markdown artifacts)
  [1] vision-workshop ──────> planning/vision.md
  [2] feature-discovery ────> planning/features.md
  [3] journey-mapping ──────> planning/journeys/<slug>.md

EXECUTION LAYER (agent-autonomous, beads database)
  [4] beads-plan ───────────> bead DAG per epic
  [5] beads-execute ────────> completed code/tests/docs
```

The planning layer produces markdown artifacts through collaborative
human-agent dialogue. The execution layer decomposes epics into
fine-grained tasks (beads) and executes them autonomously. The handoff
point is at the epic level: journey-mapping defines epics, beads-plan
decomposes them.

### Key design principles

**Markdown artifacts.** Planning output is markdown in `planning/`.
Universally supported, renders on any forge, native format for agent
skill files. Beads provides task management; agents execute code
directly.

**Each artifact references its predecessor.** The traceability chain is:
vision ← features ← journeys ← epics ← beads. Every node tells you
what preceded it. For beads, the epic carries a `source_journey`
reference; child beads trace back through the parent relationship.

**INVEST readiness gate at the planning/execution boundary.**
Journey-mapping validates each epic against INVEST criteria (Independent,
Negotiable, Valuable, Estimable, Small, Testable) before it is ready
for beads-plan. This gates the handoff between human-co-creative
planning and agent-autonomous execution.

### Supporting and adjacent skills

In addition to the 5 core pipeline skills:
- `validate-pipeline`, `epic-validation`, `coherence-audit`,
  `iterative-planning-context` (pipeline supporting)
- 4 BDD skills (pipeline-adjacent — journey-mapping produces Gherkin
  scenarios that these skills implement)
- `writing-clearly-and-concisely` (cross-cutting prose quality)

### Consequences

* Good, because the pipeline covers the full vision-to-code path
* Good, because markdown artifacts are universally accessible
* Good, because the planning/execution boundary is explicit and gated
* Good, because each stage has a defined output template serving as the
  contract for the next stage
* Neutral, because no off-the-shelf framework covers the full pipeline,
  so we can't delegate maintenance to an upstream project
* Bad, because the pipeline is custom and requires its own documentation
  and onboarding
