# DVP Composer

Data Validation Plan (DVP) composition assistant for clinical trial Data Management.

## Overview

DVP Composer is a Claude Code plugin designed for **Data Managers** at CROs and sponsors. It guides you through a structured, interactive workflow to compose a complete Data Validation Plan for clinical trials — from collecting protocol materials to generating a formatted Excel deliverable.

## Features

- **6-Phase Interactive Workflow** — Collection → Scope & Strategy → Design Checks → Alignment → Draft → Internal Review
- **Two-Level Task Tracking** — Phase-level roadmap at startup, sub-task breakdown within each phase for clear progress visibility
- **Unified Interaction Protocol** — Structured decision framework (Self-decide / Recommend / Must-ask) with four question types (Collect, Confirm, Conflict, Done)
- **Multiple Input Methods** — Paste text, provide file paths (PDF/Word/Markdown), or answer Q&A-style prompts
- **Excel Output** — Generates formatted `.xlsx` with Check List, Summary, Revision History, and External Data Reconciliation sheets
- **Template Support** — Use your own DVP Excel template or the default 4-sheet format
- **Built-in Quality Review** — Dedicated reviewer agent that checks completeness, consistency, logic, and quality before finalization

## Workflow

| Phase | Name | Description |
|-------|------|-------------|
| 1 | Collection | Gather protocol, CRF, SAP and other input materials; understand study context |
| 2 | Scope & Strategy | Define validation scope, identify key variables and risk points, assign validation methods |
| 3 | Design Checks | Design specific, executable check rules for each data module |
| 4 | Alignment | Verify rules are correct, feasible, and not conflicting with existing systems |
| 5 | Draft | Compile all content into a structured DVP document and generate Excel output |
| 6 | Internal Review | Comprehensive review for completeness, consistency, and clarity |

### Task Tracking

When the workflow starts, 6 phase-level tasks are created with chained dependencies, giving you a full roadmap. As each phase begins, its sub-steps are broken down into individual tasks — so you always know what's done and what's next.

## Quick Start

### Prerequisites

- Python 3.8+
- openpyxl: `pip install openpyxl`

### Install

Add the plugin to your Claude Code environment:

```bash
claude install-plugin /path/to/dvp-composer
```

Or copy the project directory into `.claude-plugin/` in your working directory.

### Trigger

In Claude Code, use any of the following:

```
/dvp-composer
```

Or describe your need naturally:

- "compose DVP"
- "create a Data Validation Plan"
- "I need to write a DVP for my study"
- "帮我写一个 DVP"

Then follow the interactive prompts through each phase.

## Project Structure

```
dvp-composer/
├── .claude-plugin/
│   └── plugin.json                        # Plugin manifest
├── CLAUDE.md                              # Claude Code instructions
├── README.md
└── skills/dvp-composer/
    ├── SKILL.md                           # Main skill definition & workflow orchestration
    ├── agents/
    │   └── dvp-reviewer.md                # Quality review agent for Phase 6
    ├── scripts/
    │   └── generate_xlsx.py               # Excel generation script
    └── references/
        ├── stages/
        │   ├── phase-1-collection.md      # Phase 1 instructions
        │   ├── phase-2-strategy.md        # Phase 2 instructions
        │   ├── phase-3-design.md          # Phase 3 instructions
        │   ├── phase-4-alignment.md       # Phase 4 instructions
        │   ├── phase-5-draft.md           # Phase 5 instructions
        │   └── phase-6-review.md          # Phase 6 instructions
        ├── excel-spec.md                  # Excel output format specification
        ├── section-catalog.md             # Standard DVP document structure
        └── example-output.md              # Example DVP output with sample checks
```

## License

MIT
