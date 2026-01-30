# Use Managed BDD Fixtures for Test Data

## Context and Problem Statement

The project uses BDD scenarios (Gherkin) produced by journey-mapping and
implemented by the 4 BDD skills. Step definitions need test data. The
question is whether to use inline test data in step definitions or a
managed fixture registry.

## Considered Options

* Inline test data in step definitions (ad-hoc)
* Managed fixture registry with validation (structured)

## Decision Outcome

Chosen option: "Managed fixture registry with validation."

Fixtures are an upfront investment that pays off with consistent,
predictable testing. The `fixture-validate` skill enforces registry
integrity as a hybrid gate in the validation pipeline.

### Rationale

**Standardization keeps agents aligned.** With agentic coding, multiple
agents work on the same codebase across sessions. Inline test data
drifts — each agent invents its own test users, emails, and
configurations. A fixture registry provides a single source of truth
for test data that all agents reference.

**Consistency compounds.** When every step definition references the
same `users/admin.fixture.yaml`, test behavior is predictable. When
test data is inline, the same logical test can produce different
results depending on which agent wrote it.

**Validation catches drift.** The `fixture-validate` skill detects
missing references, orphan fixtures, circular dependencies, and
hardcoded data in step definitions. Without managed fixtures, these
problems are invisible until tests fail in production.

**Low ongoing cost.** Creating a fixture takes minutes. The validation
runs automatically as part of `validate-pipeline`. The investment is
front-loaded; the maintenance cost is near zero.

### Consequences

* Good, because test data is centralized, consistent, and versioned
* Good, because fixture validation catches integrity issues early
* Good, because agents reference fixtures by name instead of inventing
  test data
* Neutral, because fixtures must be created before step definitions
  that use them (ordered dependency)
* Bad, because initial fixture setup adds time to the first BDD cycle
