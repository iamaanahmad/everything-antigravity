# Antigravity Git Workflow Rules

**Mandatory git workflow guidelines for all code changes in Antigravity projects.**

## 1. Branch Strategy

### Branch Naming
- **Feature branches**: `feature/description` (e.g., `feature/add-semantic-search`)
- **Bug fixes**: `fix/description` (e.g., `fix/login-error`)
- **Hotfixes**: `hotfix/description` (e.g., `hotfix/security-patch`)
- **Refactoring**: `refactor/description` (e.g., `refactor/cleanup-utils`)

### Branch Protection
- **Never commit directly to `main` or `master`**
- Always create a feature branch for changes
- Delete branches after merging

## 2. Commit Messages

### Format
```
<type>(<scope>): <subject>

<body>

<footer>
```

### Types
- **feat**: New feature
- **fix**: Bug fix
- **docs**: Documentation changes
- **style**: Code style changes (formatting, no logic change)
- **refactor**: Code refactoring (no feature change)
- **test**: Adding or updating tests
- **chore**: Maintenance tasks (dependencies, config)
- **perf**: Performance improvements
- **ci**: CI/CD changes

### Examples

**Good commit messages:**
```
feat(search): add semantic search with Redis vector store

Implemented semantic search using OpenAI embeddings and Redis
vector search. Falls back to substring search if Redis unavailable.

Closes #123
```

```
fix(auth): resolve wallet connection timeout

Increased timeout from 5s to 15s to handle slow wallet responses.
Added retry logic for failed connections.

Fixes #456
```

```
refactor(utils): consolidate date formatting functions

Merged formatDate, formatDateTime, and formatTimestamp into single
utility function with options parameter.
```

**Bad commit messages:**
```
fix stuff
update code
changes
wip
asdf
```

### Commit Message Rules
- Use present tense: "add feature" not "added feature"
- Use imperative mood: "fix bug" not "fixes bug"
- First line max 72 characters
- Separate subject from body with blank line
- Reference issue numbers when applicable

## 3. Commit Frequency

### Atomic Commits
- Each commit should represent one logical change
- Don't mix unrelated changes in one commit
- Commit often, but keep commits meaningful

### When to Commit
- ✅ After completing a feature
- ✅ After fixing a bug
- ✅ After refactoring a module
- ✅ Before switching tasks
- ❌ Don't commit broken code
- ❌ Don't commit commented-out code
- ❌ Don't commit debug statements

## 4. Pull Requests

### PR Title Format
```
<type>: <description>
```

Examples:
- `feat: Add semantic search functionality`
- `fix: Resolve wallet connection timeout`
- `refactor: Clean up unused components`

### PR Description Template
```markdown
## Summary
Brief description of changes

## Changes
- Change 1
- Change 2
- Change 3

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests passing
- [ ] Manual testing completed

## Screenshots (if applicable)
[Add screenshots for UI changes]

## Related Issues
Closes #123
Relates to #456

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] No console.log statements
- [ ] Tests passing
```

### PR Size
- Keep PRs small and focused (< 500 lines changed)
- Large PRs are hard to review and more likely to have bugs
- Break large features into multiple PRs

### PR Review Process
1. Create PR with descriptive title and description
2. Request review from team members
3. Address review comments
4. Ensure CI/CD checks pass
5. Squash and merge (or rebase and merge)

## 5. Code Review

### As a Reviewer
- Review within 24 hours
- Be constructive and respectful
- Focus on logic, security, and maintainability
- Approve only if no critical issues
- Use review comments for suggestions

### As an Author
- Respond to all comments
- Don't take feedback personally
- Fix critical issues before merging
- Thank reviewers for their time

## 6. Merging Strategy

### Merge Methods
- **Squash and merge**: For feature branches (keeps history clean)
- **Rebase and merge**: For linear history
- **Merge commit**: For preserving branch history

### Before Merging
- [ ] All CI/CD checks passing
- [ ] Code review approved
- [ ] No merge conflicts
- [ ] Branch is up to date with main
- [ ] Tests passing locally

### After Merging
- Delete the feature branch
- Close related issues
- Deploy to staging/production if applicable

## 7. Git Hygiene

### What to Commit
- ✅ Source code
- ✅ Configuration files
- ✅ Documentation
- ✅ Tests
- ✅ Build scripts

### What NOT to Commit
- ❌ `node_modules/` or dependency directories
- ❌ `.env` files with secrets
- ❌ Build artifacts (`dist/`, `build/`)
- ❌ IDE-specific files (`.vscode/`, `.idea/`)
- ❌ OS-specific files (`.DS_Store`, `Thumbs.db`)
- ❌ Log files
- ❌ Temporary files

### .gitignore
Maintain a comprehensive `.gitignore`:
```
# Dependencies
node_modules/
.pnp/

# Testing
coverage/

# Production
build/
dist/
.next/

# Environment
.env
.env.local
.env.*.local

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Logs
*.log
npm-debug.log*
```

## 8. Handling Secrets

### Never Commit Secrets
- API keys
- Passwords
- Tokens
- Private keys
- Database credentials

### If You Accidentally Commit a Secret
1. **Immediately rotate the secret** (generate new key)
2. Remove from git history:
   ```bash
   git filter-branch --force --index-filter \
     "git rm --cached --ignore-unmatch path/to/file" \
     --prune-empty --tag-name-filter cat -- --all
   ```
3. Force push (if safe to do so)
4. Notify team members

### Use Environment Variables
```typescript
// ❌ Bad
const apiKey = "sk-abc123"

// ✅ Good
const apiKey = process.env.OPENAI_API_KEY
if (!apiKey) {
  throw new Error('OPENAI_API_KEY not configured')
}
```

## 9. Rebasing and History

### When to Rebase
- Before creating a PR (rebase on main)
- To clean up commit history
- To resolve conflicts

### Interactive Rebase
```bash
# Rebase last 3 commits
git rebase -i HEAD~3

# Options:
# pick - keep commit
# reword - change commit message
# squash - combine with previous commit
# drop - remove commit
```

### Never Rebase Public Branches
- Don't rebase commits that have been pushed to shared branches
- Don't rebase `main` or `master`
- Only rebase your own feature branches

## 10. Conflict Resolution

### Resolving Merge Conflicts
1. Pull latest changes from main
2. Resolve conflicts in your editor
3. Test the merged code
4. Commit the resolution
5. Push to your branch

### Avoiding Conflicts
- Pull from main frequently
- Keep PRs small and focused
- Communicate with team about overlapping work

## 11. Git Commands Reference

### Daily Workflow
```bash
# Create feature branch
git checkout -b feature/my-feature

# Stage changes
git add .

# Commit changes
git commit -m "feat: add new feature"

# Push to remote
git push origin feature/my-feature

# Update branch with main
git checkout main
git pull
git checkout feature/my-feature
git rebase main

# Squash last 3 commits
git rebase -i HEAD~3
```

### Useful Commands
```bash
# View commit history
git log --oneline --graph

# View changes
git diff

# Stash changes
git stash
git stash pop

# Undo last commit (keep changes)
git reset --soft HEAD~1

# Undo last commit (discard changes)
git reset --hard HEAD~1

# View remote branches
git branch -r

# Delete local branch
git branch -d feature/my-feature

# Delete remote branch
git push origin --delete feature/my-feature
```

## 12. CI/CD Integration

### Pre-commit Hooks
Use git hooks to enforce quality:
```bash
# .git/hooks/pre-commit
#!/bin/bash
npm run lint
npm test
```

### Pre-push Hooks
```bash
# .git/hooks/pre-push
#!/bin/bash
npm run build
npm test
```

### CI/CD Checks
Ensure these pass before merging:
- Linting (ESLint)
- Type checking (TypeScript)
- Unit tests
- Integration tests
- Build succeeds
- Security scan (npm audit)

## Enforcement

These rules are enforced by:
- Branch protection rules
- Required PR reviews
- CI/CD checks
- Code review process
- Antigravity code-reviewer agent

**Remember**: Good git hygiene makes collaboration easier and keeps the codebase maintainable. Follow these rules consistently.
