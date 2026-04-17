# Phase 6: Internal Review

## Goal

Perform comprehensive internal review of the DVP to identify omissions, inconsistencies, and quality issues before finalization.

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

Run through the review checklist above systematically. For each dimension:
- Flag specific issues with Check IDs
- Categorize by severity (Must Fix, Should Fix, Nice to Have)
- Provide specific recommendations

### Step 2: Present Findings

Output a structured review report:

```
Internal Review Report:

Completeness:
  - [Missing] AE module lacks SAE determination time window check
  - [Missing] Lab module lacks abnormal value trending check

Logical Consistency:
  - [Duplicate] AE-008 and AE-012 share the same logic, recommend merging
  - [Conflict] VS-005 and VS-009 contradict on visit window definition

Expression Quality:
  - [Vague] CM-003 Query wording "please verify" is too generic, recommend changing to "please verify if the medication start date is before the AE start date"

Numbering Standards:
  - [Gap] IE module: IE-003 jumps directly to IE-006

Risk Coverage:
  - [Suggestion] Consider increasing check density for primary endpoint data

Summary: 3 must-fix items, 2 should-fix items, 1 optional improvement
```

### Step 3: Address Issues

For each flagged issue:
1. Present the specific change recommendation
2. Get user approval
3. Apply the change
4. Verify the change doesn't introduce new issues

### Step 4: Final Verification

After all issues are resolved:
1. Re-verify Check ID sequence
2. Confirm total check count
3. Ensure Excel output reflects all changes
4. Regenerate Excel if needed

### Step 5: Present Final DVP

Present the finalized DVP:
- Final check count summary
- Review completion confirmation
- Updated Excel file path
- List of changes made during review

The DVP is now ready for distribution and team review.

## Tips

- Review should be thorough but practical — not every check needs to be perfect on first draft.
- Prioritize must-fix issues (duplicates, contradictions, missing safety checks).
- Document all review findings for audit trail purposes.
- If the user wants to skip certain review items, document the decision and rationale.
