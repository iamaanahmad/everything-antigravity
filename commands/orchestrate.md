---
description: Sequential multi-agent workflow for complex tasks. Chains specialized agents with structured handoffs for features, bugfixes, refactors, and security reviews.
---

# Orchestrate Command

Sequential agent workflow for complex tasks in Antigravity.

## Usage

`/orchestrate [workflow-type] [task-description]`

## Workflow Types

### feature
```
planner -> tdd-guide -> code-reviewer -> security-reviewer
```

### bugfix
```
planner -> tdd-guide -> code-reviewer
```

### refactor
```
architect -> code-reviewer -> tdd-guide
```

### security
```
security-reviewer -> code-reviewer -> architect
```

## Handoff Document Format

Between agents:

```markdown
## HANDOFF: [previous-agent] -> [next-agent]

### Context
[Summary of what was done]

### Findings
[Key discoveries or decisions]

### Files Modified
[List of files touched]

### Open Questions
[Unresolved items for next agent]

### Recommendations
[Suggested next steps]
```

## Final Report Format

```
ORCHESTRATION REPORT
====================
Workflow: feature
Task: [description]
Agents: [chain]

SUMMARY
-------
[One paragraph summary]

FILES CHANGED
-------------
[List all files modified]

TEST RESULTS
------------
[Test pass/fail summary]

SECURITY STATUS
---------------
[Security findings]

RECOMMENDATION
--------------
[SHIP / NEEDS WORK / BLOCKED]
```

## Parallel Execution

For independent checks, run agents in parallel:

```
Run simultaneously:
- code-reviewer (quality)
- security-reviewer (security)
- architect (design)

Merge results into single report
```

## Arguments

- `feature <description>` - Full feature workflow
- `bugfix <description>` - Bug fix workflow
- `refactor <description>` - Refactoring workflow
- `security <description>` - Security review workflow
- `custom <agents> <description>` - Custom agent sequence

## Example

```
/orchestrate feature "Add user authentication"
/orchestrate custom "architect,tdd-guide,code-reviewer" "Redesign caching layer"
```

## Tips

1. Start with planner for complex features
2. Always include code-reviewer before merge
3. Use security-reviewer for auth/payment/PII
4. Keep handoffs concise — focus on what next agent needs
