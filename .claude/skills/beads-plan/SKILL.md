---
name: beads-plan
description: >-
  Transform epic definitions from journey files into executable beads
  dependency graphs. Decomposes epics into bounded tasks with dependency
  mapping and validation. Use when breaking down epics from journey-mapping,
  planning multi-step work, or when asked to create a beads implementation
  plan.
---

# Beads Planning Skill

## What This Skill Does

This skill guides the transformation of project specifications into instantiated beads dependency graphs. A well-planned beads implementation creates an executable DAG (directed acyclic graph) where each bead (issue) has clear boundaries, explicit inputs/outputs, and testable success criteria.

### Pipeline Position

This skill is the bridge between **planning** and **execution**:

```
journey-mapping ──> planning/journeys/<slug>.md
                    (includes epic definitions + acceptance criteria)
                         │
                    beads-plan (YOU ARE HERE)
                    ├── Read epic definitions from journey file
                    ├── Decompose each epic into bead DAG
                    └── Validate dependency graph
                         │
                    beads-execute
                    (claim/work/close beads autonomously)
```

**Input:** Epic definitions from `planning/journeys/<slug>.md`. Each
epic has a title, scope, acceptance criteria (derived from Gherkin
scenarios), walking skeleton reference, estimated complexity, and
dependencies.

**Output:** Instantiated bead DAG — a dependency graph of tasks with
clear boundaries, explicit inputs/outputs, and testable acceptance
criteria.

**Traceability:** Each epic's bead description includes a
`source_journey` reference linking back to its origin journey file.
Child beads trace back through the parent relationship.

### What Beads Planning Offers

1. **Explicit dependency graph** - Forces architectural clarity upfront
2. **Bounded units** - Each bead has clear inputs/outputs/success criteria
3. **Parallelization** - Independent beads can execute concurrently
4. **Integration contracts** - Boundaries are explicit and testable
5. **Fault isolation** - One bead's failure doesn't corrupt others
6. **Ready work detection** - `bd ready` shows unblocked work
7. **Progress visibility** - Dependency tree shows completion state

## Prerequisites

- `bd` CLI installed (`npm install -g @beads/bd` or `brew install steveyegge/beads/bd`)
- Repository initialized with `bd init` (run once per project)
- Working directory is within a git repository
- For iterative changes: Planning Context Snapshot recommended (see `iterative-planning-context` skill)

---

## Quick Start

For a simple project, the workflow is:

```bash
# 1. Create the epic (top-level container)
bd create "My Project Epic" -t epic -p 1 -d "Description of the overall goal"

# 2. Create tasks as children of the epic (auto-generates hierarchical IDs)
bd create "First task" -t task -p 2 --parent bd-XXXX
bd create "Second task" -t task -p 2 --parent bd-XXXX

# 3. Add blocking dependencies between tasks
bd dep add bd-XXXX.2 bd-XXXX.1  # Task 2 depends on Task 1

# 4. Validate the plan
bd ready          # Should show Task 1 as ready
bd dep tree bd-XXXX   # Visualize the dependency graph
bd dep cycles     # Should return empty (no cycles)
```

For complex projects, follow the five phases below. The phases are internal reasoning steps—work through them continuously and present the instantiated result. Only pause for user input when the specification has genuine ambiguity that blocks progress.

---

## Context-Aware Planning (Iterative Changes)

When planning changes to an **existing codebase** (vs greenfield), leverage Planning Context Snapshots for better decomposition.

### Check for Existing Context

Before starting Phase 1, check if a Planning Context Snapshot exists:

```bash
ls docs/planning/context-*.md 2>/dev/null
```

If found, read it first - it contains:
- **Structure Map**: Where new code should live
- **Conventions**: Patterns to follow (naming, error handling, tests)
- **Integration Points**: What existing code to touch
- **Core Abstractions**: Types/traits to extend or use

### Using Context During Decomposition

**Phase 1 Enhancement** - Use context to:
- Verify scope against documented structure
- Identify integration points from context rather than exploring
- Check for existing planning state (related work, blockers)

**Phase 2 Enhancement** - Use context for better beads:
- Match file locations to "Adding New Code" table
- Follow documented naming conventions in bead titles
- Reference existing patterns in acceptance criteria
- Add integration beads for documented touch points

**Phase 3 Enhancement** - Use context for dependencies:
- Dependencies on documented integration points become explicit beads
- Test patterns inform test bead structure
- Build/deploy patterns inform chore beads

### Context-Informed Bead Anatomy

When context exists, beads should reference it:

| Field | Context Enhancement |
|-------|---------------------|
| **Title** | Use naming conventions from context |
| **Description** | Reference location from structure map |
| **Acceptance** | Include pattern compliance from conventions |

Example with context:

```bash
# Context says: new commands go in commands/<name>.rs, follow init.rs pattern
bd create "Add export command" -t task -p 2 --parent bd-XXXX \
  -d "Create commands/export.rs following init.rs pattern. See planning-context.md §Structure Map." \
  --acceptance "Command works, tests in #[cfg(test)] module, error handling uses anyhow::Result with .context()"
```

### Acquiring Context When Missing

If no Planning Context Snapshot exists and the change is non-trivial:

1. **For minor changes**: Proceed with Phase 1 exploration
2. **For major changes**: Use `iterative-planning-context` skill first to generate context
3. **Save context**: Output to `docs/planning/context-<project>.md` for future use

### Context Staleness

Planning Context Snapshots include staleness warnings. If context is stale:
- For structural info (directory layout): Verify before relying on it
- For patterns: Sample a recent file to confirm conventions haven't changed
- For integration points: May need refresh if major changes occurred

---

## Phase 1: Understanding

**Goal**: Fully understand the epic scope before decomposing.

### Read the Epic Definition

Read the epic definition from `planning/journeys/<slug>.md`. Extract:

1. **Title and scope** — what this epic delivers
2. **Acceptance criteria** — derived from Gherkin scenarios in the journey
3. **Walking skeleton** — if this is the first epic, the thin slice
4. **Estimated complexity** — S/M/L/XL from the journey's INVEST gate
5. **Dependencies** — on other epics from this or other journeys

If a Planning Context Snapshot exists (from `iterative-planning-context`
skill), load it now for codebase structure, conventions, and integration
points.

### Key Determinations

Before creating any beads, establish clarity on:

1. **Scope**: What does the epic deliver? What is explicitly out of scope?
2. **Success criteria**: What Gherkin scenarios must pass?
3. **Constraints**: Technology stack, external dependencies, timeline?
4. **Risks**: What could go wrong? What's uncertain?
5. **Stakeholders**: Who needs to review or approve?

### Identifying Natural Boundaries

Look for natural decomposition points:

- **Functional boundaries**: Different capabilities (auth, data, UI)
- **Data boundaries**: Different data domains or storage systems
- **Interface boundaries**: API contracts between components
- **Deployment boundaries**: Services that deploy independently
- **Team boundaries**: Work that different people/agents would own

### Surfacing Hidden Dependencies

Identify non-obvious blockers:

- External services or APIs to integrate
- Data migrations or schema changes
- Infrastructure provisioning
- Documentation or compliance requirements
- Testing environments or fixtures

### Output of Phase 1

A clear mental model of:
- The project's overall structure
- 3-7 major components or phases
- Known dependencies between them
- Risks that need mitigation beads

---

## Phase 2: Decomposition

**Goal**: Break work into bounded units (beads) with clear definitions.

### Bead Anatomy

Every bead must have:

| Field | Description | Example |
|-------|-------------|---------|
| **Title** | Clear, action-oriented | "Implement user authentication API" |
| **Type** | epic, feature, task, bug, chore | `-t task` |
| **Priority** | P0 (critical) to P4 (backlog) | `-p 1` |
| **Description** | What and why | `-d "Create JWT-based auth..."` |
| **Acceptance criteria** | Testable success conditions | `--acceptance "Tests pass, docs updated"` |

### Type Selection Guide

| Type | Use When | Typical Children |
|------|----------|------------------|
| **epic** | Container for related work, multi-week effort | features, tasks |
| **feature** | User-visible capability, days to a week | tasks |
| **task** | Single unit of work, hours to a day | none or sub-tasks |
| **bug** | Defect fix | none |
| **chore** | Maintenance, refactoring, tooling | none |

### Priority Mapping

| Priority | Meaning | Use When |
|----------|---------|----------|
| P0 | Critical/Blocker | Must be done immediately, blocks everything |
| P1 | High | Core functionality, needed for MVP |
| P2 | Medium | Important but not blocking (default) |
| P3 | Low | Nice to have, polish |
| P4 | Backlog | Future consideration |

### Granularity Rules

**Too Large** (split it):
- Takes more than 2-3 days
- Has multiple distinct deliverables
- Would benefit from partial completion visibility
- Different skills or contexts needed for different parts

**Too Small** (merge it):
- Takes less than 30 minutes
- Has no independent value
- Always done together with another task
- Creates noise in the dependency graph

**Just Right**:
- Clear single deliverable
- Independently testable
- Can be claimed and completed in one session
- Dependencies are obvious and minimal

### Bead Quality Criteria (CRITICAL)

**The planning context window is precious.** When you plan, you have full visibility into requirements, research, and design decisions. That knowledge must be encoded into the bead, not left implicit.

A poorly-specified bead forces the executing agent to re-discover domain knowledge, make assumptions about intent, and produce work that technically passes but misses the point.

See `references/bead-quality-criteria.md` for the complete specification. Key requirements:

**1. Description Encodes Domain Knowledge**

Bad: "Create the vision-workshop skill for guided vision creation."

Good: "Create skills/vision-workshop/SKILL.md implementing the Product Vision Board methodology (Roman Pichler) for co-creative vision document creation. Workflow phases: 1) North Star, 2) Target Users, 3) Core Problem, 4) Value Proposition, 5) Standout Capabilities, 6) Success Criteria, 7) Anti-Goals, 8) Synthesis. Reference: skills/feature-discovery/SKILL.md for structure pattern."

**Test:** Could an agent with NO prior context execute this bead and produce the intended result?

**2. Acceptance Criteria Are Behavioral, Not Mechanical**

Acceptance criteria should verify that the bead achieves its **purpose**, not just that artifacts exist or code was written.

**Common anti-pattern: Mechanism-vs-Purpose confusion**

For a bead about "per-bead context isolation":

Bad (verifies mechanism):
```
1. Function get_next_ready_bead() exists
2. Session persistence code removed
3. CONTEXT.md file created before each bead
```

Good (verifies purpose):
```
1. Run harness on 2-bead epic: opencode session list shows 2 distinct sessions
2. Bead 2 cannot reference variables/decisions from Bead 1's context
3. CONTEXT.md timestamps differ between beads (stat shows different mtime)
```

The bad criteria verify that changes were made. The good criteria verify that **isolation actually works**.

**More examples by bead type:**

| Bead Type | Mechanical (bad) | Behavioral (good) |
|-----------|------------------|-------------------|
| File migration | "Files moved to new location" | "grep shows all references use new paths" |
| New function | "Function exists in module" | "Calling function with X input produces Y output" |
| Refactor | "Code restructured per plan" | "All existing tests still pass + new behavior verified" |
| Config change | "Config file updated" | "System behavior changes as expected when config applied" |

**Test:** Do the acceptance criteria verify PURPOSE, not just EXISTENCE?

**3. References Are Concrete**

Bad: "Follow existing patterns in the codebase."

Good: "Follow the pattern in skills/feature-discovery/SKILL.md: workflow section with phases/steps/outputs, patterns section, antipatterns section, checklist section, examples section."

**4. Scope Is Bounded**

Every bead should have implicit or explicit in-scope/out-of-scope. If scope is ambiguous, make it explicit in the description.

**Quality Checklist (verify before finalizing each bead):**

- [ ] Domain knowledge encoded (methodologies, constraints, key decisions)
- [ ] Behavioral acceptance criteria (verify purpose, not just existence)
- [ ] Concrete references (file paths, line numbers, pattern examples)
- [ ] Bounded scope (in-scope and out-of-scope explicit if ambiguous)
- [ ] Observable success (verification steps are concrete)

### INVEST at Bead Granularity

Journey-mapping applies INVEST at the epic level. Re-apply at bead
granularity — each bead should be:

- **Independent** — Minimal dependencies on other beads
- **Negotiable** — Implementation approach has latitude
- **Valuable** — Delivers a testable increment
- **Estimable** — Fits in one session (30min–3hr)
- **Small** — Single deliverable, independently testable
- **Testable** — Acceptance criteria are concrete

### Walking Skeleton First

If the epic identifies a walking skeleton, decompose it first as a
vertical slice — a thin end-to-end path touching every layer. This
surfaces integration issues early and provides a working foundation
for subsequent beads to build on.

```
Epic: First Deployment Happy Path
  Bead 1: Walking skeleton (install → deploy → verify, happy path only)
  Bead 2: Robust project detection (expand recognition)
  Bead 3: Deployment configuration (support custom settings)
  ...
```

### Gherkin-to-Acceptance Mapping

Each bead's acceptance criteria should trace to specific Gherkin
scenarios from the journey file. This ensures the bead verifies
**purpose** (the scenario's intent), not just existence of code.

```
Journey scenario:
  Scenario: Tool recognizes standard project structure
    Given a developer has a standard project structure
    When the developer adds the deployment tool
    Then the tool reports successful project recognition

Bead acceptance criteria:
  1. Running `mytool init` in a standard Node.js project reports success
  2. Running `mytool init` in a standard Go project reports success
  3. Project type is detected and stored in config
```

The scenario describes **what**; the bead criteria describe **how to
verify it** in concrete, testable terms.

### Decomposition Patterns

**Top-Down**: Start with epic, break into features, break into tasks
```
Epic: Build Authentication System
  Feature: User Registration
    Task: Create registration form
    Task: Implement email validation
    Task: Add password strength checker
  Feature: Login Flow
    Task: Create login form
    Task: Implement JWT generation
    Task: Add session management
```

**Horizontal Layers**: Decompose by system layer
```
Epic: Add Search Feature
  Task: Design search API contract
  Task: Implement search backend
  Task: Create search UI component
  Task: Add search result caching
  Task: Write integration tests
```

**Vertical Slices**: End-to-end thin slices
```
Epic: User Dashboard
  Task: Basic dashboard with user info (full stack)
  Task: Add activity feed (full stack)
  Task: Add settings panel (full stack)
```

### Integration & Synthesis Beads (REQUIRED for multi-component epics)

When an epic has multiple features or components that must work together, you MUST add integration beads as leaf nodes in the DAG. These execute last (after all feature beads) and ensure the system works as a coherent whole.

**Why Integration Beads Are Necessary:**
- Each agent optimizes locally for its bead
- Agents produce documentation that only describes their piece
- Cross-component contracts can drift silently
- No agent has the full picture without explicit integration tasks

**Standard Integration Beads:**

For any epic with 2+ features, add:

```
Feature: Integration & Documentation (P3 - depends on all other features)
  Task: Documentation synthesis
    - Generate unified README from spec + all component implementations
    - Ensure docs tell the complete story, not just one piece
    - Acceptance: README covers full system architecture, setup, and usage
  
  Task: Cross-component verification
    - Verify API contracts match between client and server
    - Run end-to-end tests across component boundaries
    - Acceptance: E2E test passes: data flows through entire system
  
  Task: Deployment documentation
    - Verify deployment docs cover full stack (not just one component)
    - Acceptance: New developer can deploy entire system from docs
```

**Dependency Pattern:**

Integration beads use fan-in - they depend on ALL feature beads:

```bash
# Integration depends on all features
bd dep add bd-integration bd-feature1
bd dep add bd-integration bd-feature2
bd dep add bd-integration bd-feature3
```

This ensures integration beads naturally execute last, after all components are built.

**Example: Multi-Component App**

```
Epic: Oracy MVP
  Feature: Server Core (P1)
    └── [server tasks...]
  Feature: Mobile Core (P1)
    └── [mobile tasks...]
  Feature: Integration & Documentation (P3)
    ├── Task: Write unified README from spec + implementations
    ├── Task: Verify mobile API client matches server OpenAPI spec
    ├── Task: E2E test: record audio → upload → transcript appears
    └── Task: Verify deployment docs cover full stack
```

**When to Skip Integration Beads:**
- Single-component epics (no cross-component boundaries)
- Pure refactoring epics (no new functionality to document)
- Bug-fix epics (no integration surface changed)

**Do NOT skip integration beads just because they're "P3"** - they're P3 because they must run last, not because they're optional.

---

## Phase 3: Dependency Mapping

**Goal**: Establish relationships between beads.

### Dependency Types

| Type | Semantics | Use When | Command |
|------|-----------|----------|---------|
| **blocks** | A must complete before B starts | Sequential work, technical dependency | `bd dep add B A` |
| **parent-child** | Hierarchical containment | Epic contains features/tasks | `--parent <epic-id>` |
| **related** | Soft connection, informational | Related but independent work | `bd dep add A B -t related` |
| **discovered-from** | Work found during other work | Bug found while implementing feature | `bd dep add B A -t discovered-from` |

### Dependency Decision Tree

```
Is B inside A (containment)?
  YES -> parent-child (use --parent on create)
  NO  -> Continue...

Must A complete before B can start?
  YES -> blocks (bd dep add B A)
  NO  -> Continue...

Was B discovered while working on A?
  YES -> discovered-from (bd dep add B A -t discovered-from)
  NO  -> Continue...

Are A and B related but independent?
  YES -> related (bd dep add A B -t related)
  NO  -> No dependency needed
```

### Blocking Dependency Patterns

**Sequential Chain**:
```bash
# A -> B -> C (A blocks B, B blocks C)
bd dep add bd-B bd-A
bd dep add bd-C bd-B
```

**Fan-Out** (one blocks many):
```bash
# Setup blocks all implementation tasks
bd dep add bd-impl1 bd-setup
bd dep add bd-impl2 bd-setup
bd dep add bd-impl3 bd-setup
```

**Fan-In** (many block one):
```bash
# All features must complete before release
bd dep add bd-release bd-feature1
bd dep add bd-release bd-feature2
bd dep add bd-release bd-feature3
```

**Diamond** (convergent paths):
```bash
# A -> B, A -> C, B -> D, C -> D
bd dep add bd-B bd-A
bd dep add bd-C bd-A
bd dep add bd-D bd-B
bd dep add bd-D bd-C
```

### Detecting Cycles Before Instantiation

Before adding dependencies, mentally trace the graph:
- Can I reach A from A by following dependencies? If yes, cycle exists.
- Draw it out if complex.

After instantiation, verify:
```bash
bd dep cycles  # Should return empty
```

---

## Phase 4: Instantiation

**Goal**: Create the beads in the correct order with proper relationships.

### Order of Creation

1. **Epics first** (top of hierarchy)
2. **Features** (with `--parent` pointing to epic)
3. **Tasks** (with `--parent` pointing to epic or feature)
4. **Dependencies** (after all beads exist)

### Traceability (Before Creating Epic)

Before creating the epic, establish the traceability chain. Every epic
must reference its source journey file:

```bash
bd create "Epic Title" \
  -t epic \
  -p 1 \
  -d "[source_journey: journeys/first-deployment.md#epic-name] Description of the goal" \
  --acceptance "All child beads complete, acceptance criteria from journey verified"
```

The `source_journey` reference links the epic back to its origin in the
planning layer. Child beads trace back through the parent relationship —
only the epic needs this reference.

Capture the returned epic ID (e.g., `bd-a3f8`).

### Creating Child Tasks with Hierarchical IDs

```bash
# Using --parent auto-generates hierarchical IDs: bd-a3f8.1, bd-a3f8.2, etc.
bd create "First task" -t task -p 2 --parent bd-a3f8 \
  -d "Description of what this task accomplishes" \
  --acceptance "Specific testable criteria"

bd create "Second task" -t task -p 2 --parent bd-a3f8 \
  -d "Description" \
  --acceptance "Criteria"
```

### Adding Dependencies

After all beads are created:

```bash
# Task 2 depends on Task 1 (Task 1 blocks Task 2)
bd dep add bd-a3f8.2 bd-a3f8.1

# Task 3 depends on Task 2
bd dep add bd-a3f8.3 bd-a3f8.2

# Final task depends on multiple tasks (fan-in)
bd dep add bd-a3f8.5 bd-a3f8.3
bd dep add bd-a3f8.5 bd-a3f8.4
```

### Batch Creation from Markdown

For larger plans, create a markdown file:

```markdown
# Project Plan

## Epic: Authentication System
- Type: epic
- Priority: 1
- Description: Implement complete auth system

### Task: Design API contract
- Type: task
- Priority: 1
- Blocks: Implement auth endpoints

### Task: Implement auth endpoints
- Type: task
- Priority: 2
```

Then: `bd create -f plan.md`

---

## Phase 5: Validation

**Goal**: Verify the dependency graph is complete, correct, and executable.

### Validation Checklist

#### 0. Bead Quality Gate (REQUIRED)

Before structural validation, verify each bead meets quality criteria:

```bash
# Check description length (guards against thin beads)
bd list --parent $EPIC_ID --json | jq -e '
  [.[] | select((.description | length) < 100)] 
  | if length > 0 then 
      "QUALITY FAIL: Thin descriptions on: \([.[].id] | join(", "))" | halt_error 
    else empty end
'

# Check acceptance criteria count (minimum 2 criteria per bead)
# Note: This guards against missing/thin criteria but cannot detect mechanical-vs-behavioral.
# The planner must self-audit for behavioral criteria using the test: "Does this verify PURPOSE?"
bd list --parent $EPIC_ID --json | jq -e '
  [.[] | select(
    .acceptance_criteria == null or 
    (.acceptance_criteria | split("\n") | map(select(length > 0)) | length) < 2
  )] 
  | if length > 0 then 
      "QUALITY FAIL: Insufficient acceptance criteria on: \([.[].id] | join(", "))" | halt_error 
    else empty end
'
```

**Quality Failures Are Hard Stops.** Do not proceed to execution with thin beads.

**Remediation:** For each failing bead:
1. Ask: "What domain knowledge does the executor need that I have right now?"
2. Encode that knowledge into the description
3. Ask: "How will we verify this achieves its PURPOSE, not just exists?"
4. Add behavioral acceptance criteria

#### 1. Ready Work Exists

```bash
bd ready
```

**Expected**: At least one bead with status `open` and no blockers.

**If empty**: Check for cycles or missing root tasks.

#### 2. No Cycles

```bash
bd dep cycles
```

**Expected**: Empty output (no cycles detected).

**If cycles found**: Remove one edge to break the cycle.

#### 3. Dependency Tree is Coherent

```bash
bd dep tree bd-EPIC-ID
```

**Expected**: All tasks appear in the tree, logical ordering.

**Check for**:
- Orphaned beads (not connected to epic)
- Missing dependencies (parallel tasks that should be sequential)
- Over-connected graph (unnecessary dependencies)

#### 4. Visualize Execution Order

```bash
bd graph bd-EPIC-ID
```

**Expected**: ASCII visualization showing layers:
- Layer 0: No dependencies (start here)
- Higher layers: Depend on lower layers
- Same layer: Can run in parallel

#### 5. Epic Status

```bash
bd epic status bd-EPIC-ID
```

**Expected**: Shows completion percentage and blocked/ready counts.

### Orphan Detection

List all beads and verify each is either:
- A root epic, OR
- Has a `--parent` relationship, OR  
- Has at least one `blocks` or `related` dependency

```bash
bd list --all --json | jq '.[] | select(.parent == null and .dependencies == [])'
```

### Planning Complete: Handoff to Execution

**Phase 5 is the END of the beads-plan skill's responsibility.**

After validation passes, the planning agent outputs a handoff message and STOPS:

```
Planning complete.

Epic: <epic-id>
Branch: <branch-name>  
Work Type: <type>
Beads: <count> tasks ready for execution

To execute:
  beads-harness <epic-id>
```

**The planning agent MUST NOT execute beads.** Execution is a separate phase with a separate agent (the `beads-harness`). This separation ensures:

1. **Beads are self-contained** - If the planning agent could execute, beads might rely on conversational context that isn't encoded in the bead description.

2. **Execution is scalable** - The harness can parallelize across multiple agents. Manual execution cannot.

3. **Recovery is possible** - If execution fails, the plan remains intact for retry.

See `references/bead-quality-criteria.md` § "Planning/Execution Boundary" for details.

**Exception:** The planning agent may execute only if the user explicitly requests it (e.g., "execute the first bead now").

---

## Reference Material

For detailed reference material extracted for modularity, see:

- **[Swarm Execution Guidance](references/swarm-execution.md)** — Multi-agent coordination patterns, claim/close workflow, conflict avoidance
- **[Worked Example](references/worked-example.md)** — Complete walkthrough: CLI tool with 3 subcommands, from understanding through validation
- **[BD Command Reference](references/bd-command-reference.md)** — Full `bd` CLI command reference (create, dep, ready, update, close, sync)

---

## Anti-Patterns

### Over-Decomposition

**Symptom**: 50+ tiny tasks for a simple feature.
**Problem**: Graph noise, overhead exceeds value.
**Fix**: Merge related micro-tasks, aim for 30min-3hr tasks.

### Under-Decomposition

**Symptom**: One "Implement everything" task.
**Problem**: No progress visibility, can't parallelize.
**Fix**: Break into functional units with clear boundaries.

### Missing Dependencies

**Symptom**: `bd ready` shows tasks that actually can't start.
**Problem**: Implicit dependencies not captured.
**Fix**: Trace each task's true prerequisites, add blocking deps.

### Cycle Creation

**Symptom**: `bd dep cycles` returns results.
**Problem**: Deadlock - nothing can complete.
**Fix**: Identify the spurious edge, remove it.

### Orphaned Beads

**Symptom**: Tasks not connected to any epic or dependency.
**Problem**: Lost work, unclear ownership.
**Fix**: Add parent or dependency relationships.

### Vague Acceptance Criteria

**Symptom**: "It works" as acceptance criteria.
**Problem**: No way to verify completion.
**Fix**: Specify testable conditions: "Tests pass", "API returns 200", "Config loads".

### Priority Inflation

**Symptom**: Everything is P0.
**Problem**: No way to triage, paralysis.
**Fix**: Reserve P0 for true blockers, use P1-P2 for normal work.

---

## Troubleshooting

### "bd ready" Returns Empty

**Causes**:
1. All tasks have blockers - check `bd blocked`
2. Cycle exists - run `bd dep cycles`
3. All tasks closed - run `bd list --status open`

**Fix**: Add a task with no dependencies, or break the cycle.

### "bd dep add" Fails

**Causes**:
1. Issue ID doesn't exist - verify with `bd show <id>`
2. Would create cycle - check existing deps first

**Fix**: Verify IDs, reconsider dependency direction.

### Hierarchical ID Not Generated

**Cause**: Missing `--parent` flag on create.

**Fix**: Use `--parent bd-EPIC-ID` when creating child tasks.

### Sync Conflicts

**Cause**: Multiple agents modified same JSONL.

**Fix**: 
```bash
git pull --rebase
bd import -i .beads/issues.jsonl
bd sync
```

### Agent Can't Claim

**Cause**: Another agent already claimed.

**Fix**: Query `bd ready --unassigned` to find unclaimed work.

---

## Related Skills

| Skill | Relationship |
|-------|--------------|
| `journey-mapping` | Produces epic definitions that this skill decomposes |
| `iterative-planning-context` | Produces Planning Context Snapshots consumed during decomposition |
| `beads-execute` | Executes the beads created by this skill |
| `validate-pipeline` | Quality gate per bead during execution |
