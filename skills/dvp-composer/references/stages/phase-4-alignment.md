# Phase 4: Alignment

## Goal

Confirm that all designed check rules are correct, feasible, and do not conflict with existing systems or analysis plans. Resolve any issues before drafting.

## Interaction Guide

Follow the Interaction Protocol defined in `SKILL.md`. This phase primarily uses **[Conflict]**, **[Confirm]**, and **[Done]** question types.

| Decision point | Level | Notes |
|----------------|-------|-------|
| Overlapping check resolution (keep/merge/remove) | Must-ask | Affects DVP structure |
| Feasibility issue trade-offs | Must-ask | Affects whether checks can execute |
| Contradictory checks | Must-ask | Must be resolved before drafting |
| Query burden assessment | Recommend | Suggest consolidation, user confirms |
| Low-priority rule consolidation | Recommend | Suggest removal, user confirms |
| Numbering corrections | Self-decide | Fix automatically |
| Wording refinements | Self-decide | Improve automatically |

## Steps

### Step 1: Protocol Alignment Review

**[Self-decide]** For each check rule, verify:
- Logic aligns with protocol requirements
- Triggers match protocol-specified events
- Scope matches the study population and visit structure
- No checks contradict protocol definitions

**[Must-ask]** If any checks cannot be verified from the protocol, flag for confirmation:
```
[Conflict] [Check ID] Protocol alignment issue
  Finding: [This check's logic depends on protocol requirement X, but the protocol text is ambiguous]
  Suggested resolution: [Interpret as Y / Remove this check]
  Please confirm: adopt suggestion / provide the correct interpretation
```

### Step 2: SAP/Analysis Alignment

**[Self-decide]** If SAP is available:
- Verify that checks support (not conflict with) analysis needs
- Confirm that endpoint-related checks match SAP definitions
- Check that derived variables are validated consistently with analysis derivation rules

### Step 3: Edit Check Conflict Check

**[Must-ask]** Compare DVP checks against known edit checks. For each conflict found:
```
[Conflict] [Check ID] Overlap with existing Edit Check
  Finding: [DVP check logic duplicates Edit Check #EC-XXX]
  Suggested resolution: Remove — [this logic is already covered by the edit check]
  Alternative: Keep as a supplementary check
  Please confirm: adopt suggestion / keep / merge
```

Batch all conflict items into one prompt.

### Step 4: Database Feasibility

**[Self-decide]** For each check, assess:
- Can the required fields be queried from the database?
- Are the referenced variables available and correctly named?
- Is the execution method feasible (SAS, listing, manual)?

**[Must-ask]** If feasibility issues are found:
```
[Conflict] [Check ID] Database feasibility issue
  Finding: [Required field Y does not exist in the database / execution method is not feasible]
  Suggested resolution: [Change execution method / Remove this check / Adjust logic]
  Please confirm: adopt suggestion / other approach
```

### Step 5: Query Burden Assessment

**[Recommend]** Evaluate:
- Are there checks that would generate excessive queries?
- Can any checks be consolidated to reduce query volume?
- Are query wordings clear enough to avoid back-and-forth with sites?

```
[Confirm] Query burden assessment
  Recommendation: Merge [Check A] and [Check B] to reduce query volume
  Rationale: Both checks share similar logic; merging preserves coverage while reducing ~[N] queries
  Alternative: Keep them separate
  Please confirm whether to adopt the recommendation.
```

### Step 6: Present Alignment Results

**[Done]** Output a structured alignment report:

```
[Done] Phase 4: Alignment
  Output summary:
  - Protocol alignment: [N] confirmed / [N] corrected
  - Edit Check conflicts: [N] overlaps resolved ([N] removed / [N] merged / [N] kept)
  - Feasibility issues: [N] resolved
  - Query burden: [N] optimizations applied
  - Final check rule count: [N]

  Next: Phase 5: Draft
  Will proceed after your confirmation. Let me know if adjustments are needed.
```

Wait for user confirmation before proceeding to Phase 5.

## Tips

- This phase may require multiple rounds of discussion with the user.
- For protocol alignment, reference specific protocol sections when flagging items.
- Be practical: if a check is nice-to-have but causes excessive queries, recommend removing it.
- Document all decisions and rationale for traceability.
