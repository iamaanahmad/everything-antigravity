# Installation Guide - Everything Antigravity

Quick guide to install and configure Everything Antigravity in your project.

## Prerequisites

- Google Antigravity IDE installed
- Git installed
- Node.js and npm (for MCP servers)

## Installation Methods

### Method 1: Clone Repository (Recommended)

```bash
# Clone the repository
git clone https://github.com/iamaanahmad/everything-antigravity.git

# Navigate to the directory
cd everything-antigravity
```

### Method 2: Download ZIP

Download the ZIP file from GitHub and extract it to your desired location.

## Configuration Steps

### 1. Install Agents

Copy agents to your Antigravity agents directory:

```bash
# Copy all agents
cp agents/*.md ~/.antigravity/agents/

# Or copy specific agents
cp agents/planner.md ~/.antigravity/agents/
cp agents/code-reviewer.md ~/.antigravity/agents/
cp agents/tdd-guide.md ~/.antigravity/agents/
```

**Available Agents:**
- `planner.md` - Feature planning
- `architect.md` - System design
- `code-reviewer.md` - Code review
- `tdd-guide.md` - Test-driven development
- `security-reviewer.md` - Security review
- `build-error-resolver.md` - Build error fixing
- `e2e-runner.md` - E2E testing
- `refactor-cleaner.md` - Code cleanup
- `doc-updater.md` - Documentation

### 2. Install Rules

Copy rules to your Antigravity rules directory:

```bash
# Copy all rules
cp rules/*.md ~/.antigravity/rules/

# Or copy specific rules
cp rules/security.md ~/.antigravity/rules/
cp rules/coding-style.md ~/.antigravity/rules/
cp rules/git-workflow.md ~/.antigravity/rules/
```

**Available Rules:**
- `security.md` - Security guidelines
- `coding-style.md` - Coding conventions
- `git-workflow.md` - Git workflow

### 3. Install Commands

Copy commands to your Antigravity commands directory:

```bash
# Copy all commands
cp commands/*.md ~/.antigravity/commands/

# Or copy specific commands
cp commands/tdd.md ~/.antigravity/commands/
cp commands/plan.md ~/.antigravity/commands/
cp commands/code-review.md ~/.antigravity/commands/
```

**Available Commands:**
- `/tdd` - Test-driven development
- `/plan` - Implementation planning
- `/code-review` - Code review

### 4. Install Skills

Copy skills to your Antigravity skills directory:

```bash
# Copy all skills
cp -r skills/* ~/.antigravity/skills/
```

**Available Skills:**
- `coding-standards/` - Language best practices

### 5. Install Contexts

Copy contexts to your Antigravity contexts directory:

```bash
# Copy all contexts
cp contexts/*.md ~/.antigravity/contexts/

# Or copy specific contexts
cp contexts/dev.md ~/.antigravity/contexts/
cp contexts/research.md ~/.antigravity/contexts/
cp contexts/review.md ~/.antigravity/contexts/
```

**Available Contexts:**
- `dev.md` - Development mode
- `research.md` - Research mode
- `review.md` - Review mode

### 6. Configure MCP Servers

Copy MCP configuration and update with your API keys:

```bash
# Copy MCP config
cp mcp-configs/mcp-servers.json ~/.antigravity/mcp-servers.json

# Edit the file and replace placeholders
nano ~/.antigravity/mcp-servers.json
```

**Replace these placeholders:**
- `YOUR_GITHUB_PAT_HERE` - Your GitHub Personal Access Token
- `YOUR_FIRECRAWL_KEY_HERE` - Your Firecrawl API key
- `YOUR_PROJECT_REF` - Your Supabase project reference
- `/path/to/your/projects` - Your projects directory path

**Important:** Never commit files with real API keys!

## Verification

### Check Installation

Verify that files are in the correct locations:

```bash
# Check agents
ls ~/.antigravity/agents/

# Check rules
ls ~/.antigravity/rules/

# Check commands
ls ~/.antigravity/commands/

# Check skills
ls ~/.antigravity/skills/

# Check contexts
ls ~/.antigravity/contexts/

# Check MCP config
cat ~/.antigravity/mcp-servers.json
```

### Test Agents

Open Antigravity and try invoking an agent:

```
User: Can you help me plan a new feature?

Antigravity: [Should invoke planner agent]
```

### Test Commands

Try using a command:

```
User: /plan Add user authentication

Antigravity: [Should invoke planner agent with planning workflow]
```

## Project-Specific Configuration

For project-specific configurations, create a `.antigravity/` directory in your project root:

```bash
# Create project config directory
mkdir -p .antigravity

# Copy project-specific configs
cp agents/planner.md .antigravity/agents/
cp rules/security.md .antigravity/rules/
```

Project-specific configs override global configs.

## Customization

### Customize Agents

Edit agent files to match your workflow:

```bash
# Edit planner agent
nano ~/.antigravity/agents/planner.md
```

### Customize Rules

Add project-specific rules:

```bash
# Create custom rule
nano ~/.antigravity/rules/team-conventions.md
```

### Customize Skills

Add domain-specific skills:

```bash
# Create custom skill
mkdir -p ~/.antigravity/skills/my-domain
nano ~/.antigravity/skills/my-domain/SKILL.md
```

## Troubleshooting

### Agents Not Working

**Problem:** Agents are not being invoked

**Solutions:**
1. Check that agent files are in `~/.antigravity/agents/`
2. Verify file permissions: `chmod 644 ~/.antigravity/agents/*.md`
3. Restart Antigravity IDE
4. Check Antigravity logs for errors

### Commands Not Working

**Problem:** Commands like `/plan` are not recognized

**Solutions:**
1. Check that command files are in `~/.antigravity/commands/`
2. Verify file format (must have `---` frontmatter)
3. Restart Antigravity IDE

### MCP Servers Not Connecting

**Problem:** MCP servers fail to connect

**Solutions:**
1. Verify API keys are correct (no placeholders)
2. Check internet connection
3. Verify MCP server packages are installed: `npm install -g @modelcontextprotocol/server-*`
4. Check Antigravity MCP logs

### Rules Not Being Enforced

**Problem:** Rules are not being followed

**Solutions:**
1. Check that rule files are in `~/.antigravity/rules/`
2. Verify rules are written clearly and specifically
3. Rules are guidelines, not hard constraints
4. Use code-reviewer agent to enforce rules

## Updating

To update to the latest version:

```bash
# Navigate to repository
cd everything-antigravity

# Pull latest changes
git pull origin main

# Re-copy files
cp agents/*.md ~/.antigravity/agents/
cp rules/*.md ~/.antigravity/rules/
cp commands/*.md ~/.antigravity/commands/
cp -r skills/* ~/.antigravity/skills/
cp contexts/*.md ~/.antigravity/contexts/
```

## Uninstallation

To remove Everything Antigravity:

```bash
# Remove agents
rm ~/.antigravity/agents/planner.md
rm ~/.antigravity/agents/architect.md
# ... (remove other agents)

# Remove rules
rm ~/.antigravity/rules/security.md
rm ~/.antigravity/rules/coding-style.md
rm ~/.antigravity/rules/git-workflow.md

# Remove commands
rm ~/.antigravity/commands/tdd.md
rm ~/.antigravity/commands/plan.md
rm ~/.antigravity/commands/code-review.md

# Remove skills
rm -rf ~/.antigravity/skills/coding-standards

# Remove contexts
rm ~/.antigravity/contexts/dev.md
rm ~/.antigravity/contexts/research.md
rm ~/.antigravity/contexts/review.md

# Remove MCP config (optional)
rm ~/.antigravity/mcp-servers.json
```

## Getting Help

- **Documentation:** See [README.md](README.md) for detailed usage
- **Issues:** Report issues on GitHub
- **Discussions:** Join community discussions

## Next Steps

After installation:

1. Read the [README.md](README.md) for usage examples
2. Try the `/plan` command for your next feature
3. Use `/code-review` before committing code
4. Customize agents and rules for your workflow
5. Share your improvements with the community

---

**Installation complete!** 🎉

Start using Everything Antigravity to supercharge your development workflow.
