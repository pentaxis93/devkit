# Persuasion Principles for Skill Design

Understanding why certain language patterns increase agent compliance
helps design more effective discipline skills -- not to manipulate, but
to ensure critical practices are followed even under pressure.

**Research foundation:** Meincke et al. (2025) tested 7 persuasion
principles with N=28,000 AI conversations. Persuasion techniques more
than doubled compliance rates (33% → 72%, p < .001).

## The Seven Principles

### 1. Authority

Deference to expertise, credentials, or official sources.

**In skills:** Imperative language ("YOU MUST", "Never", "Always"),
non-negotiable framing ("No exceptions"). Eliminates decision fatigue
and rationalization.

**Use for:** Discipline skills, safety-critical practices, established
best practices.

```markdown
# GOOD
Write code before test? Delete it. Start over. No exceptions.

# BAD
Consider writing tests first when feasible.
```

### 2. Commitment

Consistency with prior actions, statements, or declarations.

**In skills:** Require announcements ("Announce skill usage"), force
explicit choices ("Choose A, B, or C"), use tracking (todo lists for
checklists).

**Use for:** Multi-step processes, accountability mechanisms.

```markdown
# GOOD
When you find a skill, you MUST announce: "I'm using [Skill Name]"

# BAD
Consider letting your partner know which skill you're using.
```

### 3. Scarcity

Urgency from time limits or limited availability.

**In skills:** Time-bound requirements ("Before proceeding"), sequential
dependencies ("Immediately after X"). Prevents procrastination.

**Use for:** Immediate verification, time-sensitive workflows.

```markdown
# GOOD
After completing a task, IMMEDIATELY request code review before proceeding.

# BAD
You can review code when convenient.
```

### 4. Social Proof

Conformity to what others do or what's considered normal.

**In skills:** Universal patterns ("Every time", "Always"), failure modes
("X without Y = failure"). Establishes norms.

**Use for:** Documenting universal practices, warning about common failures.

```markdown
# GOOD
Checklists without tracking = steps get skipped. Every time.

# BAD
Some people find tracking helpful for checklists.
```

### 5. Unity

Shared identity, "we-ness", in-group belonging.

**In skills:** Collaborative language ("our codebase", "we're colleagues"),
shared goals ("we both want quality").

**Use for:** Collaborative workflows, establishing team culture.

```markdown
# GOOD
We're colleagues working together. I need your honest technical judgment.

# BAD
You should probably tell me if I'm wrong.
```

### 6. Reciprocity

Obligation to return benefits received.

Use sparingly -- can feel manipulative. Rarely needed in skills.

### 7. Liking

Preference for cooperating with those we like.

**Do not use for compliance.** Conflicts with honest feedback culture.
Creates sycophancy.

## Principle Combinations by Skill Type

| Skill Type | Use | Avoid |
|------------|-----|-------|
| Discipline | Authority + Commitment + Social Proof | Liking, Reciprocity |
| Technique | Moderate Authority + Unity | Heavy authority |
| Collaborative | Unity + Commitment | Authority, Liking |
| Reference | Clarity only | All persuasion |

## Why This Works

**Bright-line rules reduce rationalization:**
- "YOU MUST" removes decision fatigue
- Absolute language eliminates "is this an exception?" questions
- Explicit anti-rationalization counters close specific loopholes

**Implementation intentions create automatic behavior:**
- Clear triggers + required actions = automatic execution
- "When X, do Y" is more effective than "generally do Y"
- Reduces cognitive load on compliance

**LLMs are parahuman:**
- Trained on human text containing these patterns
- Authority language precedes compliance in training data
- Commitment sequences (statement → action) are frequently modeled
- Social proof patterns (everyone does X) establish norms

## Ethical Use

**Legitimate:** Ensuring critical practices are followed, creating
effective documentation, preventing predictable failures.

**Illegitimate:** Manipulating for personal gain, creating false
urgency, guilt-based compliance.

**The test:** Would this technique serve the user's genuine interests
if they fully understood it?

## Research Citations

**Cialdini, R. B. (2021).** *Influence: The Psychology of Persuasion
(New and Expanded).* Harper Business.
- Seven principles of persuasion
- Empirical foundation for influence research

**Meincke, L., Shapiro, D., Duckworth, A. L., Mollick, E., Mollick, L.,
& Cialdini, R. (2025).** Call Me A Jerk: Persuading AI to Comply with
Objectionable Requests. University of Pennsylvania.
- Tested 7 principles with N=28,000 LLM conversations
- Compliance increased 33% → 72% with persuasion techniques
- Authority, commitment, scarcity most effective
- Validates parahuman model of LLM behavior

## Attribution

Adapted from
[obra/superpowers-skills/writing-skills/persuasion-principles.md](https://github.com/obra/superpowers-skills/tree/main/skills/meta/writing-skills),
generalized for provider-agnostic use.
