# Phase 2: Scope & Strategy

## Goal

Define the validation scope, identify key data and risk points, and establish the overall validation strategy.

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

**[Done]** Output the validation strategy as a structured document including:
- Scope definition (in-scope vs out-of-scope modules)
- Key data list
- Risk assessment summary
- Validation method matrix
- Module-level strategy summaries

```
[Done] Phase 2: Scope & Strategy
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

## Tips

- Prioritize risk-based approach: focus more checks on higher-risk data.
- Consider query burden — avoid creating checks that generate excessive noise.
- Align validation strategy with SAP analysis needs where available.
- Document decisions on what NOT to include in DVP and why.
