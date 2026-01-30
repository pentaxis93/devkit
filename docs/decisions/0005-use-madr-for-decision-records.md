# Use MADR Minimal Format for Decision Records

## Context and Problem Statement

During skill review, we made decisions that need rationale preserved
(naming conventions, pipeline design, tool choices). Where should these
decisions live, and in what format? The decision needs to be lightweight
enough to actually get written, but structured enough to be useful
months later.

## Considered Options

* No formal records (rationale lives in commit messages and comments)
* Y-statements in a single decision log file
* Full MADR (Markdown Any Decision Records) with all optional sections
* MADR minimal template (5 sections only)

## Decision Outcome

Chosen option: "MADR minimal template," because it provides enough
structure for future readers without overhead that discourages writing.

MADR (originally "Markdown Architectural Decision Records," renamed to
"Markdown Any Decision Records" in v3.0.0) is the most widely adopted
decision record format in open source. The minimal template has 5
sections: Title, Context and Problem Statement, Considered Options,
Decision Outcome, Consequences.

### Format

Records live in `docs/decisions/NNNN-title-with-dashes.md`. Numbers are
sequential, never reused. Superseded decisions stay but are marked in a
Status section.

The MADR minimal template:

```markdown
# Title of Decision

## Context and Problem Statement
{What's the situation}

## Considered Options
* Option A
* Option B

## Decision Outcome
Chosen option: "A", because {reason}.

### Consequences
* Good, because ...
* Bad, because ...
```

### Scope

Record any decision that meets at least one of:
- Involved evaluating alternatives
- Affects how future contributors work
- Would be asked about in a code review ("why did you...?")

Do not gate on "architecture" — the community consensus (MADR v3,
joelparkerhenderson's canonical repo, Zimmermann 2021) is that any
important decision qualifies.

### Alternatives not chosen

**Y-statements** (Zimmermann, SATURN 2012): Single-sentence format
("In the context of X, facing Y, we decided Z, accepting W"). Elegant
but hard to scan and doesn't accommodate sub-decisions or detailed
rationale. MADR was explicitly derived from Y-statements but broke the
single sentence into sections for readability.

**No formal records**: Commit messages capture what was done but not
what was considered and rejected. The "why not" is often more valuable
than the "why."

### Consequences

* Good, because rationale is preserved alongside the codebase
* Good, because the minimal template is fast to write (~10 minutes)
* Good, because sequential numbering creates a natural project history
* Good, because MADR is a recognized standard with tooling support
* Neutral, because this adds a step to the decision process (mitigated
  by the template being minimal)
* Bad, because judgment is required on what qualifies as "important
  enough" to record (mitigated by the scope criteria above)
