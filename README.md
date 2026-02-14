# Everything Antigravity

**The definitive collection of configurations for Google's Antigravity IDE.**

Unlock the full potential of your Antigravity assistant with production-ready agents, skills, hooks, commands, rules, and MCP configurations. 

---

## What's Inside

This repository provides a comprehensive suite of tools designed to enhance your workflow with Antigravity:

```
everything-antigravity/
|-- agents/           # Specialized subagents for delegation
|   |-- planner.md           # Feature implementation planning
|   |-- architect.md         # System design decisions
|   |-- code-reviewer.md     # Quality and security review
|
|-- skills/           # Workflow definitions and domain knowledge
|   |-- coding-standards/    # Language best practices
|   |-- security-review/     # Security checklist
|
|-- commands/         # Slash commands for quick execution
|   |-- /tdd                 # Test-driven development workflow
|   |-- /plan                # Implementation planning
|   |-- /review              # Code quality review
|
|-- rules/            # Always-follow guidelines
|   |-- security.md          # Mandatory security checks
|   |-- coding-style.md      # Immutability, file organization
|
|-- mcp-configs/      # MCP server configurations
|   |-- mcp-servers.json     # GitHub, Supabase, Vercel, etc.
|
|-- contexts/         # Contexts for specific modes
|   |-- dev.md               # Development mode
|   |-- research.md          # Research mode
```

---

## Getting Started

### 1. Installation

Clone this repository into your project or integrate specific components into your Antigravity configuration.

### 2. Configure Agents

Copy desired agents from `agents/` to your `.antigravity/agents/` directory (or equivalent configuration location).

### 3. Set Up Rules

Establish project-wide rules by placing files from `rules/` into `.antigravity/rules/`.

---

## Key Concepts

### Agents
Delegate specific tasks to specialized subagents. For example, the `code-reviewer` agent focuses solely on code quality and security.

### Skills
Define reusable workflows. The `tdd-workflow` skill guides you through the Red-Green-Refactor cycle.

### Commands
Execute complex tasks with simple slash commands like `/plan` or `/review`.

---

## Contributing

Contributions are welcome! If you have useful agents, skills, or configurations for Antigravity, please submit a pull request.

---

**Built for the future of coding with Google Deepmind's Antigravity.**
