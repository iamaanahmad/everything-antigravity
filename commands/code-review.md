---
description: Comprehensive security and quality review of uncommitted changes. Checks for security issues, code quality, and best practices.
---

# Code Review Command

This command invokes the **code-reviewer** agent to perform a comprehensive review of your code changes.

## What This Command Does

1. **Get Changed Files** - Identify all uncommitted changes
2. **Security Review** - Check for vulnerabilities and exposed secrets
3. **Quality Review** - Assess code quality and maintainability
4. **Best Practices** - Verify adherence to coding standards
5. **Generate Report** - Provide actionable feedback with severity levels

## When to Use

Use `/code-review` when:
- Before committing code
- After completing a feature
- Before creating a pull request
- After making significant changes
- When you want a second opinion

## Review Checklist

### Security Issues (CRITICAL)
- Hardcoded credentials, API keys, tokens
- SQL injection vulnerabilities
- XSS vulnerabilities
- Missing input validation
- Insecure dependencies
- Path traversal risks

### Code Quality (HIGH)
- Functions > 50 lines
- Files > 800 lines
- Nesting depth > 4 levels
- Missing error handling
- console.log statements
- TODO/FIXME comments
- Missing JSDoc for public APIs

### Best Practices (MEDIUM)
- Mutation patterns (use immutable instead)
- Emoji usage in code/comments
- Missing tests for new code
- Accessibility issues (a11y)

## Output Format

```
[CRITICAL] Hardcoded API key
File: src/api/client.ts:42
Issue: API key exposed in source code
Fix: Move to environment variable

const apiKey = "sk-abc123";  // ❌ Bad
const apiKey = process.env.API_KEY;  // ✓ Good
```

## Approval Criteria

- ✅ Approve: No CRITICAL or HIGH issues
- ⚠️ Warning: MEDIUM issues only (can merge with caution)
- ❌ Block: CRITICAL or HIGH issues found

## Important Notes

**Never approve code with security vulnerabilities!**

The code-reviewer agent will block commits if CRITICAL or HIGH severity issues are found.

## Related Agents

This command invokes the `code-reviewer` agent located at:
`.antigravity/agents/code-reviewer.md`
