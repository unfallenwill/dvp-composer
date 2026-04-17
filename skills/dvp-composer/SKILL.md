---
name: dvp-composer
description: >
  This skill should be used when the user asks to "compose DVP", "create a DVP",
  "Data Validation Plan", "data cleaning plan", "edit check spec",
  "validation check list", "write DVP", "draft DVP", or mentions creating,
  reviewing, or drafting a data validation plan or data cleaning strategy
  for clinical trials in CRO or sponsor data management contexts.
version: 0.3.0
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

## Task Tracking

When the workflow starts, create a task list to give the user a clear roadmap and granular progress tracking.

### Phase-Level Tasks

At workflow startup, use `TaskCreate` to create 6 phase-level tasks in order. Each task must include `addBlockedBy` pointing to the previous phase's task ID (except Phase 1). Mark Phase 1 as `in_progress` immediately.

| Phase | subject | description | activeForm |
|-------|---------|-------------|------------|
| 1 | Phase 1: Collection | Gather all input materials and understand study context | Collecting study materials |
| 2 | Phase 2: Scope & Strategy | Define validation scope, key data, and risk points | Defining validation scope & strategy |
| 3 | Phase 3: Design Checks | Design executable check rules per module | Designing check rules |
| 4 | Phase 4: Alignment | Verify rules are correct, feasible, and conflict-free | Aligning check rules |
| 5 | Phase 5: Draft | Compile DVP document and generate Excel output | Drafting DVP document |
| 6 | Phase 6: Internal Review | Review for completeness, consistency, and clarity | Reviewing DVP internally |

Example:
```
TaskCreate subject="Phase 1: Collection" description="Gather all input materials and understand study context" activeForm="Collecting study materials"
→ returns task_id "1"

TaskCreate subject="Phase 2: Scope & Strategy" description="Define validation scope, key data, and risk points" activeForm="Defining validation scope & strategy" addBlockedBy=["1"]

...and so on for all 6 phases.

TaskUpdate taskId="1" status="in_progress"
```

### Sub-Tasks

When entering a phase, read its reference file and create sub-tasks for that phase's steps. Each sub-task should `addBlockedBy` the current phase's task ID. Mark each sub-task `completed` as its step finishes. When all sub-tasks are done, mark the phase-level task as `completed` and the next phase as `in_progress`.

### Phase Transition

At each `[Done]` confirmation:
1. Mark the current phase task as `completed`
2. Mark the next phase task as `in_progress`
3. Load the next phase file and create its sub-tasks

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

## Workspace

All phase deliverables are written to `dvp_workspace/` under the user's working directory.

### Rules

1. Create `dvp_workspace/` when the workflow starts
2. At each phase's [Done] step, write all deliverable files to `dvp_workspace/`
3. At the start of each phase (except Phase 1), read the previous phase's deliverable files first
4. File names are hard-coded in each phase instruction — do not rename them
5. Overwrite existing files without asking for confirmation
6. All files use Markdown format unless otherwise specified (.json, .xlsx)

### Cross-Phase File: assumptions-and-gaps.md

Phase 1 creates this file. Phases 2 and 3 may append new entries. Phase 4 resolves entries and marks them as Resolved. Always read the existing file before appending.

## Phase Instructions

For detailed instructions on each phase, load the corresponding reference file:

- **Phase 1**: `references/stages/phase-1-collection.md`
- **Phase 2**: `references/stages/phase-2-strategy.md`
- **Phase 3**: `references/stages/phase-3-design.md`
- **Phase 4**: `references/stages/phase-4-alignment.md`
- **Phase 5**: `references/stages/phase-5-draft.md`
- **Phase 6**: `references/stages/phase-6-review.md`

Load each phase file at the start of that phase. Do not load all phases upfront. After loading the phase file, read the previous phase's deliverable files from `dvp_workspace/` (except Phase 1, which has no predecessor). Each phase instruction lists exactly which files to read.

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

## Interaction Protocol

All phases must follow this unified interaction protocol for when and how to ask the user questions.

### Decision Framework — When to Ask

At each decision point, determine the appropriate level:

| Level | Condition | Action |
|-------|-----------|--------|
| **Self-decide** | Answer is derivable from provided materials; follows industry convention; low-impact and correctable | Decide independently, record assumption |
| **Recommend** | Reasonable default exists but multiple valid options are available | Present recommendation with rationale, user must explicitly confirm |
| **Must-ask** | Required information is missing; multiple options with irreversible impact; project-specific preference required | Ask structured question, wait for answer |

### Question Format — How to Ask

Use one of four question types, each with a text label:

**[Collect] Information gathering** — used when required input is missing:
```
[Collect] [topic] needs [specific information]
  Background: [why this information is needed]
  Please provide: [what exactly is needed]
```

**[Confirm] Recommendation** — used when presenting a default for confirmation:
```
[Confirm] [decision point]
  Recommendation: [option]
  Rationale: [why this option is recommended]
  Alternative: [other option(s)]
  Please confirm whether to adopt the recommendation, or choose an alternative.
```

**[Conflict] Conflict resolution** — used when checks conflict or overlap:
```
[Conflict] [Check ID] [issue description]
  Finding: [specific conflict content]
  Suggested resolution: [keep/merge/remove] — [reason]
  Please confirm: adopt suggestion / other approach
```

**[Done] Phase confirmation** — used at every phase transition:
```
[Done] Phase N: [phase name]
  Deliverables written to dvp_workspace/:
  - [file1.md] — [one-line description]
  - [file2.md] — [one-line description]

  Output summary:
  - [key output 1]
  - [key output 2]
  - Assumptions made: [if any]

  Next: Phase N+1: [phase name]
  Will proceed after your confirmation. Let me know if adjustments are needed.
```

### Batching Rules

- Combine multiple questions from the same phase into **one batch** rather than asking one by one.
- Order by priority: Must-ask first, Recommend-confirmation second.
- No more than 5 questions per batch.
- Recommendation-type questions must include rationale; user must explicitly confirm.

### Per-Phase Question Guide

| Phase | Must-ask | Recommend | Self-decide |
|-------|----------|-----------|-------------|
| 1. Collection | Protocol/CRF availability, template availability, material format | Default 4-sheet format (if no template) | Study design extraction, visit structure, data modules from materials |
| 2. Scope & Strategy | Scope boundaries, Edit Check coverage, key data confirmation | Validation method assignment, risk prioritization | Standard module strategy selection |
| 3. Design Checks | Ambiguous thresholds/boundaries, query language preference (CN/EN) | Check coverage density, severity grading | Check ID naming, standard check logic |
| 4. Alignment | Conflict resolution, feasibility trade-offs | Query burden assessment, low-priority rule consolidation | Numbering corrections, wording refinements |
| 5. Draft | Unresolvable template column mapping | Document section detail level | JSON construction, format details |
| 6. Review | Must-fix item changes | Should-fix item adoption | Nice-to-have items (default: no change) |

## Key Principles

1. **Interactive pacing**: Never rush through phases. Confirm understanding at each transition.
2. **Domain accuracy**: Use correct clinical DM terminology (domain names, check types, query handling).
3. **Practical focus**: Every check rule must be specific, executable, and testable.
4. **Risk-based**: Prioritize checks around critical data and key risk points.
5. **No hallucination**: Only generate checks based on information the user has provided or confirmed. If uncertain about protocol details, ask.
6. **Language adaptation**: Adapt all prompts, queries, and confirmation messages to match the user's conversation language (Chinese or English).
