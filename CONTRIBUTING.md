# Contributing to Everything Antigravity

Thanks for wanting to contribute! This repo is meant to be a community resource for Antigravity IDE users.

## What We're Looking For

### Agents

New agents that handle specific tasks well:
- Language-specific reviewers (Python, Go, Rust, Java)
- Framework experts (Django, Rails, Laravel, Spring, Flutter)
- DevOps specialists (Kubernetes, Terraform, Docker, CI/CD)
- Domain experts (ML pipelines, data engineering, mobile, blockchain)
- Testing specialists (different frameworks and strategies)

### Skills

Workflow definitions and domain knowledge:
- Language best practices (Python, Go, Rust, Java, etc.)
- Framework patterns (React, Vue, Angular, Next.js, etc.)
- Testing strategies (Jest, Pytest, Playwright, etc.)
- Architecture guides (microservices, serverless, etc.)
- Domain-specific knowledge (ML, data, mobile, web3)

### Commands

Slash commands that invoke useful workflows:
- Deployment commands
- Testing commands
- Documentation commands
- Code generation commands
- Refactoring commands

### Rules

Always-follow guidelines:
- Security rules
- Code style rules
- Testing requirements
- Naming conventions
- Team-specific guidelines

### Contexts

Mode-specific behaviors:
- Development contexts
- Review contexts
- Research contexts
- Debugging contexts

### MCP Configurations

New or improved MCP server configs:
- Database integrations (PostgreSQL, MongoDB, etc.)
- Cloud provider MCPs (AWS, GCP, Azure)
- Monitoring tools (Datadog, Sentry, etc.)
- Communication tools (Slack, Discord, etc.)
- Development tools (GitHub, GitLab, Jira, etc.)

---

## How to Contribute

### 1. Fork the Repo

```bash
git clone https://github.com/YOUR_USERNAME/everything-antigravity.git
cd everything-antigravity
```

### 2. Create a Branch

```bash
git checkout -b add-python-reviewer
```

### 3. Add Your Contribution

Place files in the appropriate directory:
- `agents/` for new agents
- `skills/` for skills (can be single .md or directory)
- `commands/` for slash commands
- `rules/` for rule files
- `contexts/` for context definitions
- `mcp-configs/` for MCP server configs

### 4. Follow the Format

#### Agents

Agents should have frontmatter with Gemini model references:

```markdown
---
name: agent-name
description: What it does and when to use it
tools: Read, Grep, Glob, Bash
model: gemini-2.0-pro-exp
---

You are an expert [role] specializing in [domain].

## Your Role

- Responsibility 1
- Responsibility 2

## Workflow

### Step 1: [Name]
[Description]

### Step 2: [Name]
[Description]

## Best Practices

...
```

**Model Selection:**
- `gemini-2.0-flash` - Fast, lightweight tasks
- `gemini-2.0-pro-exp` - Main development work (default)
- `gemini-2.0-ultra` - Complex reasoning tasks

#### Skills

Skills should be clear and actionable:

```markdown
# Skill Name

## When to Use

[Description of when this skill applies]

## How It Works

[Step-by-step workflow]

## Examples

[Code examples and patterns]

## Best Practices

[Do's and don'ts]
```

#### Commands

Commands should explain what they do:

```markdown
---
description: Brief description of command
---

# Command Name

This command invokes the **agent-name** agent to [purpose].

## What This Command Does

1. Step 1
2. Step 2

## When to Use

Use `/command` when:
- Scenario 1
- Scenario 2

## How It Works

[Detailed explanation]

## Related Agents

This command invokes the `agent-name` agent.
```

#### Rules

Rules should be specific and enforceable:

```markdown
# Rule Category

**Mandatory guidelines for [scope].**

## 1. Rule Name

### Description
[What the rule is]

### Examples

```typescript
// ❌ Bad
[bad example]

// ✅ Good
[good example]
```

### Enforcement
[How this rule is enforced]
```

#### Contexts

Contexts should define mode-specific behavior:

```markdown
# Antigravity [Mode] Context

Mode: [Mode name]
Focus: [What this mode focuses on]

## Behavior
- Behavior 1
- Behavior 2

## Priorities
1. Priority 1
2. Priority 2

## Tools to Favor
- tool1 for purpose1
- tool2 for purpose2

## Antigravity Specifics
- Specific consideration 1
- Specific consideration 2
```

### 5. Test Your Contribution

Make sure your config works with Antigravity IDE before submitting:

1. Copy files to your Antigravity config directory
2. Test the agent/command/rule in real scenarios
3. Verify it works as expected
4. Document any dependencies or setup required

### 6. Submit a PR

```bash
git add .
git commit -m "Add Python code reviewer agent"
git push origin add-python-reviewer
```

Then open a PR with:
- **What you added** - Clear description of the contribution
- **Why it's useful** - Explain the problem it solves
- **How you tested it** - Show that it works
- **Any dependencies** - List required packages or setup

---

## Guidelines

### DO

- ✅ Keep configs focused and modular
- ✅ Include clear descriptions
- ✅ Test before submitting
- ✅ Follow existing patterns
- ✅ Document any dependencies
- ✅ Use Gemini model references (not Claude)
- ✅ Provide examples and use cases
- ✅ Write clear, actionable instructions

### DON'T

- ❌ Include sensitive data (API keys, tokens, paths)
- ❌ Add overly complex or niche configs
- ❌ Submit untested configs
- ❌ Create duplicate functionality
- ❌ Add configs requiring specific paid services without alternatives
- ❌ Use Claude-specific terminology or models
- ❌ Submit configs without documentation

---

## File Naming

- Use lowercase with hyphens: `python-reviewer.md`
- Be descriptive: `tdd-workflow.md` not `workflow.md`
- Match the agent/skill name to the filename
- Use `.md` extension for all documentation

---

## Code of Conduct

### Be Respectful

- Treat all contributors with respect
- Be constructive in feedback
- Welcome newcomers
- Help others learn

### Be Professional

- Keep discussions on-topic
- Avoid offensive language
- Focus on the contribution, not the person
- Assume good intentions

### Be Collaborative

- Share knowledge freely
- Give credit where due
- Build on others' work
- Improve existing contributions

---

## Review Process

1. **Submission** - You submit a PR
2. **Initial Review** - Maintainers check format and quality
3. **Testing** - Maintainers test the contribution
4. **Feedback** - You address any comments
5. **Approval** - PR is approved and merged
6. **Recognition** - You're added to contributors list

---

## Recognition

All contributors will be:
- Listed in the README
- Credited in release notes
- Mentioned in announcements
- Part of the community

---

## Questions?

- **Issues**: Open an issue on GitHub
- **Discussions**: Join GitHub Discussions
- **Documentation**: See [README.md](README.md) and [INSTALL.md](INSTALL.md)

---

## Ideas for Contributions

Need inspiration? Here are some ideas:

### High Priority
- [ ] Python development agent
- [ ] Go development agent
- [ ] Rust development agent
- [ ] React patterns skill
- [ ] Vue patterns skill
- [ ] Django patterns skill
- [ ] Kubernetes deployment agent
- [ ] Database migration agent

### Medium Priority
- [ ] Mobile development agents (iOS, Android, Flutter)
- [ ] Data engineering skills
- [ ] ML/AI development patterns
- [ ] GraphQL API patterns
- [ ] Microservices architecture guide
- [ ] Serverless patterns
- [ ] Web3/blockchain patterns

### Nice to Have
- [ ] More MCP server configurations
- [ ] Team-specific workflow examples
- [ ] Industry-specific patterns (fintech, healthcare, etc.)
- [ ] Accessibility guidelines
- [ ] Internationalization patterns
- [ ] Performance optimization guides

---

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thanks for contributing! Let's build a great resource together for the Antigravity community. 🚀
