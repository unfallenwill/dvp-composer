# Phase 1: Collection

## Goal

Gather all input materials needed to compose the DVP. Understand the study context, key data, and validation requirements.

## Steps

### Step 1: Request Input Materials

Ask the user to provide the following materials (any format: paste text, file path, or verbal description):

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

### Step 2: Confirm DVP Template

Ask specifically whether the user has a DVP Excel template. If yes, read the template to understand:
- Sheet names and structure
- Column headers in Check List
- Required fields vs optional fields
- Formatting conventions

If no template is provided, note that the default 4-sheet format will be used (see `references/excel-spec.md`).

### Step 3: Analyze Input Materials

After receiving materials, analyze and summarize:
1. **Study design**: Phase, indication, sponsor, study type
2. **Key data points**: Primary/secondary endpoints, critical variables
3. **Visit structure**: Schedule of assessments, visit windows
4. **Data modules**: Which domains are in scope (AE, Lab, ConMed, etc.)
5. **Existing checks**: What's already handled by the system

### Step 4: Identify Gaps

List any critical information missing from the provided materials. Ask targeted questions to fill gaps. Do not fabricate study details.

### Step 5: Present Summary

Output a structured summary including:
- Study overview (protocol number, indication, phase, design)
- Available materials list
- Key data and modules identified
- Template decision (user template or default)
- Outstanding questions (if any)

Ask the user: "Here is the summary of the study materials. Please confirm if this is accurate, and I will proceed to the next phase: Scope & Strategy."

Wait for user confirmation before proceeding to Phase 2.

## Tips

- Not all materials are required to start. Begin with what's available and iterate.
- If the user provides a Protocol, extract the study design, visit schedule, and endpoints first.
- For CRF, focus on understanding the data collection structure rather than every single field.
- Record any assumptions made and flag them for user confirmation.
