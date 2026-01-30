# Blend Skill-Creation Guidance from Multiple Sources

## Context and Problem Statement

We need a guide for creating new skills. Two strong open-source
references exist: Anthropic's `skill-creator` (information architecture,
progressive disclosure, degrees of freedom) and obra's `writing-skills`
+ `testing-skills-with-subagents` (BDD methodology, agent search
optimization, behavioral hardening). Neither alone covers the full
picture. Should we use one as-is, or synthesize?

## Considered Options

* Use Anthropic's `skill-creator` as-is
* Use obra's skill-writing guides as-is
* Blend both into a single comprehensive guide

## Decision Outcome

Chosen option: "Blend both into a single comprehensive guide"
(`skill-creator`), because each source covers complementary concerns.
Anthropic provides the structural framework (how to organize a skill);
obra provides the quality methodology (how to verify a skill works).

Key sub-decisions made during the blend:

**BDD over TDD for skill testing.** Skills are behavioral specifications
— they define what an agent should do in context. BDD's Given/When/Then
maps naturally to skill triggers and expected agent behavior. TDD's
unit-test mindset doesn't fit because skills aren't functions with
inputs and outputs.

**Provider-agnostic naming.** All terminology uses neutral language:
"agent" not "Claude," "ASO" (Agent Search Optimization) not "CSO"
(Claude Search Optimization), "sub-agent" not "Claude Code Task tool."
This keeps skills portable across tools (Claude Code, OpenCode, future
tools) and avoids coupling documentation to a vendor.

### Consequences

* Good, because the blended guide covers structure, testing, and
  discovery — the full skill lifecycle
* Good, because BDD methodology provides concrete verification criteria
* Good, because provider-agnostic naming enables cross-tool compatibility
* Bad, because the blended skill cannot be updated from either upstream
  automatically (status: Blended, maintained independently)
* Neutral, because both sources are MIT-licensed, so blending is
  legally straightforward
