# Phase 5: Draft

## Goal

Compile all validated content into a structured DVP document and generate the final Excel output.

## Interaction Guide

Follow the Interaction Protocol defined in `SKILL.md`. This phase primarily uses **[Confirm]** and **[Done]** question types. This phase has the fewest questions.

| Decision point | Level | Notes |
|----------------|-------|-------|
| Template column mapping (unresolvable) | Must-ask | Cannot guess field mappings |
| Document section detail level | Recommend | Default to standard detail |
| JSON construction | Self-decide | Follow defined schema |
| Format details | Self-decide | Follow specification |

## Steps

### Step 1: Compile Document Sections

**[Self-decide]** Assemble the full DVP document with these sections (see `references/section-catalog.md` for details):

1. **Purpose** — Why this DVP exists, what it covers
2. **Scope** — Studies, modules, data types in scope
3. **Roles & Responsibilities** — Who does what in data validation
4. **Data Review Strategy** — Overall approach to data review (from Phase 2)
5. **Validation Check Details** — All check rules (from Phase 3, revised in Phase 4)
6. **External Data Reconciliation** — Third-party data reconciliation plan
7. **Query Management Rules** — How queries are generated, tracked, resolved
8. **Review Frequency & Milestones** — When reviews happen, aligned to project timeline
9. **Revision History** — Initial version entry
10. **Appendix** — Check list index, reference documents, mapping tables

**[Confirm]** If the detail level of document sections needs user input:
```
[Confirm] Document section detail level
  Recommendation: Standard detail — each section includes full description
  Rationale: Full detail is recommended for first draft; subsequent versions can be streamlined
  Alternative: Brief version — only essential content
  Please confirm whether to adopt the recommendation.
```

### Step 2: Format Check List

**[Self-decide]** Organize all check rules into the Check List structure. If using a user-provided template, map to the template's columns. Otherwise, use the default column structure from `references/excel-spec.md`.

**[Must-ask]** If template column names cannot be automatically matched:
```
[Collect] Template column mapping confirmation
  Background: The following column names in the template could not be auto-mapped to standard fields
  Please confirm the mapping:
  1. Template column "[Column A]" → Check description / Check logic / Other?
  2. Template column "[Column B]" → Severity / Priority / Other?
  ...
```

### Step 3: Build JSON Input

**[Self-decide]** Construct `dvp_content.json` with all compiled content:

```json
{
  "summary": {
    "protocol_number": "...",
    "study_phase": "...",
    "indication": "...",
    "sponsor": "...",
    "version": "1.0",
    "author": "...",
    "scope": "...",
    "key_data": "...",
    "risk_summary": "...",
    "validation_strategy": "...",
    "roles": [
      {"role": "Lead DM", "responsibility": "..."},
      {"role": "DM", "responsibility": "..."}
    ]
  },
  "checks": [
    {
      "check_id": "AE-001",
      "module": "AE",
      "category": "Completeness",
      "description": "...",
      "logic": "...",
      "scope": "...",
      "trigger": "...",
      "query_wording": "...",
      "severity": "Major",
      "method": "System",
      "status": "Active",
      "notes": ""
    }
  ],
  "revisions": [
    {"version": "1.0", "date": "YYYY-MM-DD", "author": "...", "description": "Initial draft"}
  ],
  "reconciliation": [
    {
      "recon_id": "RECON-001",
      "data_source": "Lab",
      "provider": "...",
      "key_fields": "...",
      "method": "...",
      "frequency": "...",
      "discrepancy_handling": "...",
      "responsible_party": "..."
    }
  ]
}
```

### Step 4: Generate Excel

**[Self-decide]** Execute the generation script:

```bash
python3 scripts/generate_xlsx.py \
  --input dvp_content.json \
  --output DVP_<ProtocolNumber>_v1.0.xlsx \
  [--template user_template.xlsx]
```

Verify the output file is created successfully.

### Step 5: Review Output

**[Self-decide]** After generating the Excel:
1. Read back key sections to verify correctness
2. Check that all modules are represented
3. Verify Check IDs are sequential and no gaps
4. Confirm sheet structure matches expected format

### Step 6: Present Draft

**[Done]** Present the draft to the user:
- Summary of document structure
- Total check count per module
- File path to the generated Excel
- Any remaining caveats or assumptions

```
[Done] Phase 5: Draft
  Output summary:
  - Document structure: [N] sections
  - Check rules: [N] total (per module: AE [N], Visit [N], ...)
  - Output file: [file path]
  - Assumptions made: [if any]

  Next: Phase 6: Internal Review
  Will proceed after your confirmation. Let me know if adjustments are needed.
```

Wait for user confirmation before proceeding to Phase 6.

## Tips

- Ensure the JSON input to the script is well-formed and complete before running.
- If the script fails, check for encoding issues (especially with Chinese characters in query wording).
- Generate a clean filename using protocol number and version.
- Keep a backup of the JSON input for potential regeneration.
