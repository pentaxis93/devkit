# Phase B: Org-Mode Removal and Pipeline Coherence

> **Status:** Complete
> **Last updated:** 2026-01-30
> **Scope:** Remove org-mode references from all imported skills, connect
> pipeline stages so each skill's output feeds the next skill's input.

## Problem Statement

The beadsmith pipeline was built around org-mode as its planning layer.
We replaced org-mode with markdown artifacts but only made light textual
substitutions on import. The result: skills still describe workflows
involving org-mode tasks, `spawn-to-beads`, `complete-to-org`,
`source_org_id`, `DELEGATED_TO`, and `emacsclient` — none of which
exist in this project.

An agent following these skills today would encounter broken references,
attempt to invoke non-existent skills, and check for properties that
don't exist. This isn't cosmetic — it's functionally broken.

## Goals

1. **Remove all org-mode ontology** — no agent should ever encounter
   org-mode concepts, non-imported skill names, or emacs references
2. **Connect pipeline stages** — each skill's output format must match
   the next skill's expected input
3. **Absorb journey-to-tasks methodology** — INVEST criteria, walking
   skeleton identification, and Gherkin-to-acceptance mapping were in a
   skill we didn't import but said we'd absorb into journey-mapping and
   beads-plan
4. **Keep skills under 500 lines** — beads-plan is 1327 lines, much of
   which is org-mode content or reference material that belongs in
   `references/`

## Affected Skills

### beads-plan (1327 lines, 41 org-mode matches) — MAJOR

The worst offender. Contains entire sections that must be deleted or
rewritten:

| Section | Lines (approx) | Action |
|---------|----------------|--------|
| Integration diagram (org-mode flow) | 15-46 | **Rewrite** — replace with our pipeline flow |
| Spawned Epics (from Org-mode) | 90-139 | **Delete entirely** |
| Org-Mode Coherence | 142-204 | **Delete entirely** — replace with markdown traceability principle |
| Phase 4 coherence check | 640-666 | **Rewrite** — replace org check with journey-markdown traceability |
| Anti-patterns: Ignoring Org Callback | 1231-1236 | **Delete** |
| Anti-patterns: Closing Epic Before Callback | 1238-1241 | **Delete** |
| TODO section | 1289-1312 | **Delete** (the TODOs will be done) |
| Attribution line | 1316-1317 | **Update** to reflect actual adaptation level |
| Related Skills table | 1319-1327 | **Remove** non-imported skills |
| Examples referencing org-planning | 391, 434 | **Rewrite** examples |

Additionally:
- Phase 1 (Understanding) needs input format: reads epic definitions
  from `planning/journeys/<slug>.md`
- Phase 2 (Decomposition) should absorb INVEST methodology from
  journey-to-tasks
- BD Command Reference (lines 1076-1183) and Worked Example (929-1073)
  are candidates for `references/` to get under 500 lines

**Estimated net effect:** Delete ~200 lines of org-mode content, move
~250 lines to references, add ~50 lines of pipeline-connected content.
Target: ~900 lines (still over 500 — may need further extraction).

### beads-execute (836 lines, 2 org-mode matches) — MINOR

- TODO section (lines 648-655): mentions `complete-to-org` removal —
  delete TODO, the removal happens as part of this phase
- Attribution line (660): update
- OpenCode-specific references (lines 113-123, 406): already annotated
  with "(agent-specific — adapt to your tooling)" — leave as-is

### epic-validation (314 lines, 1 match) — MINOR

- References section (line 312): link to `complete-to-org` skill —
  delete that line

### iterative-planning-context (502 lines, 3 matches) — MINOR

- Example (line 438): "planning/ exists - org-mode planning system in
  use" → change to "planning/ exists - markdown planning artifacts"
- Example (line 451): `spawn-to-beads/` in directory tree → remove
- Related Skills table (line 493): `spawn-to-beads` row → delete

### journey-mapping (480 lines, 1 match) — MINOR

- TODO section (line 474): mentions absorbing spawn-to-beads delegation
  criteria — delete after absorption is complete

### feature-discovery (465 lines, 1 match) — TRIVIAL

- Attribution line (465): "org-mode references changed to markdown" →
  update to reflect actual adaptation

### vision-workshop (449 lines, 1 match) — TRIVIAL

- Attribution line (449): same as feature-discovery

## Pipeline Handoff Analysis (Research Complete)

### What each stage currently produces

**feature-discovery → `planning/features.md`**
Each feature entry contains: job story (When/I want/So I can), evidence,
measurable success criteria, in-scope/out-of-scope, assumptions with
tests. Leaves a `Journeys:` field empty for journey-mapping to populate.
Template not yet formally defined (has TODO).

**journey-mapping → `planning/journeys/<slug>.md`**
YAML frontmatter (status, feature ref, dates). Content: persona + goal,
3-6 phases of user mental model, touchpoints per phase (user action +
system response), pain points and opportunities, Gherkin scenarios
(Feature/Scenario/Given-When-Then), walking skeleton identification.
Template not yet formally defined (has TODO).

**Crucially:** journey-mapping's TODO section (line 469) describes a
planned but unimplemented **Epics section** — each journey maps to one
or more coarse epics with title, description, walking skeleton, and
Gherkin-derived acceptance criteria. This is the missing bridge.

**beads-plan ← expects "project specifications"**
Phase 1 (Understanding) is generic: scope, success criteria, constraints,
risks, stakeholders, 3-7 major components. No reference to journey files
or any upstream pipeline stage.

### What the org-mode glue layer was doing

In beadsmith, three skills bridged journey-mapping to beads-plan:

```
journey-mapping → journeys/<slug>.md
                       │
                 journey-to-tasks (INVEST validation, Definition of Ready)
                       │
                 org-mode tasks (canonical state store)
                       │
                 spawn-to-beads (bd create epic with source_org_id)
                       │
                 beads-plan (decomposes epic into bead DAG)
                       │
                 beads-harness (executes beads)
                       │
                 complete-to-org (updates org task to DONE)
```

The org-mode layer provided:
1. **Traceability**: `source_org_id` in beads ↔ `DELEGATED_TO` in org
2. **State management**: org tasks tracked TODO → WAITING → DONE
3. **Readiness gating**: journey-to-tasks applied INVEST before spawn
4. **Completion callbacks**: complete-to-org closed the loop

### Target state: direct connection (no org layer)

```
journey-mapping → planning/journeys/<slug>.md
                  (includes epic definitions section)
                       │
                  [INVEST readiness gate — in journey-mapping]
                       │
                 beads-plan Phase 1 reads journey file
                  (extracts epic definitions, Gherkin → acceptance)
                       │
                 beads-plan Phases 2-5 (decompose + instantiate)
                       │
                 beads-execute (claim/work/close)
                       │
                 [Status update — journey file status field]
```

### What needs to be built for the handoff

| Component | Replaces | Where |
|-----------|----------|-------|
| Epic definitions section in journey output | journey-to-tasks + spawn-to-beads | journey-mapping: new output section |
| `source_journey: <slug>#<epic>` in bead descriptions | `source_org_id` marker | beads-plan: Phase 4 convention |
| Phase 1 reads journey files | Generic "understand the spec" | beads-plan: Phase 1 rewrite |
| INVEST validation before emitting epics | journey-to-tasks readiness gate | journey-mapping: quality checklist |
| Gherkin-to-acceptance mapping guidance | implicit in journey-to-tasks | beads-plan: Phase 2 addition |
| Walking skeleton as decomposition pattern | journey-to-tasks methodology | beads-plan: Phase 2 addition |
| Status update on epic completion | complete-to-org callback | Convention: update journey frontmatter |

### Epic definition format (proposed)

Each journey file would end with an Epics section:

```markdown
## Epics

### Epic: <title>

**Scope:** <1-2 sentence description of what this epic delivers>

**Walking skeleton:** <the thinnest end-to-end slice>

**Acceptance criteria:**
(derived from Gherkin scenarios above)
1. <criterion from Scenario: X>
2. <criterion from Scenario: Y>
3. <criterion from Scenario: Z>

**Estimated complexity:** S / M / L / XL

**Dependencies:** <other epic titles, if any>
```

This gives beads-plan everything it needs to create a `bd create` epic
and decompose it, without any intermediate org-mode layer.

## INVEST Methodology Absorption

From the not-imported `journey-to-tasks` skill, we committed to
absorbing:

1. **INVEST criteria** for task decomposition (Independent, Negotiable,
   Valuable, Estimable, Small, Testable)
2. **Walking skeleton identification** — find the thinnest end-to-end
   slice first
3. **Gherkin-to-acceptance mapping** — how Gherkin scenarios from
   journey-mapping become bead acceptance criteria

**Where each goes:**

| Methodology | Destination | How |
|-------------|-------------|-----|
| INVEST criteria | beads-plan Phase 2 (Decomposition) | Add alongside existing granularity rules as a validation checklist |
| Walking skeleton | beads-plan Phase 2 | Add as a decomposition pattern (vertical slice variant) |
| Gherkin-to-acceptance | beads-plan Phase 2 | Add guidance: "Extract acceptance criteria from the journey file's Gherkin scenarios" |
| Readiness gating | journey-mapping | Add INVEST validation to the epic definitions quality checklist: each epic must pass INVEST before emission |
| Definition of Ready | journey-mapping | Epic definitions section includes a readiness gate: is scope bounded? Are Gherkin scenarios concrete? Is walking skeleton identified? |

## Decisions

### Decision 1: Reference Material Extraction

**Context:** Our README says SKILL.md should be under 500 lines. The
agent loads SKILL.md fully when the skill triggers; files in
`references/` load only when the agent decides it needs them (via
markdown links in SKILL.md). This is how skills stay modular — core
methodology inline, reference material on demand.

beads-plan is 1327 lines today. After org-mode deletion it'll be ~1100.
Three self-contained reference sections account for ~320 lines: the BD
Command Reference (108 lines), the Worked Example (145 lines), and the
Swarm Execution Guidance (65 lines). None are part of the planning
methodology — they're lookup material for agents who are running `bd`
commands or executing beads.

beads-execute is 836 lines. Its Documentation Protocol (170 lines) and
Error Recovery Patterns (65 lines) are similarly reference material, not
core workflow.

**Option A:** Extract reference sections from both skills to
`references/` subdirectories. beads-plan drops to ~785, beads-execute
to ~600. SKILL.md keeps one-line pointers to each reference file.

**Option B:** Extract from beads-plan only (the worse offender). Leave
beads-execute as-is.

**Option C:** Extract from both, and audit for any additional sections
that are reference material rather than core methodology. Get both as
close to 500 as the content allows.

**Decision:** Option C. Extract from both skills, audit for additional
reference material. Modularity and composability are key benefits of
the skill system — core methodology inline, everything else on demand.

---

### Decision 2: Traceability Strictness

**Context:** In the original beadsmith pipeline, every beads epic
carried a `source_org_id` field linking it back to the org-mode task
that spawned it. This was epic-level only — individual beads (tasks
within the epic) traced back to the epic via the parent-child
relationship, and the epic traced back to org-mode.

We're replacing this with a `source_journey` field that links a beads
epic to its origin in `planning/journeys/<slug>.md`. The question is
how much of the bead hierarchy carries this reference.

The pipeline's traceability chain looks like this:

```
vision.md ← features.md ← journeys/<slug>.md ← beads epic ← bead tasks
```

Each artifact already references its upstream source (journey frontmatter
has a `feature:` field, features reference vision). So the chain is
unbroken as long as each link references the one before it.

**Option A: Epic-only.** The top-level beads epic carries
`source_journey: journeys/<slug>.md#epic-name`. Child beads trace back
through parent-child. Mirrors how `source_org_id` worked. The full chain
exists: bead → parent epic → journey → feature → vision.

**Option B: Every bead.** Every bead description includes the journey
reference. Redundant with the parent chain but provides direct lookup.
Adds boilerplate to every `bd create` command.

**Option C: Advisory.** Documented as best practice but not enforced.
Agents may or may not include the reference.

**Decision:** Option A (epic-only), as an instance of a broader pipeline
principle: every artifact references its predecessor. The chain is
vision (given — no predecessor) ← features ← journeys ← epics ← beads.
Each node tells you what preceded it. For beads, only the epic carries
`source_journey`; child beads trace back through the parent relationship.
The original `source_org_id` pattern was epic-only because that's the
natural granularity — the org-mode layer just obscured this.

---

### Decision 3: Completion Callback

**Context:** In beadsmith, when a beads epic reached 100% completion,
the `complete-to-org` skill would update the source org-mode task from
WAITING to DONE. This kept the planning layer (org-mode) in sync with
execution state (beads).

Our planning layer is now markdown files in `planning/`. Journey files
have a `status` field in their YAML frontmatter. The beads database
tracks execution state independently (`bd epic status` shows progress).

The question: when a beads epic completes, should anything update the
journey file to reflect that the work is done? This determines whether
`planning/` reflects current execution state or remains a static
planning snapshot.

**Option A: Manual.** Journey files are planning artifacts written
during the planning phase. Execution state lives in beads. Humans
update journey status during reviews if they choose.

**Option B: Documented convention.** beads-execute or epic-validation
includes a step: "If the epic has a `source_journey` reference, update
the journey file's frontmatter `status` to `complete`." Not automated
tooling — just an instruction in the skill that agents follow.

**Option C: Skip.** Two sources of truth cause drift. The beads
database is the execution truth; journey files are the planning truth.
Don't try to sync them. Query `bd epic status` for execution state.

**Decision:** Option B (documented convention), with a stronger
principle: journey files must be updated or deleted, never left stale.
Stale status in saved files invites confusion. The beads database is
the execution truth, but if planning artifacts are kept, they must
reflect reality. The convention goes in beads-execute or
epic-validation: "If the epic has a `source_journey` reference, update
the journey file's frontmatter `status` to `complete`."

---

### Decision 4: Template Formalization

**Context:** The pipeline's stages connect through markdown artifacts:
`planning/vision.md`, `planning/features.md`,
`planning/journeys/<slug>.md`. Each skill describes its output format
narratively in its methodology sections, but neither feature-discovery
nor journey-mapping defines an exact markdown template (both have TODOs
acknowledging this).

For the handoff to work mechanically, beads-plan Phase 1 needs to know
the exact structure of a journey file's Epics section so it can extract
epic definitions reliably. The other sections (vision format, feature
format) are consumed by humans during collaborative dialogue, so exact
templates matter less for automation.

**Option A: Define all templates now.** Formalize the complete output
template for every pipeline stage. Ensures consistency and makes the
pipeline runnable end-to-end. Adds scope to Phase B.

**Option B: Define only the epic definitions template.** That's the
piece beads-plan needs to parse. The rest of the journey/feature
templates are internal to each skill's dialogue process and can be
formalized when we first run the pipeline for real.

**Option C: Defer all templates.** Phase B focuses on removing org-mode
and connecting the pipeline conceptually. Templates are defined in a
later phase.

**Decision:** Option A. Define all templates now. This is the core
value of the planning phase — every detail compounds as pipeline stages
ripple into each other. The templates are the contracts between stages;
getting them right here means the implementation flows naturally.

## Execution Log

All tasks completed:

1. **Decisions resolved** — 4 decisions documented above
2. **Pipeline artifact templates created** — vision, features, journey templates in `references/`
3. **Journey-mapping updated** — Epic definitions phase added, quality checklist, template reference
4. **beads-plan rewritten** — org-mode sections deleted, pipeline flow added, Phase 1/2 rewritten,
   reference material extracted to `references/` (swarm-execution, worked-example, bd-command-reference)
   Final: 904 lines (core methodology inline, reference material modular)
5. **beads-execute cleaned** — TODO section deleted, documentation protocol and error recovery
   extracted to `references/`, example IDs genericized. Final: 590 lines
6. **epic-validation fixed** — `complete-to-org` reference replaced with journey frontmatter status
   update convention, example IDs genericized
7. **iterative-planning-context fixed** — org-mode references replaced with markdown planning artifacts,
   `spawn-to-beads` replaced with `beads-plan`, duplicate Related Skills row removed
8. **validate-pipeline and bdd-scenario-design** — example IDs genericized
9. **bead-quality-criteria.md** — org-mode and beadsmith references replaced
10. **All attribution removed** — pipeline skills are user's own code, no external upstream
11. **README updated** — pipeline status section rewritten, no remaining stale references

**Verification:** `grep -rn "org-mode\|beadsmith" .claude/skills/ --include="*.md" | grep -v ".beadsmith/"` returns clean (only `.beadsmith/` runtime directory paths in beads-execute remain, which are the actual `bd` tool directory).
