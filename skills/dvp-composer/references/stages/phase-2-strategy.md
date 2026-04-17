# Phase 2: Scope & Strategy

## Goal

Define the validation scope, identify key data and risk points, and establish the overall validation strategy.

## Read Previous Phase

Before starting Phase 2 work, read these files from `dvp_workspace/`:

- `study-overview.md` — Study context and endpoints
- `materials-received.md` — Materials inventory
- `visit-schedule.md` — Visit structure
- `data-modules.md` — Data modules and key fields
- `assumptions-and-gaps.md` — Assumptions and gaps

These contain all Phase 1 outputs. Use them as the factual basis for scope and strategy decisions.

## Deliverables

Before the [Done] step, write the following files to `dvp_workspace/`:

| File | Content |
|------|---------|
| `scope.md` | Per-module classification (DVP check / Edit Check covered / Excluded) with exclusion reasons |
| `key-data.md` | Critical data list: primary endpoints, safety data, submission key fields |
| `risk-assessment.md` | High/medium/low risk areas with rationale for each rating |
| `validation-methods.md` | Per-module validation method matrix (method + rationale) |
| `module-strategy.md` | Per-module validation strategy key points |

If new assumptions or gaps are identified during this phase, append them to `assumptions-and-gaps.md`.

## Interaction Guide

Follow the Interaction Protocol defined in `SKILL.md`. This phase primarily uses **[Confirm]** and **[Done]** question types.

| Decision point | Level | Notes |
|----------------|-------|-------|
| Scope boundaries (in/out modules) | Must-ask | Defines entire DVP coverage |
| Edit Check coverage overlap | Must-ask | Avoids duplication |
| Key data confirmation | Must-ask | User verifies critical data list |
| Validation method per category | Recommend | Default based on industry practice |
| Risk prioritization | Recommend | Default based on data criticality |
| Standard module strategy | Self-decide | Apply standard templates |

## Sub-Tasks

At the start of this phase, create the following sub-tasks. Each should `addBlockedBy` the Phase 2 task ID. Mark each `completed` when its step finishes.

| # | subject | description |
|---|---------|-------------|
| 1 | Define Data Cleaning Scope | Classify modules as DVP check, Edit Check covered, or excluded |
| 2 | Identify Key Data and Variables | List critical data and key variables from Phase 1 |
| 3 | Assess Risk Points | Identify risk areas based on study complexity |
| 4 | Define Validation Methods | Assign validation method per data category |
| 5 | Define Module-Level Strategy | Outline per-module validation approach |
| 6 | Present Strategy | Output Phase 2 strategy summary and confirm completion |

## Steps

### Step 1: Define Data Cleaning Scope

**[Self-decide]** Based on Phase 1 outputs, determine:
- Which data modules are in scope for validation
- Which modules are covered by system edit checks (no additional DVP checks needed)
- Which modules require manual/offline validation

**[Must-ask]** Present the scope boundary for user confirmation:
```
[Collect] Validation scope confirmation
  Background: Based on material analysis, here is the proposed validation scope
  Please confirm the scope classification for each module:
  1. [Module name] — DVP check / Covered by Edit Check / Excluded
  2. ...
```

### Step 2: Identify Key Data and Variables

**[Self-decide]** List critical data and key variables from Phase 1 materials:
- Primary and secondary endpoints
- Safety-critical data (SAE, death, discontinuation)
- Regulatory submission key fields
- Variables that directly impact analysis

**[Must-ask]** Confirm the key data list with the user:
```
[Collect] Key data confirmation
  Background: The following key data was identified from the protocol
  Please confirm whether anything is missing or needs adjustment:
  - Primary endpoints: [...]
  - Safety-critical data: [...]
  - Other key data: [...]
```

### Step 3: Assess Risk Points

**[Self-decide]** Identify risk areas based on materials:
- Complex visit schedules with windows
- Third-party data (lab, ECG, imaging) requiring reconciliation
- SAE reporting timelines
- Dosing/IP accountability
- Inclusion/exclusion criteria complexity
- Open-text fields requiring manual review

### Step 4: Define Validation Methods

**[Recommend]** For each category of data, determine the appropriate validation method:

| Method | When to Use |
|--------|------------|
| System Edit Check | Real-time validation at data entry; already in EDC |
| SAS Program | Batch validation on scheduled intervals |
| Listing Review | Manual review of data listings |
| Direct Query | Point-in-time query for specific data issues |
| Reconciliation | Compare internal vs external data sources |

Present as a recommendation:
```
[Confirm] Validation method assignment
  Recommendation: [specific method assignment per module]
  Rationale: Based on data characteristics per module and industry practice
  Alternative: [if different approaches exist]
  Please confirm whether to adopt the recommendation.
```

### Step 5: Define Module-Level Strategy

**[Self-decide]** For each key module, outline the validation approach:

- **AE/SAE**: Consistency checks, completeness, SAE timeline, causality assessment
- **ConMed**: Overlap with AE, IP interaction checks
- **Exposure/IP**: Dosing compliance, accountability, treatment emergent
- **Visit**: Visit windows, visit compliance, out-of-range visits
- **Lab**: Normal range checks, trending, external reconciliation
- **Inclusion/Exclusion**: Criteria compliance, violation identification
- **Demography**: Consistency, completeness, age calculations
- **Efficacy**: Endpoint-specific validation per SAP

### Step 6: Present Strategy

Before presenting the summary, write all five deliverable files listed in the Deliverables section above to `dvp_workspace/`. Also update `assumptions-and-gaps.md` if new items were identified.

**[Done]** Output the validation strategy as a structured document including:
- Scope definition (in-scope vs out-of-scope modules)
- Key data list
- Risk assessment summary
- Validation method matrix
- Module-level strategy summaries

```
[Done] Phase 2: Scope & Strategy
  Deliverables written to dvp_workspace/:
  - scope.md — Module scope classification
  - key-data.md — Critical data list
  - risk-assessment.md — Risk areas with ratings
  - validation-methods.md — Method matrix
  - module-strategy.md — Per-module strategy
  - assumptions-and-gaps.md — [Updated / No changes]

  Output summary:
  - Validation scope: [in-scope / out-of-scope module list]
  - Key data: [list]
  - Risk assessment: [high-risk areas]
  - Validation method matrix: [method per module]
  - Module strategy: [per-module summary]

  Next: Phase 3: Design Checks
  Will proceed after your confirmation. Let me know if adjustments are needed.
```

Wait for user confirmation before proceeding to Phase 3.

**Task update**: Mark Phase 2 task as `completed`. Mark Phase 3 task as `in_progress`.

## Tips

- Prioritize risk-based approach: focus more checks on higher-risk data.
- Consider query burden — avoid creating checks that generate excessive noise.
- Align validation strategy with SAP analysis needs where available.
- Document decisions on what NOT to include in DVP and why.
