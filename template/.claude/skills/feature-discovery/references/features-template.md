# Features Template

Use this template when creating `planning/features.md`.
The next pipeline stage is `journey-mapping`, which maps user journeys
for each feature discovered here.

The file contains a header with vision reference, followed by one
feature entry per feature discovered. Features are added iteratively
during the feature-discovery dialogue.

```markdown
---
status: draft
vision: planning/vision.md
downstream: journey-mapping
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# Features: [Product Name]

## Feature: [Feature Name]

**Status:** draft | ready | deferred
**Vision reference:** [Which success criterion or capability this serves]

### Opportunity

**Job story:** When [situation], I want [motivation], so I can [outcome].

**Evidence:** [interview | analytics | support-ticket | observation]
[Brief description of the evidence]

### Success Criteria

[Measurable, observable, time-bound outcome.]

Good: "80% of users complete first deployment within 10 minutes"
Bad: "Users find deployment easy"

### Scope

**In scope:**
1. [Capability this feature WILL deliver]
2. [Capability]
3. [Capability]

**Out of scope:**
1. [What this feature will NOT do] — [Why excluded]
2. [What this feature will NOT do] — [Why excluded]

### Journeys

[Left empty during feature-discovery. Populated by journey-mapping
as journeys are created that implement this feature.]

- planning/journeys/[slug].md

### Assumptions

[Riskiest beliefs that must be true for this feature to succeed.]

1. **Assumption:** [Statement]
   **Test:** [How to validate]
   **Risk if wrong:** [Impact]

---

[Repeat ## Feature: sections for each feature]
```

## Validation Criteria

Before marking features as complete:

- [ ] Each feature has a job story (When..., I want..., So I can...)
- [ ] Each feature has at least one piece of evidence
- [ ] Success criteria is measurable and time-bound
- [ ] In-scope capabilities listed (3-5 items)
- [ ] Out-of-scope exclusions explicit (2+ items)
- [ ] Riskiest assumption identified with a test
- [ ] Feature traces back to vision success criteria or capability
- [ ] Status and dates populated
- [ ] Feature set reviewed for coherence with vision
- [ ] No `[REPLACE]` or `[TODO]` placeholders remaining
