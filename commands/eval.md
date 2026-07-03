---
description: Manage eval-driven development workflow. Define capability and regression evals, check pass rates, and generate reports before shipping features.
---

# Eval Command

Manage eval-driven development workflow in Antigravity.

## Usage

`/eval [define|check|report|list] [feature-name]`

## Define Evals

`/eval define feature-name`

Creates `.antigravity/evals/feature-name.md`:

```markdown
## EVAL: feature-name
Created: $(date)

### Capability Evals
- [ ] [Description of capability 1]
- [ ] [Description of capability 2]

### Regression Evals
- [ ] [Existing behavior 1 still works]
- [ ] [Existing behavior 2 still works]

### Success Criteria
- pass@3 > 90% for capability evals
- pass^3 = 100% for regression evals
```

## Check Evals

`/eval check feature-name`

Runs evals and reports:

```
EVAL CHECK: feature-name
========================
Capability: X/Y passing
Regression: X/Y passing
Status: IN PROGRESS / READY
```

## Report Evals

`/eval report feature-name`

Generates full report with pass rates, notes, and a SHIP / NEEDS WORK / BLOCKED recommendation.

## List Evals

`/eval list`

```
EVAL DEFINITIONS
================
feature-auth      [3/5 passing] IN PROGRESS
feature-search    [5/5 passing] READY
feature-export    [0/4 passing] NOT STARTED
```

## Arguments

- `define <name>` - Create new eval definition
- `check <name>` - Run and check evals
- `report <name>` - Generate full report
- `list` - Show all evals
- `clean` - Remove old eval logs (keeps last 10 runs)
