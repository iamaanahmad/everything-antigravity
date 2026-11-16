# Everything Antigravity

**The definitive collection of configurations for the Google Antigravity ecosystem.**

Unlock the full potential of your Antigravity assistant with production-ready agents, skills, hooks, commands, rules, and MCP configurations. Fully optimized for Antigravity 2.0's Dynamic Subagents, Asynchronous Automation, and Enterprise Integrations. Battle-tested patterns evolved from real-world development.

---

## Supported Ecosystem

These configurations are compatible across the entire **Antigravity** suite:
- **Antigravity 2.0:** The core agentic platform powering parallel subagents and automation.
- **Antigravity IDE:** The standalone desktop app for visual development and code management.
- **Antigravity CLI:** The fast, terminal-first interface for local orchestration.
- **Antigravity SDK:** The developer kit for building custom programmatic agents.

---

## What's Inside

This repository provides a comprehensive suite of tools designed to enhance your workflow with Antigravity:

```
everything-antigravity/
├── agents/           # Specialized subagents for delegation
│   ├── planner.md           # Feature implementation planning
│   ├── architect.md         # System design decisions
│   ├── code-reviewer.md     # Quality and security review
│   ├── tdd-guide.md         # Test-driven development
│   ├── security-reviewer.md # Vulnerability analysis
│   ├── build-error-resolver.md # TypeScript/build error fixing
│   ├── e2e-runner.md        # Playwright E2E testing
│   ├── refactor-cleaner.md  # Dead code cleanup
│   └── doc-updater.md       # Documentation sync
│
├── skills/           # Workflow definitions and domain knowledge
│   └── coding-standards/    # Language best practices
│
├── commands/         # Slash commands for quick execution
│   ├── tdd.md               # Test-driven development workflow
│   ├── plan.md              # Implementation planning
│   ├── code-review.md       # Code review
│   ├── build-fix.md         # Incremental build error fixing
│   ├── checkpoint.md        # Workflow checkpoints
│   ├── e2e.md               # E2E test generation and execution
│   ├── eval.md              # Eval-driven development
│   ├── learn.md             # Extract reusable patterns
│   ├── orchestrate.md       # Multi-agent workflow orchestration
│   ├── swarm.md             # Parallel subagent coordination
│   ├── refactor-clean.md    # Safe dead code removal
│   ├── test-coverage.md     # Coverage analysis and test generation
│   ├── update-codemaps.md   # Architecture documentation
│   ├── update-docs.md       # Documentation sync
│   └── verify.md            # Comprehensive verification
│
├── rules/            # Always-follow guidelines
│   ├── security.md          # Mandatory security checks
│   ├── coding-style.md      # Code style and conventions
│   ├── git-workflow.md      # Git workflow guidelines
│   ├── testing.md           # Testing requirements
│   ├── patterns.md          # Common patterns
│   ├── performance.md       # Performance guidelines
│   └── agents.md            # Agent orchestration guidelines
│
├── contexts/         # Contexts for specific modes
│   ├── dev.md               # Development mode
│   ├── research.md          # Research mode
│   └── review.md            # Code review mode
│
└── skills/           # Workflow definitions and domain knowledge
    ├── coding-standards.md  # Language best practices
    ├── backend-patterns/    # Backend architecture patterns
    ├── frontend-patterns/   # React/Next.js patterns
    ├── tdd-workflow/        # TDD workflow patterns
    ├── security-review/     # Security checklist and patterns
    └── verification-loop/   # Verification system
```

---

## Getting Started

### 1. Installation

Clone this repository into your project or integrate specific components into your Antigravity configuration.

```bash
git clone https://github.com/iamaanahmad/everything-antigravity.git
cd everything-antigravity
```

### 2. Configure Agents

Copy desired agents from `agents/` to your `.agents/agents/` directory (for project scope) or `~/.gemini/config/agents/` (for global scope).

```bash
# Copy all agents
cp agents/*.md .agents/agents/

# Or copy specific agents
cp agents/planner.md .agents/agents/
cp agents/code-reviewer.md .agents/agents/
```

### 3. Set Up Rules

Establish project-wide rules by copying `AGENTS.md` into your `.agents/` customization root.

```bash
cp AGENTS.md .agents/AGENTS.md
```

### 4. Add Skills

Copy skills to enhance Antigravity's domain knowledge:

```bash
cp -r skills/* .agents/skills/
```

### 5. Configure MCP Servers

Copy MCP server configurations and replace placeholders with your actual API keys:

```bash
cp mcp-configs/mcp-servers.json .agents/mcp-servers.json
# Edit the file and replace YOUR_*_HERE with actual values
```

---

## Key Concepts

### Agents

Delegate specific tasks to specialized subagents. Take advantage of Antigravity 2.0's **Dynamic Subagents** capability, allowing these specialized agents to be spawned for parallel task execution without cluttering your main context. For example, the `code-reviewer` agent focuses solely on code quality and security.

**Available Agents:**

| Agent | Purpose | When to Use |
|-------|---------|-------------|
| **planner** | Create implementation plans | Before starting new features |
| **architect** | System design decisions | Architectural changes, scalability |
| **code-reviewer** | Quality & security review | After writing/modifying code |
| **tdd-guide** | Test-driven development | Writing new features, fixing bugs |
| **security-reviewer** | Vulnerability detection | Handling user input, auth, sensitive data |
| **build-error-resolver** | Fix TypeScript/build errors | When build fails |
| **e2e-runner** | Playwright E2E testing | Testing critical user journeys |
| **refactor-cleaner** | Dead code cleanup | Removing unused code, refactoring |
| **doc-updater** | Documentation sync | Updating docs, generating codemaps |

### Skills

Define reusable workflows and domain knowledge. The `coding-standards` skill provides language-specific best practices.

**Available Skills:**
- **coding-standards** - Naming conventions, code structure, documentation standards
- **backend-patterns** - Backend architecture, API design, database patterns
- **frontend-patterns** - React/Next.js, state management, performance patterns
- **tdd-workflow** - Full TDD workflow with unit, integration, and E2E patterns
- **security-review** - Comprehensive security checklist and patterns
- **verification-loop** - Verification system for quality gates

### Commands

Execute complex tasks with simple slash commands.

**Native Antigravity Commands (Built-in):**
- `/goal` - Run a long-running task autonomously without stopping until the goal is achieved.
- `/schedule` - Set recurring crons or one-time timers for asynchronous automation.
- `/grill-me` - Align on a plan through an interactive interview.
- `/browser` - Launch a browser subagent to interact with web pages.
- `/learn` - Automatically extract patterns and persist behaviors.
- `/swarm` - Coordinate parallel subagents through a shared message board.

Antigravity App v2.32.0 also provides **Dynamic Workflows**, which can
orchestrate parallel subagents and multi-step tasks in Python using the bundled
runtime. Use `/swarm` for coordinated parallel work and the included
`/orchestrate` command for sequential handoffs.

**Custom Workflow Commands (Included in this repo):**
- `/tdd` - Test-driven development
- `/plan` - Implementation planning
- `/code-review` - Code review
- `/build-fix` - Incremental build error fixing
- `/checkpoint` - Workflow checkpoints
- `/e2e` - E2E test generation and execution
- `/eval` - Eval-driven development
- `/learn` - Extract reusable patterns as skills
- `/orchestrate` - Multi-agent workflow orchestration
- `/swarm` - Parallel subagent coordination
- `/refactor-clean` - Safe dead code removal
- `/test-coverage` - Coverage analysis and test generation
- `/update-codemaps` - Architecture documentation
- `/update-docs` - Documentation sync
- `/verify` - Comprehensive codebase verification

### Rules

Always-follow guidelines that apply to all code. Rules are automatically enforced by Antigravity.

**Available Rules:**
- **security.md** - Mandatory security checks (secrets, input validation, auth)
- **coding-style.md** - Code style and naming conventions
- **git-workflow.md** - Git workflow and commit guidelines
- **testing.md** - Testing requirements and TDD workflow
- **patterns.md** - Common reusable patterns
- **performance.md** - Performance optimization guidelines
- **agents.md** - Agent orchestration and model selection

### Contexts

Dynamic system prompt injection for specific modes (development, research, review).

**Available Contexts:**
- **dev.md** - Active development mode
- **research.md** - Research and exploration mode
- **review.md** - Code review mode

### MCP Servers

Model Context Protocol servers provide external integrations (GitHub, databases, APIs).

**Configured Servers:**
- **github** - GitHub operations (PRs, issues, repos)
- **firecrawl** - Web scraping and crawling
- **supabase** - PostgreSQL database operations
- **memory** - Persistent memory across sessions
- **sequential-thinking** - Chain-of-thought reasoning
- **vercel** - Vercel deployments and projects
- **filesystem** - Filesystem operations

---

## Usage Examples

### Planning a New Feature

```
User: I need to add real-time notifications when markets resolve

Antigravity: Let me create an implementation plan for you.
[Invokes planner agent]

Planner Agent:
# Implementation Plan: Real-Time Market Resolution Notifications

## Requirements
- Send notifications to users when markets they're watching resolve
- Support multiple channels (in-app, email, webhook)
- Ensure reliable delivery

## Implementation Phases

### Phase 1: Database Schema
- Add notifications table
- Add user_notification_preferences table
- Create indexes

### Phase 2: Notification Service
- Create notification service
- Implement queue using Redis
- Add retry logic

### Phase 3: Integration Points
- Hook into market resolution logic
- Query users with positions
- Enqueue notifications

### Phase 4: Frontend Components
- Create NotificationBell component
- Add NotificationList modal
- Implement real-time updates

## Risks
- HIGH: Email deliverability
- MEDIUM: Performance with 1000+ users per market

**WAITING FOR CONFIRMATION**: Proceed? (yes/no/modify)
```

### Code Review

```
User: Review my changes

Antigravity: Running code review...
[Invokes code-reviewer agent]

Code Reviewer:
# Code Review Report

## Critical Issues (Must Fix)

### 1. Hardcoded API Key
**File:** src/api/client.ts:42
**Issue:** API key exposed in source code

```typescript
// ❌ Bad
const apiKey = "sk-abc123"

// ✅ Good
const apiKey = process.env.API_KEY
```

## High Issues (Should Fix)

### 2. Missing Input Validation
**File:** src/api/markets.ts:15
**Issue:** User input not validated before database query

**Recommendation:** ❌ BLOCK - Fix critical issues before merging
```

### Test-Driven Development

```
User: /tdd Implement market search function

Antigravity: Starting TDD workflow...
[Invokes tdd-guide agent]

TDD Guide:
## Step 1: Write Failing Test (RED)

```typescript
describe('searchMarkets', () => {
  it('returns markets matching query', async () => {
    const results = await searchMarkets('election')
    expect(results).toHaveLength(5)
  })
})
```

Running test... ❌ FAILED (expected - no implementation yet)

## Step 2: Write Minimal Implementation (GREEN)

```typescript
export async function searchMarkets(query: string) {
  // Minimal implementation
  return await db.markets.search(query).limit(5)
}
```

Running test... ✅ PASSED

## Step 3: Refactor (IMPROVE)

[Suggests improvements while keeping tests green]
```

---

## Best Practices

### Agent Usage

1. **Use planner before coding** - Plan complex features before implementation
2. **Use code-reviewer after changes** - Review all code before committing
3. **Use tdd-guide for new features** - Write tests first, then implement
4. **Use security-reviewer for sensitive code** - Review auth, payments, user input
5. **Use build-error-resolver when stuck** - Fix TypeScript errors efficiently

### Skill Development

1. **Keep skills focused** - One skill per domain (backend, frontend, security)
2. **Update regularly** - Evolve skills as patterns emerge
3. **Document examples** - Include code examples in skills
4. **Version control** - Track skill changes in git

### Rule Management

1. **Keep rules minimal** - Only enforce critical guidelines
2. **Make rules specific** - Vague rules are hard to follow
3. **Provide examples** - Show good and bad patterns
4. **Review regularly** - Remove outdated rules

### MCP Configuration

1. **Don't enable all MCPs** - Keep under 10 enabled per project
2. **Use project-specific configs** - Different projects need different tools
3. **Secure your keys** - Never commit API keys to git
4. **Monitor context usage** - Too many MCPs reduce available context

---

## Customization

These configs are starting points. Customize for your stack:

### For Your Language/Framework

Create language-specific skills:
```
skills/
├── python-conventions/
├── react-patterns/
├── go-best-practices/
└── rust-patterns/
```

### For Your Domain

Create domain-specific agents:
```
agents/
├── ml-engineer.md        # Machine learning specialist
├── devops-specialist.md  # Infrastructure and deployment
├── data-engineer.md      # Data pipelines and ETL
└── mobile-developer.md   # iOS/Android development
```

### For Your Team

Add team-specific rules to your `AGENTS.md` file:
```markdown
# AGENTS.md
- Team Coding Style
- Team Git Workflow
- Team Review Process
- Team Deployment
```

---

## Contributing

**Contributions are welcome!** If you have useful agents, skills, or configurations for Antigravity, please submit a pull request.

### Ideas for Contributions

- Language-specific skills (Python, Go, Rust patterns)
- Framework-specific configs (Django, Rails, Laravel)
- DevOps agents (Kubernetes, Terraform, AWS)
- Testing strategies (different frameworks)
- Domain-specific knowledge (ML, data engineering, mobile)

See [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines.

---

## Important Notes

### Context Window Management

**Critical:** Don't enable all MCPs at once. Your context window can shrink significantly with too many tools enabled.

**Rule of thumb:**
- Have 20-30 MCPs configured
- Keep under 10 enabled per project
- Monitor context usage

### Security

**Never commit secrets:**
- Use environment variables for API keys
- Add `.env` to `.gitignore`
- Use `.env.example` for documentation
- Rotate keys if accidentally committed

### Performance

**Optimize for speed:**
- Use specific agents for specific tasks
- Keep skills focused and concise
- Disable unused MCP servers
- Monitor token usage

---

## Resources

- **Antigravity Documentation:** https://antigravity.google/docs/
- **MCP Protocol:** https://modelcontextprotocol.io
---

## License

MIT - Use freely, modify as needed, contribute back if you can.

---

**Built for the future of coding with Google Deepmind's Antigravity.**

Star this repo if it helps. Customize for your needs. Build something great.
