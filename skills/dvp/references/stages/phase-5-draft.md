# Phase 5: Draft

## Goal

Compile all validated content into a structured DVP document and generate the final Excel output.

## Steps

### Step 1: Compile Document Sections

Assemble the full DVP document with these sections (see `references/section-catalog.md` for details):

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

### Step 2: Format Check List

Organize all check rules into the Check List structure. If using a user-provided template, map to the template's columns. Otherwise, use the default column structure from `references/excel-spec.md`.

### Step 3: Build JSON Input

Construct `dvp_content.json` with all compiled content:

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

Execute the generation script:

```bash
python3 scripts/generate_xlsx.py \
  --input dvp_content.json \
  --output DVP_<ProtocolNumber>_v1.0.xlsx \
  [--template user_template.xlsx]
```

Verify the output file is created successfully.

### Step 5: Review Output

After generating the Excel:
1. Read back key sections to verify correctness
2. Check that all modules are represented
3. Verify Check IDs are sequential and no gaps
4. Confirm sheet structure matches expected format

### Step 6: Present Draft

Present the draft to the user:
- Summary of document structure
- Total check count per module
- File path to the generated Excel
- Any remaining caveats or assumptions

Ask the user: "The DVP draft has been generated. Please review the file, and I will proceed to the final phase: Internal Review."

Wait for user confirmation before proceeding to Phase 6.

## Tips

- Ensure the JSON input to the script is well-formed and complete before running.
- If the script fails, check for encoding issues (especially with Chinese characters in query wording).
- Generate a clean filename using protocol number and version.
- Keep a backup of the JSON input for potential regeneration.
