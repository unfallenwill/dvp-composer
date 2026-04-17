# DVP Composer

Data Validation Plan (DVP) composition assistant for clinical trial Data Management.

## Overview

DVP Composer is a Claude Code plugin that guides users through a structured 6-phase workflow to create a complete Data Validation Plan for clinical trials in CRO/DM context.

## Features

- **6-Phase Interactive Workflow**: Collection → Strategy → Design → Alignment → Draft → Review
- **Multiple Input Methods**: Paste text, read files (PDF/Word/MD), or Q&A style
- **Excel Output**: Generates formatted .xlsx with Check List, Summary, Revision History, and External Data Reconciliation sheets
- **Template Support**: Use your own DVP Excel template or the default format
- **Quality Review**: Built-in reviewer agent for completeness, consistency, and quality checks

## Prerequisites

- Python 3.8+
- openpyxl: `pip install openpyxl`

## Installation

```bash
# Install as Claude Code plugin
claude /plugin install /path/to/dvp-composer
```

Or copy to your project's `.claude-plugin/` directory.

## Usage

In Claude Code, trigger the skill with:

```
/dvp
```

Or simply describe your need:
- "compose DVP"
- "create a Data Validation Plan"
- "I need to write a DVP for my study"

Then follow the interactive prompts through each phase.

## Workflow Phases

| Phase | Name | Description |
|-------|------|-------------|
| 1 | Collection | Gather protocol, CRF, SAP and other input materials |
| 2 | Scope & Strategy | Define validation scope, key variables, and risk points |
| 3 | Design Checks | Design specific check rules per data module |
| 4 | Alignment | Confirm rules are correct and feasible |
| 5 | Draft | Compile into structured DVP document and generate Excel |
| 6 | Internal Review | Review for completeness, consistency, and quality |

## Components

- **Skills**: 1 main skill (`dvp`) with 6 phase reference files
- **Agents**: 1 reviewer agent (`dvp-reviewer`) for quality validation
- **Scripts**: `generate_xlsx.py` for Excel generation

## License

MIT
