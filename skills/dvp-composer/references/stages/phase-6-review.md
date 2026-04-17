# Phase 6: Internal Review

## Goal

Perform comprehensive internal review of the DVP to identify omissions, inconsistencies, and quality issues before finalization.

## Read Previous Phase

Before starting Phase 6 work, read these files from `dvp_workspace/`:

- `dvp_content.json` (Phase 5) — The DVP content to review
- `checks-final.md` (Phase 4) — The authoritative check list (for cross-referencing)
- `scope.md` (Phase 2) — Scope definition for completeness check
- `key-data.md` (Phase 2) — Key data for coverage verification
- `risk-assessment.md` (Phase 2) — Risk areas for coverage verification
- `assumptions-and-gaps.md` — Assumptions for review

## Deliverables

Before the [Done] step, write the following files to `dvp_workspace/`:

| File | Content |
|------|---------|
| `review-report.md` | Review findings organized by severity (Must Fix / Should Fix / Nice to Have), with handling results for each finding |

If corrections were made during the review:
- Update `dvp_content.json` with the corrected content
- Re-run `scripts/generate_xlsx.py` to regenerate the Excel
- Update `checks-final.md` if any checks were added, removed, or modified

## Interaction Guide

Follow the Interaction Protocol defined in `SKILL.md`. This phase primarily uses **[Conflict]**, **[Confirm]**, and **[Done]** question types.

| Decision point | Level | Notes |
|----------------|-------|-------|
| Must-fix item changes | Must-ask | Must get approval before modifying |
| Should-fix item adoption | Recommend | Suggest fix, user confirms |
| Nice-to-have improvements | Self-decide | Default: no change unless user requests |
| Numbering/ID fixes | Self-decide | Fix automatically |

## Review Dimensions

### 1. Completeness Check

- [ ] All modules identified in Phase 2 are covered in the check list
- [ ] All key variables have associated checks
- [ ] All risk points from Phase 2 are addressed
- [ ] Cross-module consistency checks are included
- [ ] External data reconciliation checks are included

### 2. Logic Consistency

- [ ] No duplicate check rules (same logic under different Check IDs)
- [ ] No contradictory checks (two checks that cannot both pass)
- [ ] No overlap with existing edit checks (unless intentional)
- [ ] Date/time logic is consistent across modules
- [ ] Severity grading logic is consistent

### 3. Expression Quality

- [ ] Check descriptions are clear and unambiguous
- [ ] Logic rules are precise and testable
- [ ] Query wording is site-friendly (clear, specific, actionable)
- [ ] No undefined terms or abbreviations

### 4. Numbering Standards

- [ ] Check IDs follow naming convention (MODULE-NNN)
- [ ] No gaps in sequential numbering within modules
- [ ] No duplicate IDs
- [ ] Module prefixes are consistent

### 5. Risk Coverage

- [ ] Safety-critical data has sufficient checks
- [ ] Primary endpoint data has thorough validation
- [ ] SAE/Death reporting requirements are covered
- [ ] Inclusion/exclusion violations can be detected

## Sub-Tasks

At the start of this phase, create the following sub-tasks. Each should `addBlockedBy` the Phase 6 task ID. Mark each `completed` when its step finishes.

| # | subject | description |
|---|---------|-------------|
| 1 | Automated Review | Run through all review dimensions systematically |
| 2 | Present Findings | Present Must Fix, Should Fix, and Nice to Have items |
| 3 | Address Issues | Apply approved changes and verify no regressions |
| 4 | Final Verification | Re-verify IDs, counts, and regenerate Excel if needed |
| 5 | Present Final DVP | Present finalized DVP with review completion summary |

## Steps

### Step 1: Automated Review

**[Self-decide]** Run through the review checklist above systematically. For each dimension:
- Flag specific issues with Check IDs
- Categorize by severity (Must Fix, Should Fix, Nice to Have)
- Provide specific recommendations

### Step 2: Present Findings

**[Conflict]** For Must Fix items, present one by one for approval:
```
[Conflict] [Check ID] [issue — e.g., Duplicate logic]
  Finding: [AE-008 and AE-012 share the same logic]
  Suggested resolution: Merge into one check — [keep AE-008, remove AE-012]
  Please confirm: adopt suggestion / other approach
```

**[Confirm]** For Should Fix items, batch into one recommendation:
```
[Confirm] Should Fix items (recommended changes)
  The following issues are recommended for fixing but do not affect core correctness:
  1. [CM-003] Query wording is too generic, recommend changing to "[specific wording]"
  2. [VS-005] Visit window definition conflicts with VS-009, recommend unifying to [...]
  ...
  Please confirm: adopt all / adopt selected (please specify) / skip for now
```

Nice to Have items are listed but not actioned by default.

### Step 3: Address Issues

**[Self-decide]** For each approved change:
1. Apply the change
2. Verify the change doesn't introduce new issues

### Step 4: Final Verification

**[Self-decide]** After all issues are resolved:
1. Re-verify Check ID sequence
2. Confirm total check count
3. If any corrections were made:
   a. Update `dvp_content.json` in `dvp_workspace/`
   b. Re-run `scripts/generate_xlsx.py` to regenerate the Excel
   c. Update `checks-final.md` if checks changed

### Step 5: Present Final DVP

Before presenting the summary, write the `review-report.md` deliverable to `dvp_workspace/`. If corrections were made, also update `dvp_content.json` and regenerate the Excel.

**[Done]** Present the finalized DVP:
- Final check count summary
- Review completion confirmation
- Updated Excel file path
- List of changes made during review

```
[Done] Phase 6: Internal Review
  Deliverables written to dvp_workspace/:
  - review-report.md — Review findings and handling results
  - dvp_content.json — [Updated / No changes]
  - DVP_<ProtocolNumber>_v1.0.xlsx — [Regenerated / No changes]

  Output summary:
  - Final check rule count: [N]
  - Review results: [N] Must Fix corrected / [N] Should Fix adopted / [N] Nice to Have unchanged
  - Output file: dvp_workspace/DVP_<ProtocolNumber>_v1.0.xlsx
  - Review complete — DVP is ready for distribution and team review
```

The DVP is now ready for distribution and team review.

**Task update**: Mark Phase 6 task as `completed`. All tasks are now done.

## Tips

- Review should be thorough but practical — not every check needs to be perfect on first draft.
- Prioritize must-fix issues (duplicates, contradictions, missing safety checks).
- Document all review findings for audit trail purposes.
- If the user wants to skip certain review items, document the decision and rationale.
