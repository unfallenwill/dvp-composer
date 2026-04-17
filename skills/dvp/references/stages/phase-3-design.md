# Phase 3: Design Check Content

## Goal

Design specific, executable validation check rules for each data module. Every check must be clear, testable, and actionable.

## Check Rule Structure

Each check rule should define:

| Field | Description |
|-------|-------------|
| Check ID | Unique identifier (e.g., AE-001, VISIT-012) |
| Module | Domain/module (AE, Lab, Visit, etc.) |
| Category | Check category (consistency, completeness, range, etc.) |
| Description | Clear description of what is being checked |
| Logic Rule | Exact logical condition (e.g., "if AE start date < first dose date") |
| Applicable Scope | Which subjects, visits, or conditions apply |
| Trigger Condition | When this check fires (data entry, batch, scheduled) |
| Query Wording | Exact query text shown to site |
| Severity/Priority | Critical, Major, Minor |
| Execution Method | System, SAS, Listing, Manual, Reconciliation |

## Steps

### Step 1: Design by Module

Work through each module systematically. For each module, read the relevant Phase 1 & 2 outputs and design checks covering:

1. **Completeness checks**: Required fields not empty
2. **Consistency checks**: Logical consistency between related fields
3. **Range checks**: Values within expected ranges
4. **Cross-module checks**: Consistency across domains (e.g., AE dates vs visit dates)
5. **Timeline checks**: Date/time sequence and windows

### Step 2: Module Coverage

Cover these modules as applicable to the study:

- **DM/Subject Status**: Enrollment, withdrawal, status changes
- **Demography**: DOB, sex, age calculation, ethnicity
- **Inclusion/Exclusion**: Criteria compliance, violation tracking
- **Visit**: Schedule compliance, visit windows, missed visits
- **AE/SAE**: Completeness, date logic, severity grading, SAE criteria, causality
- **ConMed**: Indication match with AE, date overlap with AE, route/dose logic
- **Exposure/IP**: Dosing compliance, treatment duration, dose modifications
- **Lab**: Collection timing, normal range flags, trending, unit consistency
- **Efficacy**: Endpoint-specific per SAP, assessment timing
- **Medical History**: Consistency with I/E criteria, ongoing conditions
- **Death/Disposition**: Date logic, reason consistency

### Step 3: Assign Check IDs

Follow naming convention:
- Prefix with module abbreviation (AE, LB, VS, DM, IE, CM, EX, PE, SC, MH, DS)
- Sequential numbering within module (AE-001, AE-002, ...)
- Maintain a registry to avoid duplicates

### Step 4: Write Query Wording

For each check, write the query text that would appear in the EDC system:
- Be specific: "Start date of AE is before the date of first study drug administration. Please verify."
- Avoid vague queries: "Please check date" (too vague)
- Include the expected corrective action when possible

### Step 5: Present Check Rules

Output the complete check list organized by module. For each module:
- List all checks with full details
- Summarize check count and coverage
- Flag any areas where protocol details are insufficient

Ask the user: "Here are the check rules for each module. Please review and confirm, and I will proceed to the next phase: Alignment."

Wait for user confirmation before proceeding to Phase 4.

## Tips

- Focus on actionable checks — every rule must result in a clear pass/fail outcome.
- Avoid redundant checks that overlap with existing edit checks.
- Consider the perspective of the site: queries should be easy to understand and respond to.
- For cross-module checks, ensure the logic is feasible given the data structure.
