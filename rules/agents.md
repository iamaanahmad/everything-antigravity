# Antigravity Agent Orchestration

**Guidelines for when and how to delegate to specialized subagents.**

## Available Agents

Located in `~/.antigravity/agents/`:

| Agent | Purpose | When to Use |
|-------|---------|-------------|
| planner | Implementation planning | Complex features, refactoring |
| architect | System design | Architectural decisions |
| tdd-guide | Test-driven development | New features, bug fixes |
| code-reviewer | Code review | After writing code |
| security-reviewer | Security analysis | Before commits, auth/payment code |
| build-error-resolver | Fix build errors | When build fails |
| e2e-runner | E2E testing | Critical user flows |
| refactor-cleaner | Dead code cleanup | Code maintenance |
| doc-updater | Documentation | Updating docs, codemaps |

## Immediate Agent Usage

No user prompt needed — invoke proactively:

1. Complex feature requests → **planner** agent
2. Code just written/modified → **code-reviewer** agent
3. Bug fix or new feature → **tdd-guide** agent
4. Architectural decision → **architect** agent
5. Auth, payments, user input → **security-reviewer** agent

## Parallel Task Execution

ALWAYS use parallel execution for independent operations:

```
# GOOD: Parallel execution
Launch 3 agents simultaneously:
1. Agent 1: Security analysis of auth.ts
2. Agent 2: Performance review of cache system
3. Agent 3: Type checking of utils.ts

# BAD: Sequential when unnecessary
First agent 1, then agent 2, then agent 3
```

## Multi-Perspective Analysis

For complex problems, use split role sub-agents:
- Factual reviewer
- Senior engineer
- Security expert
- Consistency reviewer

## Model Selection

- `gemini-2.0-flash` — Lightweight agents, frequent invocation, simple tasks
- `gemini-2.0-pro-exp` — Main development work, orchestration, complex coding (default)
- `gemini-2.0-ultra` — Deep reasoning, complex architecture, critical security analysis

## Context Window Management

- Monitor context usage during long sessions
- Break large tasks into smaller chunks using subagents
- Use specific file paths — avoid loading entire codebase
- Clear context between unrelated tasks
