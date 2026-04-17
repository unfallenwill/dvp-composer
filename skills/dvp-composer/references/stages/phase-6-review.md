# Phase 6: Internal Review

## Goal

Perform comprehensive internal review of the DVP to identify omissions, inconsistencies, and quality issues before finalization.

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
3. Ensure Excel output reflects all changes
4. Regenerate Excel if needed

### Step 5: Present Final DVP

**[Done]** Present the finalized DVP:
- Final check count summary
- Review completion confirmation
- Updated Excel file path
- List of changes made during review

```
[Done] Phase 6: Internal Review
  Output summary:
  - Final check rule count: [N]
  - Review results: [N] Must Fix corrected / [N] Should Fix adopted / [N] Nice to Have unchanged
  - Output file: [file path]
  - Review complete — DVP is ready for distribution and team review

  Assumptions made: [if any]
```

The DVP is now ready for distribution and team review.

## Tips

- Review should be thorough but practical — not every check needs to be perfect on first draft.
- Prioritize must-fix issues (duplicates, contradictions, missing safety checks).
- Document all review findings for audit trail purposes.
- If the user wants to skip certain review items, document the decision and rationale.
