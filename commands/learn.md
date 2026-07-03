---
description: Extract reusable patterns from the current session and save them as skills. Run after solving non-trivial problems to build your knowledge base.
---

# Learn Command

Analyze the current Antigravity session and extract patterns worth saving as skills.

## When to Use

Run `/learn` when you've solved a non-trivial problem during a session.

## What to Extract

1. **Error Resolution Patterns** - Root cause, fix, and reusability
2. **Debugging Techniques** - Non-obvious steps and tool combinations
3. **Workarounds** - Library quirks, API limitations, version-specific fixes
4. **Project-Specific Patterns** - Codebase conventions, architecture decisions, integration patterns

## Output Format

Creates a skill file at `~/.antigravity/skills/learned/[pattern-name].md`:

```markdown
# [Descriptive Pattern Name]

**Extracted:** [Date]
**Context:** [Brief description of when this applies]

## Problem
[What problem this solves - be specific]

## Solution
[The pattern/technique/workaround]

## Example
[Code example if applicable]

## When to Use
[Trigger conditions - what should activate this skill]
```

## Process

1. Review the session for extractable patterns
2. Identify the most valuable/reusable insight
3. Draft the skill file
4. Ask user to confirm before saving
5. Save to `~/.antigravity/skills/learned/`

## Notes

- Don't extract trivial fixes (typos, simple syntax errors)
- Don't extract one-time issues (specific API outages, etc.)
- Focus on patterns that will save time in future sessions
- Keep skills focused — one pattern per skill
