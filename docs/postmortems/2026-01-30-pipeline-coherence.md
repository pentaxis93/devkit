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

## Second Audit (same date, after Phase B completion)

After completing Phase B and applying the first audit's fixes, a second
coherence audit checked the README's pipeline coherence checklist against
every pipeline skill. Found 8 additional discrepancies:

| ID | Description | Severity |
|----|-------------|----------|
| D1 | vision-workshop had no pipeline root declaration | Medium |
| D2 | vision-workshop quality checklist didn't mention next stage | Medium |
| D3 | vision-template frontmatter had no `downstream` field | Low |
| D4 | features-template had no downstream consumer reference | Low |
| D5 | beads-plan quality checklist didn't mention next stage | Medium |
| D6 | beads-execute had no Pipeline Position or completion convention reference | Medium |
| D7 | epic-validation quality checklist didn't mention completion callback | Medium |
| D8 | README claimed "each skill names its predecessor" — vision-workshop didn't | Low |

**Root cause:** Same mechanism as first audit. The README coherence
checklist existed but was never executed as a validation gate on the
Phase B work. Skills that received heavy rewrites (beads-plan,
journey-mapping) were coherent; lighter-touch skills had gaps.

**Fixes applied:**
- Added Pipeline Position section to vision-workshop and beads-execute
- Added next-stage mentions to quality checklists in vision-workshop,
  beads-plan, and epic-validation
- Added `downstream` frontmatter field to all 3 pipeline templates
- Updated README claim to note vision-workshop as pipeline root
- Made coherence checklist imperative: "Run this checklist after every
  pipeline skill edit"

**Lesson:** Checklists must be executed, not merely documented. Changed
the checklist header from passive to imperative.

## Third Audit (same date, after coherence-audit skill import)

After importing the `coherence-audit` skill and completing Phase B,
a third audit ran the full README pipeline coherence checklist (48
checks across Matrices A-D) against all 8 pipeline skills.

**Result: 47 PASS, 1 DISCREPANCY.**

| ID | Description | Severity |
|----|-------------|----------|
| D1 | beads-execute had no Quality Checklist section | Low |

All handoff contracts (Matrix B: 6/6), traceability chain (Matrix C:
4/4), and completion callback (Matrix D: 3/3) verified correct.

**Root cause:** Same as prior audits — the README coherence checklist
existed but wasn't executed after Phase B edits. beads-execute was the
only pipeline skill without a Quality Checklist section. The
information existed in its Pipeline Position section (line 38: "invoke
epic-validation for the completion gate") but not in a formal checklist
as the README pattern requires.

**Fix:** Added Quality Checklist section to beads-execute with 7 items
including "When epic reaches 100%: invoke epic-validation."

**Process assessment:** No new process fix needed. The existing
checklist caught this. The pattern from audits 1 and 2 — "checklists
must be executed, not just documented" — held. This audit validates
that the pipeline is now coherent: every stage's output matches the
next stage's input, every artifact references its predecessor, and
the full chain from vision to validated epic is unbroken.

## Fourth Audit (after decision-audit and fixture-validate import)

Expanded scope to all 11 pipeline skills (core + supporting). Ran 88
checks (8 per skill) plus 10 handoff contracts and new-import
verification.

**Result: 78 PASS, 4 DISCREPANCY, 6 N/A. All 10 handoff contracts PASS.**

| ID | Description | Severity |
|----|-------------|----------|
| D1 | validate-pipeline quality checklist missing next-stage mention | Low |
| D2 | decision-audit quality checklist missing downstream mention | Low |
| D3 | fixture-validate quality checklist missing next-stage mention | Low |
| D4 | iterative-planning-context has 4 broken cross-references | Medium |

**Additional finding:** 7 skills had vestigial `skill-compiler/1.0.0`
banners from beadsmith's build system. Not functional but claims a
tool that doesn't exist in this project.

**Root causes:**
1. D1-D3: Prior audits only checked core pipeline skills for checklist
   item 6 (quality checklist mentions next stage). Supporting skills
   were out of scope. The README says "pipeline skill" without
   distinguishing core vs supporting.
2. D4: Phase B used grep for org-mode removal (presence check), not
   link validation (existence check). Broken links to non-imported
   beadsmith docs were inherited undetected.
3. Banners: Phase A added frontmatter but didn't remove old version/
   compiler banners.

**Fixes applied:**
- Added next-stage mentions to quality checklists in validate-pipeline,
  decision-audit, and fixture-validate
- Removed broken references from iterative-planning-context (1 skill
  ref to non-imported `codebase-exploration`, 3 doc refs to non-imported
  beadsmith docs)
- Removed skill-compiler banners from all 7 skills
- Updated README coherence checklist to explicitly state scope: "Applies
  to ALL pipeline skills (core, supporting, and BDD)"

**Lesson:** Audit scope must match checklist scope. When the checklist
says "pipeline skill," it means every skill in the pipeline tables —
not just the 5 core stages.

## Accepted Residual

- journey-mapping at 557 lines — the Epic Definition phase is core
  methodology and cannot be extracted without fragmenting the skill's
  coherence. Accepted as over target.
- beads-plan at 905 lines — already extracted 3 reference sections;
  remaining content is core methodology. Accepted as over target.
