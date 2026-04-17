# Phase 1: Collection

## Goal

Gather all input materials needed to compose the DVP. Understand the study context, key data, and validation requirements.

## Interaction Guide

Follow the Interaction Protocol defined in `SKILL.md`. This phase primarily uses **[Collect]** and **[Done]** question types.

| Decision point | Level | Notes |
|----------------|-------|-------|
| Whether user has Protocol/CRF | Must-ask | Required materials — cannot proceed without |
| Material format (paste/file/Q&A) | Must-ask | Determines how to ingest information |
| Whether user has a DVP template | Must-ask | Determines output format |
| Default 4-sheet format (no template) | Recommend | Present default, user confirms |
| Study design extraction from materials | Self-decide | Derive from provided materials |
| Visit structure, data modules | Self-decide | Derive from provided materials |
| Missing details not in materials | Must-ask | Ask targeted questions for gaps |

## Sub-Tasks

At the start of this phase, create the following sub-tasks. Each should `addBlockedBy` the Phase 1 task ID. Mark each `completed` when its step finishes.

| # | subject | description |
|---|---------|-------------|
| 1 | Request Input Materials | Ask user for Protocol, CRF, and other materials |
| 2 | Confirm DVP Template | Determine output format (user template or default) |
| 3 | Analyze Input Materials | Extract study design, key data, visit structure, data modules |
| 4 | Identify Gaps | Ask targeted questions for missing information |
| 5 | Present Summary | Output Phase 1 summary and confirm completion |

## Steps

### Step 1: Request Input Materials

**[Collect]** Ask the user to provide the following materials (any format: paste text, file path, or verbal description):

| Material | Priority | Purpose |
|----------|----------|---------|
| Protocol | Required | Understand study design, endpoints, visit schedule |
| CRF/eCRF | Required | Identify data fields and collection structure |
| SAP | Optional | Understand analysis needs that drive validation |
| Edit Check Spec | Optional | Identify system-level checks already in place |
| Database Build Docs | Optional | Understand DB structure and constraints |
| Lab/External Data Specs | Optional | Plan external data reconciliation |
| Project Timeline | Optional | Align validation milestones |
| DVP Template | Optional | Use user's preferred output format |

Batch all collection questions into one prompt. Example:

```
[Collect] Materials needed for DVP composition
  Background: The following materials are needed to compose the DVP
  Please provide (paste text, file paths, or verbal descriptions):
  1. [Required] Protocol — study protocol document
  2. [Required] CRF/eCRF — case report form
  3. [Optional] SAP / Edit Check Spec / DB Build Docs / External data specs
  4. [Optional] DVP Excel template — output will follow your template format
  Please provide whatever materials you have available — all are not required upfront.
```

### Step 2: Confirm DVP Template

**[Confirm]** Ask specifically whether the user has a DVP Excel template. If yes, read the template to understand:
- Sheet names and structure
- Column headers in Check List
- Required fields vs optional fields
- Formatting conventions

If no template is provided:
```
[Confirm] Output format
  Recommendation: Use the default 4-sheet format (Check List / Summary / Revision History / Ext Data Recon)
  Rationale: This is the industry-standard structure when no template is provided
  Alternative: Use a custom structure you define
  Please confirm whether to adopt the recommendation.
```

### Step 3: Analyze Input Materials

**[Self-decide]** After receiving materials, analyze and summarize:
1. **Study design**: Phase, indication, sponsor, study type
2. **Key data points**: Primary/secondary endpoints, critical variables
3. **Visit structure**: Schedule of assessments, visit windows
4. **Data modules**: Which domains are in scope (AE, Lab, ConMed, etc.)
5. **Existing checks**: What's already handled by the system

### Step 4: Identify Gaps

**[Collect]** List any critical information missing from the provided materials. Ask targeted questions to fill gaps. Do not fabricate study details.

Batch all gap questions together. Example:
```
[Collect] Items to clarify from the protocol
  Background: The following information was not found in the provided materials but is needed for DVP composition
  Please provide:
  1. [Visit windows] What is the allowed window (in days) for each visit?
  2. [SAE reporting] What is the SAE reporting timeline (in hours)?
  3. ...
```

### Step 5: Present Summary

**[Done]** Output a structured summary including:
- Study overview (protocol number, indication, phase, design)
- Available materials list
- Key data and modules identified
- Template decision (user template or default)
- Outstanding questions (if any)

```
[Done] Phase 1: Collection
  Output summary:
  - Study overview: [Protocol / Phase / Indication / Design]
  - Available materials: [list]
  - Key data and modules identified: [list]
  - Template decision: [user template / default 4-sheet]
  - Assumptions made: [if any]

  Next: Phase 2: Scope & Strategy
  Will proceed after your confirmation. Let me know if adjustments are needed.
```

Wait for user confirmation before proceeding to Phase 2.

**Task update**: Mark Phase 1 task as `completed`. Mark Phase 2 task as `in_progress`.

## Tips

- Not all materials are required to start. Begin with what's available and iterate.
- If the user provides a Protocol, extract the study design, visit schedule, and endpoints first.
- For CRF, focus on understanding the data collection structure rather than every single field.
- Record any assumptions made and flag them for user confirmation.
