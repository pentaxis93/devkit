# Post-Mortem: Pipeline Coherence Audit

**Date:** 2026-01-30
**Scope:** Full pipeline chain from vision-workshop to epic-validation
**Trigger:** First use of coherence-audit skill on the pipeline itself

## Timeline

1. Phase A+B imported and adapted 14 skills from beadsmith, removing
   org-mode references and creating pipeline artifact templates
2. Phase B planning doc defined "connect pipeline stages" as a goal
3. Connection work focused on beads-plan input/output (the hardest
   handoff) and template traceability (frontmatter references)
4. Coherence audit revealed 10 discrepancy classes across 7 skills

## Discrepancies Found

| ID | Description | Severity |
|----|-------------|----------|
| D1 | journey-mapping didn't name feature-discovery as upstream skill | Medium |
| D2 | beads-execute didn't reference validate-pipeline or epic-validation | Medium |
| D3 | No forward link from validate-pipeline to epic-validation | Low |
| D4 | epic-validation completion callback was a single undeveloped sentence | High |
| D5 | vision-template had no downstream reference | Low |
| D6 | 5 of 8 pipeline skills lacked Related Skills tables | Medium |
| D7 | validate-pipeline referenced 2 nonexistent files | High |
| D8 | README pipeline status claims were imprecise | Low |
| D9 | vision-workshop and feature-discovery still had TODO sections | Low |
| D10 | journey-mapping at 548 lines (over 500 target) | Low |

## Root Causes

**Primary:** Phase B focused on removal (org-mode grep) rather than
connection. The grep-based approach found all org-mode references
exhaustively but had no equivalent systematic check for bidirectional
cross-referencing between skills.

**Secondary:** Original beadsmith skills had inconsistent cross-
referencing — only beads-plan and epic-validation had Related Skills
tables. This inconsistency was inherited, not introduced.

**Tertiary:** validate-pipeline referenced files (`../docs/validation-
interface.md`, `../commands/beads-loop.md`) that existed in beadsmith
but were never imported. No broken-link audit was performed during import.

## Fixes Applied

### Object-Level (Phase 4)
- Added explicit upstream skill names to journey-mapping
- Added Related Skills tables to 5 skills (vision-workshop,
  feature-discovery, journey-mapping, beads-execute, validate-pipeline)
- Replaced broken references in validate-pipeline with Related Skills
- Added downstream reference to vision-template
- Fleshed out epic-validation completion callback with procedure
- Converted epic-validation References section to Related Skills table
- Tightened README pipeline status claims
- Deleted stale TODO sections from vision-workshop and feature-discovery

### Process-Level (Phase 5)
- Added Pipeline Coherence Checklist to README — 8 verification items
  that must be checked when adding or editing any pipeline skill

## Lessons Learned

1. **Removal is not connection.** Deleting broken references does not
   create working cross-references. The two are independent tasks.

2. **Grep finds what's there; checklists find what's missing.** Org-mode
   removal succeeded because grep is exhaustive for presence checks.
   Cross-referencing failed because absence checks require a checklist
   of what *should* exist.

3. **Inherited inconsistency compounds.** When source material is
   inconsistent (some skills have Related Skills, some don't), the
   inconsistency looks intentional during import. An explicit standard
   ("every pipeline skill needs a Related Skills table") prevents this.

4. **Coherence audits should run after major edits.** Phase B edited
   every pipeline skill. A coherence audit at the end would have caught
   all 10 discrepancies in the same session. The process fix (checklist)
   makes this cheaper for incremental changes; the full audit is for
   batch operations.

## Accepted Residual

- journey-mapping at 557 lines — the Epic Definition phase is core
  methodology and cannot be extracted without fragmenting the skill's
  coherence. Accepted as over target.
- beads-plan at 904 lines — already extracted 3 reference sections;
  remaining content is core methodology. Accepted as over target.
