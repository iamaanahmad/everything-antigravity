# Everything Antigravity

**The definitive collection of configurations for Google's Antigravity IDE.**

Unlock the full potential of your Antigravity assistant with production-ready agents, skills, hooks, commands, rules, and MCP configurations. Battle-tested patterns evolved from real-world development.

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
│   └── tdd.md               # Test-driven development workflow
│
├── rules/            # Always-follow guidelines
│   └── security.md          # Mandatory security checks
│
├── contexts/         # Contexts for specific modes
│   ├── dev.md               # Development mode
│   ├── research.md          # Research mode
│   └── review.md            # Code review mode
│
└── mcp-configs/      # MCP server configurations
    └── mcp-servers.json     # GitHub, Supabase, Vercel, etc.
```

---

## Getting Started

### 1. Installation

Clone this repository into your project or integrate specific components into your Antigravity configuration.

```bash
git clone https://github.com/yourusername/everything-antigravity.git
cd everything-antigravity
```

### 2. Configure Agents

Copy desired agents from `agents/` to your `.antigravity/agents/` directory (or equivalent configuration location).

```bash
# Copy all agents
cp agents/*.md ~/.antigravity/agents/

# Or copy specific agents
cp agents/planner.md ~/.antigravity/agents/
cp agents/code-reviewer.md ~/.antigravity/agents/
```

### 3. Set Up Rules

Establish project-wide rules by placing files from `rules/` into `.antigravity/rules/`.

```bash
cp rules/*.md ~/.antigravity/rules/
```

### 4. Add Skills

Copy skills to enhance Antigravity's domain knowledge:

```bash
cp -r skills/* ~/.antigravity/skills/
```

### 5. Configure MCP Servers

Copy MCP server configurations and replace placeholders with your actual API keys:

```bash
cp mcp-configs/mcp-servers.json ~/.antigravity/mcp-servers.json
# Edit the file and replace YOUR_*_HERE with actual values
```

---

## Key Concepts

### Agents

Delegate specific tasks to specialized subagents. For example, the `code-reviewer` agent focuses solely on code quality and security.

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

### Commands

Execute complex tasks with simple slash commands like `/tdd` or `/plan`.

**Available Commands:**
- `/tdd` - Enforce test-driven development workflow

### Rules

Always-follow guidelines that apply to all code. Rules are automatically enforced by Antigravity.

**Available Rules:**
- **security.md** - Mandatory security checks (secrets, input validation, auth)

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

Create team-specific rules:
```
rules/
├── team-coding-style.md
├── team-git-workflow.md
├── team-review-process.md
└── team-deployment.md
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

## Comparison with Claude Code

This repository is inspired by [everything-claude-code](https://github.com/affaan-m/everything-claude-code) but adapted specifically for Google's Antigravity IDE.

**Key Differences:**
- Antigravity-specific agent configurations
- Gemini model references (gemini-2.0-pro-exp)
- Antigravity workflow patterns
- Context management for Antigravity

**Shared Concepts:**
- Agent-based delegation
- Skills for domain knowledge
- Rules for always-follow guidelines
- MCP server integrations

---

## Resources

- **Antigravity Documentation:** [Link to Antigravity docs]
- **MCP Protocol:** https://modelcontextprotocol.io
- **Inspired by:** [everything-claude-code](https://github.com/affaan-m/everything-claude-code)

---

## License

MIT - Use freely, modify as needed, contribute back if you can.

---

**Built for the future of coding with Google Deepmind's Antigravity.**

Star this repo if it helps. Customize for your needs. Build something great.
