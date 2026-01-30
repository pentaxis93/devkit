# Journey Template

Use this template when creating `planning/journeys/<slug>.md`.

Each journey file maps one user journey from a feature. A feature may
produce multiple journeys. Each journey produces one or more epics that
beads-plan decomposes into executable tasks.

```markdown
---
status: draft | scenarios-complete | ready | in-progress | complete
feature: planning/features.md#[feature-name]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# Journey: [Journey Name]

## Persona

[Who is taking this journey? Reference the target user from the vision,
with any journey-specific context.]

## Goal

[What does this person want to accomplish? Derived from the feature's
job story.]

## Phases

### Phase 1: [Phase Name]

**User mental model:** [What the user is thinking at this stage]

#### Touchpoint: [Action Name]

**User action:** [What the user does — intent-based, not UI mechanics]
**System response:** [What happens — behavior, not implementation]

**Pain points:**
- [Current pain] — Severity: high | medium | low

**Opportunities:**
- [How we can improve this]

[Repeat touchpoints as needed per phase]

---

[Repeat phases — 3-6 phases covering the complete flow]

---

## Scenarios

```gherkin
Feature: [Journey phase or capability]

  Scenario: [Happy path — what behavior this tests]
    Given [precondition — the starting state]
    When [user action — what triggers the behavior]
    Then [outcome — what should happen]

  Scenario: [Error case — what can go wrong]
    Given [precondition]
    When [action that triggers the error]
    Then [how the system handles it gracefully]

  Scenario: [Edge case — unusual but valid]
    Given [unusual precondition]
    When [action]
    Then [expected outcome]
```

[Include scenarios for: happy path through all phases, key error cases,
edge cases. Each scenario tests one behavior.]

---

## Walking Skeleton

[The thinnest possible end-to-end slice through this journey. This is
the first thing to implement — minimal but touching every layer.]

**Slice:** [One sentence describing the minimal path]
**Covers phases:** [Which phases this slice touches]
**Defers:** [What is deliberately left out of this first slice]

---

## Epics

[Each journey maps to one or more coarse epics. An epic is a unit of
work that beads-plan will decompose into a bead DAG. Epics must pass
the INVEST gate below before they are ready for beads-plan.]

### Epic: [Title]

**Scope:** [1-2 sentences — what this epic delivers as a coherent unit]

**Walking skeleton:** [Which slice from above, or "N/A — not the first
epic"]

**Acceptance criteria:**
[Derived from the Gherkin scenarios above. Each criterion traces to a
specific scenario.]

1. [Criterion] — from Scenario: [name]
2. [Criterion] — from Scenario: [name]
3. [Criterion] — from Scenario: [name]

**Estimated complexity:** S | M | L | XL

**Dependencies:** [Other epic titles from this or other journeys, or
"None"]

---

[Repeat ### Epic: sections for each epic in this journey]
```

## INVEST Readiness Gate

Each epic must pass before it is ready for beads-plan:

- [ ] **Independent** — Can be planned and delivered without waiting on
  other epics (or dependencies are explicitly listed)
- [ ] **Negotiable** — Scope can be adjusted without losing the core
  value
- [ ] **Valuable** — Delivers observable value to the user or system
- [ ] **Estimable** — Complexity is understood well enough to estimate
  (S/M/L/XL)
- [ ] **Small** — Can be decomposed into ≤15 beads (if larger, split
  the epic)
- [ ] **Testable** — Acceptance criteria are concrete and verifiable

## Validation Criteria

Before marking journey as ready:

- [ ] Journey has persona and goal defined
- [ ] Journey has 3-6 phases covering the complete flow
- [ ] Each phase has user action and system response touchpoints
- [ ] Touchpoints describe needs, not mechanisms (no technology details)
- [ ] Gherkin scenarios are declarative (no UI scripting)
- [ ] Each scenario has exactly one When-Then pair
- [ ] Happy path scenarios cover all phases
- [ ] Key error cases have scenarios
- [ ] Walking skeleton (first slice) is identified
- [ ] At least one epic is defined
- [ ] Each epic passes the INVEST readiness gate
- [ ] Each epic's acceptance criteria trace to Gherkin scenarios
- [ ] `feature` frontmatter references parent feature
- [ ] `status` set to `ready`
- [ ] No `[REPLACE]` or `[TODO]` placeholders remaining
