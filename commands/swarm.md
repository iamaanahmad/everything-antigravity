---
description: Coordinate parallel subagents through Antigravity's native swarm workflow.
---

# Swarm Command

Use Antigravity's native `/swarm` command for long-running work that can be split
into independent subtasks and coordinated through a shared message board.

## Usage

`/swarm [task description]`

## When to Use

- The task has two or more independent workstreams.
- Each workstream has a clear deliverable and bounded scope.
- The final result can be integrated after the parallel work completes.

## Workflow

1. Define the goal and acceptance criteria.
2. Identify independent subtasks and assign clear ownership.
3. Keep shared files and interfaces explicit to avoid conflicting edits.
4. Review each subagent's result before integration.
5. Run the project's verification workflow after merging the results.

Use `/orchestrate` instead when the work requires a strict sequential handoff
between specialized agents.
