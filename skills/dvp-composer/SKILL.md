---
name: dvp-composer
description: >
  This skill should be used when the user asks to "compose DVP", "create a DVP",
  "Data Validation Plan", "data cleaning plan", "edit check spec",
  "validation check list", "write DVP", "draft DVP", or mentions creating,
  reviewing, or drafting a data validation plan or data cleaning strategy
  for clinical trials in CRO or sponsor data management contexts.
version: 2.0.0
---

# DVP Composer

## Purpose

Guide the user through a structured 6-phase interactive workflow to compose a Data Validation Plan (DVP) for clinical trial Data Management in CRO context. Each phase builds on the previous one, with user confirmation required between phases.

## Workflow Overview

| Phase | Name | Goal |
|-------|------|------|
| 1 | Collection | Read protocol, CRF, SAP and other materials; understand study data context |
| 2 | Scope & Strategy | Define validation scope, key variables, risk points, and check methods |
| 3 | Design Checks | Design specific, executable check rules per module |
| 4 | Alignment | Confirm rules are correct, feasible, and not conflicting |
| 5 | Draft | Compile everything into a structured DVP document |
| 6 | Internal Review | Review for completeness, consistency, and clarity |

After completing each phase, present a summary to the user and ask for confirmation before proceeding to the next phase. If the user requests changes, iterate within the current phase before moving forward.

## Input Methods

Support three input methods simultaneously:
1. **Paste text**: User pastes protocol summaries, CRF descriptions, etc. directly into the conversation
2. **Read files**: User provides file paths (PDF, Word, Markdown); use the Read tool to access content
3. **Q&A**: Claude asks targeted questions, user answers verbally

Adapt questioning strategy based on what the user has readily available. Do not demand all materials upfront — work with what is provided and ask targeted questions for gaps.

## Output

Generate an Excel (.xlsx) file as the final DVP document.

**Template handling**: If the user provides a DVP Excel template during Phase 1, use that template's format and sheet structure. Otherwise, generate with the default 4-sheet format (see `references/excel-spec.md`).

**Default sheets**:
- **Check List** — All validation rules (Check ID, module, description, logic, trigger, query wording, priority, method)
- **Summary** — Document overview, scope, roles & responsibilities, review strategy
- **Revision History** — Version change log
- **Ext Data Recon** — External data reconciliation checks

## Phase Instructions

For detailed instructions on each phase, load the corresponding reference file:

- **Phase 1**: `references/stages/phase-1-collection.md`
- **Phase 2**: `references/stages/phase-2-strategy.md`
- **Phase 3**: `references/stages/phase-3-design.md`
- **Phase 4**: `references/stages/phase-4-alignment.md`
- **Phase 5**: `references/stages/phase-5-draft.md`
- **Phase 6**: `references/stages/phase-6-review.md`

Load each phase file at the start of that phase. Do not load all phases upfront.

## Additional References

- **`references/section-catalog.md`** — Standard DVP document structure. Load during Phase 5 to verify all required document sections are included.
- **`references/excel-spec.md`** — Detailed Excel output format specification. Load during Phase 5 when generating output.
- **`references/example-output.md`** — Example DVP output with sample check rules. Load during Phase 3 if the user wants to see examples.

## Script

Use `scripts/generate_xlsx.py` to generate the final Excel file. The script accepts a JSON input containing all DVP content and produces the formatted .xlsx output.

```bash
python3 scripts/generate_xlsx.py --input dvp_content.json --output DVP.xlsx [--template template.xlsx]
```

If the user provides a template, pass the `--template` flag to match the template's format.

## Key Principles

1. **Interactive pacing**: Never rush through phases. Confirm understanding at each transition.
2. **Domain accuracy**: Use correct clinical DM terminology (domain names, check types, query handling).
3. **Practical focus**: Every check rule must be specific, executable, and testable.
4. **Risk-based**: Prioritize checks around critical data and key risk points.
5. **No hallucination**: Only generate checks based on information the user has provided or confirmed. If uncertain about protocol details, ask.
6. **Language adaptation**: Adapt all prompts, queries, and confirmation messages to match the user's conversation language (Chinese or English).
