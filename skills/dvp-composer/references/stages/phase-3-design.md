# Phase 3: Design Check Content

## Goal

Design specific, executable validation check rules for each data module. Every check must be clear, testable, and actionable.

## Read Previous Phase

Before starting Phase 3 work, read these files from `dvp_workspace/`:

- `study-overview.md` (Phase 1) — Study context
- `visit-schedule.md` (Phase 1) — Visit structure
- `data-modules.md` (Phase 1) — Data modules and key fields
- `scope.md` (Phase 2) — Which modules are in scope
- `key-data.md` (Phase 2) — Critical data to cover
- `risk-assessment.md` (Phase 2) — Risk priorities
- `validation-methods.md` (Phase 2) — Validation method per module
- `module-strategy.md` (Phase 2) — Per-module strategy
- `assumptions-and-gaps.md` (Phase 1/2) — Known assumptions and gaps

## Deliverables

Before the [Done] step, write the following files to `dvp_workspace/`:

| File | Content |
|------|---------|
| `checks.md` | Complete check rules list organized by module. Each check has all 10 fields in a table. |
| `check-rationale.md` | Design decisions: why checks were designed this way, boundary conditions, items not confirmed from materials |
| `unresolved.md` | Issues still unresolved, items needing Phase 4 confirmation or user input |

If new assumptions or gaps are identified, append them to `assumptions-and-gaps.md`.

## Interaction Guide

Follow the Interaction Protocol defined in `SKILL.md`. This phase primarily uses **[Collect]**, **[Confirm]**, and **[Done]** question types.

| Decision point | Level | Notes |
|----------------|-------|-------|
| Ambiguous thresholds/boundaries from protocol | Must-ask | Cannot guess protocol-specific values |
| Query language preference (CN/EN) | Must-ask | Project-specific preference |
| Check coverage density (standard/concise) | Recommend | Default to standard coverage |
| Severity grading for each check | Recommend | Default based on risk assessment |
| Check ID naming | Self-decide | Follow naming convention |
| Standard check logic design | Self-decide | Follow industry practice |

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

## Sub-Tasks

At the start of this phase, create the following sub-tasks. Each should `addBlockedBy` the Phase 3 task ID. Mark each `completed` when its step finishes.

| # | subject | description |
|---|---------|-------------|
| 1 | Clarify Design Parameters | Resolve ambiguous protocol details and confirm query language |
| 2 | Design by Module | Design checks per module covering all check types |
| 3 | Module Coverage | Ensure all applicable modules are covered |
| 4 | Assign Check IDs | Assign unique IDs following naming convention |
| 5 | Write Query Wording | Write clear, actionable query text for each check |
| 6 | Present Check Rules | Output complete check list organized by module |

## Steps

### Step 1: Clarify Design Parameters

**[Collect]** Before designing checks, batch any questions about ambiguous protocol details:

```
[Collect] Check design parameters
  Background: The following information affects check rule logic but is not clearly specified in the protocol
  Please provide:
  1. [Visit windows] Allowed window in days for each visit (e.g., ±3 days)
  2. [Age calculation] Reference date for age calculation (informed consent date or other)
  3. [SAE timeline] SAE reporting time window
  ...
```

**[Must-ask]** Confirm query language:
```
[Collect] Query language preference
  Background: Query wording in check rules requires a language selection
  Please choose: Chinese / English / Bilingual (Chinese + English)
```

**[Confirm]** Confirm coverage density:
```
[Confirm] Check coverage density
  Recommendation: Standard coverage (5-8 checks per module, covering major risk points)
  Rationale: Balances check completeness with query burden
  Alternative: Concise coverage (3-4 core checks per module)
  Please confirm whether to adopt the recommendation.
```

### Step 2: Design by Module

**[Self-decide]** Work through each module systematically. For each module, read the relevant Phase 1 & 2 outputs and design checks covering:

1. **Completeness checks**: Required fields not empty
2. **Consistency checks**: Logical consistency between related fields
3. **Range checks**: Values within expected ranges
4. **Cross-module checks**: Consistency across domains (e.g., AE dates vs visit dates)
5. **Timeline checks**: Date/time sequence and windows

### Step 3: Module Coverage

**[Self-decide]** Cover these modules as applicable to the study:

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

### Step 4: Assign Check IDs

**[Self-decide]** Follow naming convention:
- Prefix with module abbreviation (AE, LB, VS, DM, IE, CM, EX, PE, SC, MH, DS)
- Sequential numbering within module (AE-001, AE-002, ...)
- Maintain a registry to avoid duplicates

### Step 5: Write Query Wording

**[Self-decide]** For each check, write the query text that would appear in the EDC system:
- Be specific: "Start date of AE is before the date of first study drug administration. Please verify."
- Avoid vague queries: "Please check date" (too vague)
- Include the expected corrective action when possible
- Use the language preference confirmed in Step 1

### Step 6: Present Check Rules

Before presenting the summary, write all three deliverable files listed in the Deliverables section above to `dvp_workspace/`. Also update `assumptions-and-gaps.md` if new items were identified.

**[Done]** Output the complete check list organized by module. For each module:
- List all checks with full details
- Summarize check count and coverage
- Flag any areas where protocol details are insufficient

```
[Done] Phase 3: Design Checks
  Deliverables written to dvp_workspace/:
  - checks.md — Complete check rules list
  - check-rationale.md — Design decisions and rationale
  - unresolved.md — Items needing Phase 4 resolution
  - assumptions-and-gaps.md — [Updated / No changes]

  Output summary:
  - Total check rules: [N]
  - Per-module breakdown:
    - AE/SAE: [N]
    - Visit: [N]
    - Lab: [N]
    - ...
  - Coverage by type: Completeness [N] / Consistency [N] / Range [N] / Cross-module [N] / Timeline [N]
  - Query language: [CN/EN]
  - Assumptions made: [if any]

  Next: Phase 4: Alignment
  Will proceed after your confirmation. Let me know if adjustments are needed.
```

Wait for user confirmation before proceeding to Phase 4.

**Task update**: Mark Phase 3 task as `completed`. Mark Phase 4 task as `in_progress`.

## Tips

- Focus on actionable checks — every rule must result in a clear pass/fail outcome.
- Avoid redundant checks that overlap with existing edit checks.
- Consider the perspective of the site: queries should be easy to understand and respond to.
- For cross-module checks, ensure the logic is feasible given the data structure.
