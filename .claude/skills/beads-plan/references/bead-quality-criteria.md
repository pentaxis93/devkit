# Bead Quality Criteria

> Version: 1.0.0
> Purpose: Define what constitutes a well-specified bead that an agent can execute without re-discovering context

## The Core Problem

Beads are work units for autonomous agent execution. A poorly-specified bead forces the executing agent to:
- Re-discover domain knowledge that the planning agent already had
- Make assumptions about intent that may diverge from the plan
- Produce work that technically passes acceptance criteria but misses the point

**The planning context window is precious.** When we plan, we have full visibility into requirements, research, and design decisions. That knowledge must be encoded into the bead, not left implicit.

## Quality Criteria

### 1. Description Encodes Domain Knowledge

**Bad:**
```
Create the vision-workshop skill for guided vision creation.
```

**Good:**
```
Create skills/vision-workshop/input.yaml implementing the Product Vision Board 
methodology (Roman Pichler) for co-creative vision document creation.

Workflow phases:
1. North Star - Elicit the overarching goal/positive change
2. Target Users - Identify primary (daily users) and secondary (indirect beneficiaries)
3. Core Problem - Define the pain point or unmet need being addressed
4. Value Proposition - Articulate why users will choose this over alternatives
5. Standout Capabilities - Limit to 3-5 key differentiators (not a feature list)
6. Success Criteria - Define measurable business and user outcomes
7. Anti-Goals - Explicitly capture what this is NOT building
8. Synthesis - Generate/update planning/vision.org with captured content

Output: planning/vision.org updated with all sections populated
```

**Test:** Could an agent with no prior context execute this bead and produce the intended result?

### 2. Acceptance Criteria Are Behavioral, Not Mechanical

Acceptance criteria should verify that the bead achieves its **purpose**, not just that artifacts exist or code was written.

**Bad:**
```
- File exists
- Compiles successfully
- Tests pass
```

**Good:**
```
- Skill has 7+ workflow phases covering: north star, users, problem, value prop, 
  capabilities, success criteria, anti-goals, synthesis
- Each phase includes guiding questions in principles section
- Skill outputs to planning/vision.org (referenced in workflow)
- Example section demonstrates complete workshop flow with sample Q&A
- Checklist section defines "vision ready" criteria for handoff to features
- cargo test passes with skill count updated in skills.rs (line ~244)
```

**Common anti-pattern: Mechanism-vs-Purpose confusion**

For infrastructure/refactoring beads, criteria often verify the mechanism rather than the purpose:

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

**Quick reference by bead type:**

| Bead Type | Mechanical (bad) | Behavioral (good) |
|-----------|------------------|-------------------|
| File migration | "Files moved to new location" | "grep shows all references use new paths" |
| New function | "Function exists in module" | "Calling function with X produces Y" |
| Refactor | "Code restructured per plan" | "Existing tests pass + new behavior verified" |
| Config change | "Config file updated" | "System behavior changes as expected" |

**Test:** Do the acceptance criteria verify that the work achieves its purpose, not just that it exists?

### 3. References Are Concrete

**Bad:**
```
Follow existing patterns in the codebase.
```

**Good:**
```
Follow the pattern in skills/org-planning/input.yaml:
- workflow: section with phases, steps, outputs, elisp/shell examples
- patterns: section for reusable approaches
- antipatterns: section for what to avoid
- checklist: section for verification
- examples: section with realistic scenarios
```

**Test:** Can the agent find exactly what to reference without searching?

### 4. Scope Is Bounded

**Bad:**
```
Implement user authentication.
```

**Good:**
```
Implement JWT token generation and validation.

In scope:
- Token generation with configurable expiry
- Token validation middleware
- Refresh token flow

Out of scope (separate beads):
- User registration
- Password reset
- OAuth integration
```

**Test:** Is it clear what the bead does NOT include?

### 5. Success Is Observable

**Bad:**
```
The feature works correctly.
```

**Good:**
```
Validation:
- `cargo test skill_vision_workshop` passes
- Manual test: invoke skill, answer prompts, verify vision.org updated
- planning/vision.org contains all 7 sections with placeholder content replaced
```

**Test:** Can success be verified without subjective judgment?

## Quality Checklist

Before finalizing a bead, verify:

- [ ] **Domain knowledge encoded** - Key decisions, methodologies, and constraints are in the description
- [ ] **Behavioral acceptance criteria** - Criteria verify purpose, not just existence
- [ ] **Concrete references** - File paths, line numbers, pattern examples are specific
- [ ] **Bounded scope** - In-scope and out-of-scope are explicit
- [ ] **Observable success** - Verification steps are concrete and automatable

## Skill-Building Beads: Research Requirement

**Skills are where intelligence compounds.** Unlike regular implementation beads, skills encode reusable methodologies that will be invoked many times. A poorly-researched skill propagates mediocrity across all future uses.

### The Research Imperative

Before implementing any skill, the executor MUST:

1. **Delegate research to multiple agents** - Commission parallel research exploring:
   - Established methodologies in the domain
   - Open-source tools and solutions
   - Best practices from industry/academia
   - Anti-patterns and failure modes

2. **Synthesize findings** - Identify the best-in-class approach before implementing

3. **Encode research provenance** - The skill should reference the methodologies it implements

### Skill Bead Structure

Every skill-building bead MUST include a research phase:

```yaml
title: Create [skill-name] skill

description: |
  PHASE 0: RESEARCH (delegate to Sonnet agents)
  Commission parallel research exploring:
  - "What are best-practice methodologies for [domain]?"
  - "What open-source tools exist for [problem]?"
  - "What are common failure modes in [domain]?"
  
  Research briefs should request:
  - Top 3-5 methodologies ranked by [relevant criteria]
  - Notable open-source tools (prefer text-based, CLI-friendly)
  - Recommended output format/template
  - Key insights about what makes [domain] successful
  
  PHASE 1: IMPLEMENTATION (after research synthesis)
  Create skills/[name]/input.yaml implementing [chosen methodology]...
  
  [rest of specification informed by research]

acceptance_criteria: |
  - Research phase completed with findings from 2+ parallel agents
  - Chosen methodology is justified (not arbitrary)
  - Skill references the methodology it implements
  - Alternative approaches documented (for future reference)
  - [behavioral criteria for the skill itself]
```

### Why This Matters

Consider the difference:

**Without research:**
```
Create a vision-workshop skill for guided vision creation.
```
The agent invents an approach. It might be good, might be mediocre. We'll never know what we missed.

**With research:**
```
Research found Product Vision Board (Roman Pichler) highest-rated for clarity+practicality.
Alternative PR/FAQ (Amazon) better for large initiatives. Lean Canvas too business-model focused.
Implementing Product Vision Board with 8 phases...
```
The agent implements a proven methodology. The skill benefits from collective wisdom.

### Research Delegation Pattern

When executing a skill-building bead, use this pattern:

```
Commission 3-4 parallel research agents with different briefs:

Agent 1: "Research best-practice methodologies for [domain]"
Agent 2: "Research open-source tools for [problem]"  
Agent 3: "Research failure modes and anti-patterns in [domain]"
Agent 4: "Research how [domain] integrates with [related domains]"

Each agent should use web search to find current best practices.
Synthesize findings before proceeding to implementation.
```

### Skill Research Checklist

Before implementing any skill:

- [ ] Research delegated to 2+ parallel agents
- [ ] Multiple methodologies compared
- [ ] Open-source landscape surveyed
- [ ] Chosen approach justified with rationale
- [ ] Alternative approaches documented for reference
- [ ] Failure modes/anti-patterns identified

## Schema and Specification Beads: Structure Requirement

**Schemas define contracts.** A schema bead doesn't just create a file - it establishes the structure that other artifacts must conform to. A vague schema bead produces inconsistent artifacts downstream.

### The Structure Imperative

Schema/specification beads MUST include:

1. **Complete field definitions** - Every field that the schema defines, with:
   - Field name and type
   - Required vs optional
   - Purpose/semantics
   - Valid values or constraints

2. **Relationships to other artifacts** - How this schema connects to:
   - Upstream artifacts (what feeds into it)
   - Downstream artifacts (what consumes it)
   - Cross-references (ID linking patterns)

3. **Example instance** - A concrete example showing the schema populated with realistic data

### Schema Bead Structure

```yaml
title: Create [artifact] schema

description: |
  Create docs/schemas/[artifact].yaml defining the structure for [artifact] documents.
  
  FIELDS:
  - field_name (type, required/optional): Description and purpose
    Valid values: [constraints or enumeration]
  - field_name (type, required/optional): Description
    ...
  
  RELATIONSHIPS:
  - Links TO: [upstream artifact] via [property name]
  - Links FROM: [downstream artifact] via [property name]
  - Cross-references: [ID pattern, e.g., "journey-<slug>"]
  
  EXAMPLE INSTANCE:
  ```yaml
  field_name: example_value
  another_field: example_value
  ```
  
  VALIDATION RULES:
  - [Rule 1: e.g., "ID must be unique across all artifacts"]
  - [Rule 2: e.g., "STATUS must be one of: draft, ready, implementing, done"]

acceptance_criteria: |
  - Schema file exists at specified location
  - All fields from description are present with correct types
  - Relationships section documents upstream/downstream links
  - Example section shows valid populated instance
  - Schema is valid YAML (parseable)
  - Downstream templates conform to schema
```

### Why This Matters

Consider a schema bead like:

**Bad:**
```
Create vision.yaml schema defining the structure for vision documents.
```
The agent invents fields. Templates and skills may expect different fields. Inconsistency cascades.

**Good:**
```
Create docs/schemas/vision.yaml with fields:
- id (string, required): Unique identifier, format "vision-root"
- north_star (string, required): One inspiring sentence
- target_users.primary (string, required): Who uses this daily
- target_users.secondary (string, optional): Who benefits indirectly
- core_problem (string, required): Pain point being addressed
- value_proposition (string, required): Why users choose this
- capabilities (array[string], required): 3-5 key differentiators, max 5
- success_criteria.business (string, required): Measurable business outcome
- success_criteria.user (string, required): Measurable user outcome
- anti_goals (array[string], required): What this is NOT
- open_questions (array[string], optional): Assumptions to validate
- status (enum, required): draft|reviewed|approved
- created (date, required): Creation timestamp
- updated (date, required): Last update timestamp
```
The agent creates exactly this structure. Templates conform. Skills know what to expect.

### Template Beads: Schema Conformance

Template beads (that create initial artifact files) MUST:

1. **Reference their schema** - Link to the schema they implement
2. **Include all required fields** - With placeholder content
3. **Match schema structure exactly** - Field names, nesting, types
4. **Document the placeholder pattern** - How users should replace placeholder content

```yaml
title: Create [artifact].org template

description: |
  Create planning/[artifact].org implementing the schema from docs/schemas/[artifact].yaml
  
  SCHEMA REFERENCE: docs/schemas/[artifact].yaml
  
  TEMPLATE STRUCTURE:
   [Show the markdown structure that maps to schema fields]
  
  PLACEHOLDER PATTERN:
  - Required fields: Use "[REPLACE: description of what goes here]"
  - Optional fields: Use "[OPTIONAL: description]" or omit
  - Lists: Include one example item with "[REPLACE: ...]"

acceptance_criteria: |
  - Template exists at specified location
  - All required schema fields have corresponding org sections
  - Placeholder pattern is consistent throughout
  - Template is valid markdown (well-formed headings and frontmatter)
  - ID property matches schema ID format
```

### Schema/Template Checklist

Before finalizing schema or template beads:

- [ ] All fields defined with type and required/optional
- [ ] Relationships to other artifacts documented
- [ ] Example instance included
- [ ] Validation rules specified
- [ ] Downstream templates reference the schema
- [ ] Cross-reference ID patterns are explicit

## ADR (Architecture Decision Record) Beads

**ADRs capture the WHY behind decisions.** An ADR bead doesn't just create a file - it documents reasoning that future developers (human and AI) will rely on to understand the system.

### ADR Bead Structure

ADR beads MUST include:

1. **The decision being recorded** - Clear statement of what was decided
2. **Context that necessitated the decision** - What problem or question prompted this?
3. **Alternatives considered** - What other options existed? Why were they rejected?
4. **Consequences** - What are the implications (positive and negative)?
5. **References** - Related ADRs, documents, or discussions

```yaml
title: Create ADR-NNN: [Decision Title]

description: |
  Create docs/adr/NNN-[slug].md documenting the decision to [brief statement].
  
  CONTEXT:
  [What problem or question prompted this decision? What forces are at play?]
  
  DECISION:
  [What is being decided? Be specific and unambiguous.]
  
  ALTERNATIVES CONSIDERED:
  1. [Alternative 1]: [Why rejected]
  2. [Alternative 2]: [Why rejected]
  3. [Alternative 3]: [Why rejected or why chosen]
  
  CONSEQUENCES:
  Positive:
  - [Benefit 1]
  - [Benefit 2]
  
  Negative:
  - [Tradeoff 1]
  - [Tradeoff 2]
  
  REFERENCES:
  - [Related ADR or document]
  - [Discussion or research that informed this]
  
  TEMPLATE: Follow docs/adr/000-template.md format

acceptance_criteria: |
  - ADR file exists at docs/adr/NNN-[slug].md
  - Status section shows "Accepted" with date
  - Context section explains the forces/problem (not just "we needed X")
  - Decision section is unambiguous
  - Alternatives section lists 2+ options with rejection rationale
  - Consequences section has both positive and negative items
  - Format matches existing ADRs (000-template.md)
```

### Why This Matters

**Bad ADR bead:**
```
Create ADR for the planning hierarchy decision.
```
The agent writes a generic ADR. The reasoning from the planning session is lost.

**Good ADR bead:**
```
CONTEXT: The project has beads for execution, but nothing for the co-creative 
phases (vision, features, journeys) that precede implementation.

DECISION: Adopt a 6-phase planning pipeline with vision→features→journeys as 
co-creative phases feeding into beads-plan→beads-execute as autonomous phases.

ALTERNATIVES:
1. Single flat backlog: Rejected - loses the what/how boundary
2. All in one tool: Rejected - co-creative collaboration needs different structure than execution
3. External tool (Notion, etc.): Rejected - breaks text-based, version-controlled workflow
```
The reasoning is preserved. Future agents understand WHY this architecture exists.

## Specification Beads

**Specifications are authoritative definitions.** A spec bead creates a document that other artifacts reference as the source of truth.

### Specification Bead Structure

Spec beads MUST include:

1. **What the spec defines** - Clear scope of what's being specified
2. **Structure of the spec** - Sections, their purpose, and what each contains
3. **Key content for each section** - Not implementation details, but WHAT must be covered
4. **Relationships** - What artifacts reference this spec? What does it reference?

```yaml
title: Create [System] Specification

description: |
  Create docs/[name]-spec.md as the authoritative definition for [system/process].
  
  SPEC SCOPE:
  [What does this specification define? What is OUT of scope?]
  
  SPEC STRUCTURE:
  1. [Section 1 Name]: [What this section covers]
     Key content: [Specific items that MUST be addressed]
  2. [Section 2 Name]: [What this section covers]
     Key content: [Specific items]
  ...
  
  KEY DEFINITIONS:
  - [Term 1]: [How this spec defines it]
  - [Term 2]: [Definition]
  
  DIAGRAMS/VISUALS:
  - [Diagram 1]: [What it shows, format (mermaid/ascii/etc)]
  
  REFERENCES:
  - Upstream: [What this spec is derived from]
  - Downstream: [What artifacts implement this spec]
  
  VERSIONING: Include version number and changelog section

acceptance_criteria: |
  - Spec exists at specified location
  - All sections from structure are present
  - Key content items are addressed in each section
  - Definitions section defines all domain terms
  - Diagrams are included and render correctly
  - Version number is present
  - Downstream artifacts can reference this as source of truth
```

## Skill Modification Beads

**Updating skills requires understanding the existing structure.** A skill modification bead must specify exactly what changes, where, and why.

### Skill Modification Bead Structure

Skill modification beads MUST include:

1. **Which skill is being modified** - Exact path
2. **Current state** - What exists now that's being changed
3. **Changes to make** - Specific additions, removals, or modifications
4. **Rationale** - Why these changes are needed
5. **Backward compatibility** - Do existing uses still work?

```yaml
title: Update [skill-name] skill with [feature]

description: |
  Modify skills/[skill-name]/input.yaml to add [capability].
  
  SKILL LOCATION: skills/[skill-name]/input.yaml
  
  CURRENT STATE:
  [What does the skill currently do? What sections exist?]
  
  CHANGES:
  1. ADD to [section]:
     - [New item 1]: [Purpose]
     - [New item 2]: [Purpose]
  
  2. MODIFY [section]:
     - Change [existing item] from [old] to [new]
     - Reason: [Why this change]
  
  3. ADD new section [section-name]:
     [Content of new section]
  
  RATIONALE:
  [Why are these changes needed? What problem do they solve?]
  
  BACKWARD COMPATIBILITY:
  - Existing [feature] still works: [yes/no, explanation]
  - Existing triggers still fire: [yes/no]
  
  COMPILE REQUIREMENTS:
  - Regenerate SKILL.md from input.yaml
  - Update skills.rs if exports change
  - Update test counts if new skill added

acceptance_criteria: |
  - input.yaml modified with all specified changes
  - SKILL.md regenerated and reflects changes
  - Existing functionality preserved (unless explicitly deprecated)
  - New sections/items follow existing formatting patterns
  - skills.rs updated if needed
  - cargo test passes
```

### Why This Matters

**Bad skill modification bead:**
```
Add traceability queries to org-planning skill.
```
What queries? Where in the skill? What do they return?

**Good skill modification bead:**
```
SKILL LOCATION: skills/org-planning/input.yaml

CHANGES:
1. ADD to api_reference section:
   - agent-query-by-vision: Query all features/journeys/tasks linked to vision
   - agent-query-by-feature: Query all journeys/tasks linked to feature
   - agent-trace-to-vision: Given task ID, return chain to vision

2. ADD to workflow section, new phase "Cross-Phase Queries":
   - Steps for querying across planning hierarchy
   - Example queries for common use cases

3. ADD to patterns section:
   - "Traceability Chain Navigation": How to walk up/down the hierarchy
```

## Validation Beads

**Validation beads are gates, not just checklists.** They must specify exactly what passes, what fails, and how to remediate failures.

### Validation Bead Structure

Validation beads MUST include:

1. **What is being validated** - Scope of validation
2. **Validation steps in order** - Exact commands or checks
3. **Pass criteria for each step** - What output indicates success
4. **Fail handling** - What to do when a step fails
5. **Dependencies** - What must be true before validation can run

```yaml
title: Validate [scope] implementation

description: |
  Run validation suite to verify [scope] is correctly implemented.
  
  PREREQUISITES:
  - [Prerequisite 1]: [How to verify it's met]
  - [Prerequisite 2]: [How to verify]
  
  VALIDATION STEPS (in order):
  
  1. [Step name]:
     Command: [exact command to run]
     Pass: [what output indicates success]
     Fail: [what output indicates failure]
     Remediation: [what to do if it fails]
  
  2. [Step name]:
     Command: [command]
     Pass: [criteria]
     Fail: [criteria]
     Remediation: [fix]
  
  ...
  
  FINAL GATE:
  All steps must pass. Any failure blocks completion.
  
  POST-VALIDATION:
  [Any cleanup or reporting after validation passes]

acceptance_criteria: |
  - All validation steps executed in order
  - Each step's pass/fail criteria evaluated
  - Any failures documented with specific error
  - Remediation attempted for failures (or documented why not)
  - Final status: PASS (all steps green) or FAIL (with specifics)
```

### Why This Matters

**Bad validation bead:**
```
Run tests and make sure everything passes.
```
What tests? What counts as passing? What if clippy has warnings?

**Good validation bead:**
```
VALIDATION STEPS:

1. Rust compilation:
   Command: cargo build 2>&1
   Pass: "Finished" in output, exit code 0
   Fail: Any "error[E" in output
   Remediation: Fix compilation errors before proceeding

2. Test suite:
   Command: cargo test 2>&1
   Pass: "test result: ok" with 0 failures
   Fail: Any "FAILED" or non-zero failure count
   Remediation: Fix failing tests
   Note: Check skill count tests specifically (skills.rs line ~244)

3. Clippy:
   Command: cargo clippy -- -D warnings 2>&1
   Pass: Exit code 0, no warnings
   Fail: Any warning or error output
   Remediation: Fix clippy warnings (they're treated as errors)

4. Format check:
   Command: cargo fmt --check 2>&1
   Pass: Exit code 0, no output
   Fail: Any file listed as needing formatting
   Remediation: Run cargo fmt
   
5. Skill bundling:
   Command: cargo build && ./target/debug/mytool skill list
   Pass: All new skills appear in output
   Fail: New skill missing from list
   Remediation: Check skills.rs constant and bundled_skills() entry
```

## Anti-Patterns

### "Thin Bead" Anti-Pattern

A bead with minimal description that assumes the executor will figure it out:

```yaml
title: Create feature X
description: Implement the feature as discussed.
acceptance: Feature works.
```

This bead will produce unpredictable results because "as discussed" lives only in the planner's context window, which the executor doesn't have.

### "Kitchen Sink" Anti-Pattern

A bead that tries to encode everything, becoming a specification document:

```yaml
title: Create feature X
description: |
  [2000 words of detailed specification including implementation approach,
   data structures, API design, error handling, test strategy...]
```

This bead conflates planning (what) with implementation (how). The executor loses autonomy to make good implementation decisions.

### The Balance

A well-specified bead encodes:
- **What** to build (domain knowledge, methodology, constraints)
- **Why** it matters (context for judgment calls)
- **What success looks like** (behavioral criteria)

A well-specified bead does NOT encode:
- **How** to implement (that's executor sovereignty)
- **Every detail** (trust the executor to handle reasonable gaps)

## Integration with /beads Workflow

The `/beads` workflow MUST validate bead quality during Phase 7 (Validation Gate):

```
Quality Checks:
- Description length >= 100 characters (guards against thin beads)
- Description contains methodology/approach reference OR explicit "implementation discretion"
- Acceptance criteria count >= 3 (guards against mechanical-only criteria)
- At least one acceptance criterion references observable behavior
```

Beads failing quality checks should block execution until remediated.

## Examples

### Example: Skill Creation Bead (Good)

```yaml
title: Create vision-workshop skill

description: |
  Create skills/vision-workshop/input.yaml implementing the Product Vision Board 
  methodology (Roman Pichler) for co-creative vision document creation.

  Workflow phases:
  1. North Star - Elicit the overarching goal/positive change
  2. Target Users - Identify primary and secondary users
  3. Core Problem - Define the pain point being addressed
  4. Value Proposition - Why users will choose this
  5. Standout Capabilities - 3-5 key differentiators
  6. Success Criteria - Measurable business and user outcomes
  7. Anti-Goals - What this is NOT
  8. Synthesis - Update planning/vision.org

  Each phase should include:
  - Guiding question (in principles)
  - Examples of good/bad answers
  - Validation criteria

  Reference: skills/org-planning/input.yaml for structure pattern
  Output: planning/vision.org updated

acceptance_criteria: |
  - Skill has 7+ workflow phases matching the methodology above
  - Each phase has principles section with guiding questions
  - workflow.outputs references planning/vision.org
  - Example section shows complete workshop flow
  - Checklist defines "vision ready" criteria
  - Skill compiles: SKILL.md generated without errors
  - skills.rs updated with new constant and bundled_skills entry
  - cargo test passes (skill count tests updated)
```

### Example: Bugfix Bead (Good)

```yaml
title: Fix race condition in session cleanup

description: |
  The session cleanup task runs every 5 minutes and can conflict with 
  active session operations, causing spurious "session not found" errors.

  Root cause: cleanup acquires table lock before checking session activity
  
  Fix approach: Check session last_active timestamp before acquiring lock.
  Only proceed with cleanup if session inactive > cleanup_interval.

  Affected file: src/services/session.rs (cleanup_expired_sessions fn, ~line 142)
  Related: src/services/session.rs (get_session fn that's failing)

acceptance_criteria: |
  - cleanup_expired_sessions checks last_active before lock acquisition
  - Regression test added: concurrent session access during cleanup
  - Existing session tests still pass
  - No new clippy warnings in session.rs
  - Manual verification: run cleanup while hammering get_session, no errors
```

## Planning/Execution Boundary

**Planning and execution are separate phases with different agents.**

The agent that plans beads (creates the epic, designs the DAG, upgrades bead quality) is NOT the agent that executes beads. This separation is critical:

| Phase | Agent | Tools | Ends With |
|-------|-------|-------|-----------|
| **Planning** | Conversational agent | `/beads`, `bd create`, `bd update` | Handoff message |
| **Execution** | `beads-harness` | `bd claim`, file operations, `bd close` | Epic completion |

### Why This Separation Exists

1. **Context isolation** - The planning agent has conversational context (user intent, research findings, design decisions). The execution agent has only the bead description. This forces beads to be self-contained.

2. **Scalability** - Execution can be parallelized across multiple agents. Planning is inherently sequential and conversational.

3. **Auditability** - The planning conversation is preserved. Execution is logged separately. Mixing them creates confusion about what was decided vs. what was done.

4. **Recovery** - If execution fails, the plan remains intact. A new execution agent can pick up where the previous one failed.

### The Handoff Protocol

Planning ends with a handoff message:

```
Planning complete.

Epic: <epic-id>
Branch: <branch-name>
Work Type: <type>
Beads: <count> tasks ready for execution

To execute:
  beads-harness <epic-id>
```

The planning agent MUST NOT:
- Claim beads (`bd update --claim`)
- Create or modify files that beads would create
- Run validation commands that beads would run
- Close beads (`bd close`)

These actions belong to the execution phase.

### Anti-Pattern: Planning Agent Executing

**Violation:**
```
Planning agent: "Let me now execute the first bead..."
[Claims bead, creates files, closes bead]
```

**Problem:** The planning agent's context window is exhausted. Research findings, design rationale, and conversation history are lost. The "execution" happens without the benefits of the harness (parallelization, context management, progress tracking).

**Correct behavior:**
```
Planning agent: "Planning complete. To execute: beads-harness <epic-id>"
[Stops. Does not execute.]
```

### Exception: Explicit User Request

The planning agent MAY execute beads only if the user explicitly requests it:

```
User: "Execute the first bead now, I want to watch"
Planning agent: [May proceed with execution]
```

Without explicit request, always hand off to the harness.

## Related Documents

- `/beads` workflow skill - Consumes these criteria in validation phase
- `docs/workflow-decision-guide.md` - When to create beads
- `skills/beads-plan/SKILL.md` - Planning methodology
- `beads-harness` - Execution agent for beads
