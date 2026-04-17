# Phase 4: Alignment

## Goal

Confirm that all designed check rules are correct, feasible, and do not conflict with existing systems or analysis plans. Resolve any issues before drafting.

## Steps

### Step 1: Protocol Alignment Review

For each check rule, verify:
- Logic aligns with protocol requirements
- Triggers match protocol-specified events
- Scope matches the study population and visit structure
- No checks contradict protocol definitions

Present findings as a list of items requiring user confirmation:

```
Items to confirm:
1. [AE-015] Is the SAE determination logic consistent with Protocol Section X.X?
2. [VS-008] Does the visit window calculation use the Protocol-defined ±3 days?
...
```

### Step 2: SAP/Analysis Alignment

If SAP is available:
- Verify that checks support (not conflict with) analysis needs
- Confirm that endpoint-related checks match SAP definitions
- Check that derived variables are validated consistently with analysis derivation rules

### Step 3: Edit Check Conflict Check

Compare DVP checks against known edit checks:
- Identify overlaps (same logic already in edit check — remove from DVP or note as supplement)
- Identify contradictions (DVP check conflicts with edit check)
- Identify gaps (edit check covers it, but DVP provides additional context)

### Step 4: Database Feasibility

For each check, assess:
- Can the required fields be queried from the database?
- Are the referenced variables available and correctly named?
- Is the execution method feasible (SAS, listing, manual)?

### Step 5: Query Burden Assessment

Evaluate:
- Are there checks that would generate excessive queries?
- Can any checks be consolidated to reduce query volume?
- Are query wordings clear enough to avoid back-and-forth with sites?

### Step 6: Present Alignment Results

Output a structured alignment report:

```
Alignment Report:
- Protocol alignment: X items need confirmation (list)
- SAP conflicts: X items (list)
- Edit Check overlaps: X items (list with recommended action)
- Database feasibility issues: X items (list)
- Query burden risks: X items (list)
- Summary: X total check rules, Y need adjustment
```

Ask the user to resolve flagged items. Iterate until all issues are addressed.

Ask the user: "Alignment is complete and all flagged items have been addressed. Please confirm, and I will proceed to the next phase: Draft."

Wait for user confirmation before proceeding to Phase 5.

## Tips

- This phase may require multiple rounds of discussion with the user.
- For protocol alignment, reference specific protocol sections when flagging items.
- Be practical: if a check is nice-to-have but causes excessive queries, recommend removing it.
- Document all decisions and rationale for traceability.
