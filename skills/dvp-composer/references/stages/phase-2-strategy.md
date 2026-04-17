# Phase 2: Scope & Strategy

## Goal

Define the validation scope, identify key data and risk points, and establish the overall validation strategy.

## Steps

### Step 1: Define Data Cleaning Scope

Based on Phase 1 outputs, determine:
- Which data modules are in scope for validation
- Which modules are covered by system edit checks (no additional DVP checks needed)
- Which modules require manual/offline validation

### Step 2: Identify Key Data and Variables

List critical data and key variables:
- Primary and secondary endpoints
- Safety-critical data (SAE, death, discontinuation)
- Regulatory submission key fields
- Variables that directly impact analysis

### Step 3: Assess Risk Points

Identify risk areas:
- Complex visit schedules with windows
- Third-party data (lab, ECG, imaging) requiring reconciliation
- SAE reporting timelines
- Dosing/IP accountability
- Inclusion/exclusion criteria complexity
- Open-text fields requiring manual review

### Step 4: Define Validation Methods

For each category of data, determine the appropriate validation method:

| Method | When to Use |
|--------|------------|
| System Edit Check | Real-time validation at data entry; already in EDC |
| SAS Program | Batch validation on scheduled intervals |
| Listing Review | Manual review of data listings |
| Direct Query | Point-in-time query for specific data issues |
| Reconciliation | Compare internal vs external data sources |

### Step 5: Define Module-Level Strategy

For each key module, outline the validation approach:

- **AE/SAE**: Consistency checks, completeness, SAE timeline, causality assessment
- **ConMed**: Overlap with AE, IP interaction checks
- **Exposure/IP**: Dosing compliance, accountability, treatment emergent
- **Visit**: Visit windows, visit compliance, out-of-range visits
- **Lab**: Normal range checks, trending, external reconciliation
- **Inclusion/Exclusion**: Criteria compliance, violation identification
- **Demography**: Consistency, completeness, age calculations
- **Efficacy**: Endpoint-specific validation per SAP

### Step 6: Present Strategy

Output the validation strategy as a structured document including:
- Scope definition (in-scope vs out-of-scope modules)
- Key data list
- Risk assessment summary
- Validation method matrix
- Module-level strategy summaries

Ask the user: "Here is the validation strategy overview. Please confirm if the direction is correct, and I will proceed to the next phase: Design Checks."

Wait for user confirmation before proceeding to Phase 3.

## Tips

- Prioritize risk-based approach: focus more checks on higher-risk data.
- Consider query burden — avoid creating checks that generate excessive noise.
- Align validation strategy with SAP analysis needs where available.
- Document decisions on what NOT to include in DVP and why.
