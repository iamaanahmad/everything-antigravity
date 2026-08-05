# Everything Antigravity Rules



## rules\agents.md

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


## rules\coding-style.md

# Antigravity Coding Style Rules

**Mandatory coding style guidelines for all code generated or reviewed in Antigravity.**

## 1. File Organization

### File Size Limits
- **Maximum file size**: 800 lines
- **Typical file size**: 200-400 lines
- **Reason**: Smaller files are easier to understand, test, and maintain

### File Naming
- Use `kebab-case` for file names: `user-profile.ts`, `market-card.tsx`
- Use `PascalCase` for component files: `UserProfile.tsx`, `MarketCard.tsx`
- Use descriptive names that reflect the file's purpose

### Directory Structure
- Group related files by feature or domain
- Keep directory depth reasonable (max 4-5 levels)
- Use index files to re-export public APIs

## 2. Naming Conventions

### Variables
- Use `camelCase` for variables: `userName`, `marketPrice`
- Use `UPPER_SNAKE_CASE` for constants: `MAX_RETRIES`, `API_ENDPOINT`
- Use descriptive names, avoid abbreviations: `user` not `u`, `market` not `mkt`

### Functions
- Use `camelCase` with verb prefixes: `calculateTotal`, `fetchUserData`, `validateInput`
- Boolean functions should start with `is`, `has`, `can`: `isValid`, `hasPermission`, `canEdit`

### Classes and Interfaces
- Use `PascalCase`: `UserProfile`, `MarketData`, `IUserRepository`
- Prefix interfaces with `I` (optional, depends on team preference)

### Types
- Use `PascalCase`: `UserType`, `MarketStatus`, `ApiResponse`

## 3. Code Structure

### Modularity
- Break down complex logic into small, reusable functions
- Each function should do one thing well (Single Responsibility Principle)
- Maximum function length: 50 lines (prefer 20-30 lines)

### Immutability
- Prefer immutable data structures
- Use spread operators instead of mutation:
  ```typescript
  // ❌ Bad - Mutation
  user.name = 'John'
  array.push(item)
  
  // ✅ Good - Immutable
  const updatedUser = { ...user, name: 'John' }
  const updatedArray = [...array, item]
  ```

### Nesting Depth
- Maximum nesting depth: 4 levels
- Use early returns to reduce nesting:
  ```typescript
  // ❌ Bad - Deep nesting
  if (user) {
    if (user.isActive) {
      if (user.hasPermission) {
        // Do something
      }
    }
  }
  
  // ✅ Good - Early returns
  if (!user) return
  if (!user.isActive) return
  if (!user.hasPermission) return
  // Do something
  ```

## 4. Documentation

### Comments
- Only comment complexity or non-obvious logic
- Avoid redundant comments that just repeat the code
- Use JSDoc for public APIs and exported functions

### JSDoc Format
```typescript
/**
 * Calculate the total price including tax
 * @param price - Base price before tax
 * @param taxRate - Tax rate as decimal (e.g., 0.1 for 10%)
 * @returns Total price including tax
 */
function calculateTotalPrice(price: number, taxRate: number): number {
  return price * (1 + taxRate)
}
```

### README Files
- Every significant module should have a README
- Include purpose, usage examples, and API documentation

## 5. Error Handling

### Specific Errors
- Throw specific error types where possible
- Include context in error messages:
  ```typescript
  // ❌ Bad
  throw new Error('Invalid input')
  
  // ✅ Good
  throw new Error(`Invalid market ID: ${marketId}. Expected format: UUID`)
  ```

### Try-Catch Blocks
- Catch errors at appropriate boundaries
- Log errors with sufficient context
- Don't swallow errors silently

## 6. Imports

### Import Organization
Group imports in this order:
1. Standard library imports
2. Third-party imports
3. Internal imports (absolute paths)
4. Relative imports

```typescript
// 1. Standard library
import * as fs from 'fs'

// 2. Third-party
import React from 'react'
import { supabase } from '@supabase/supabase-js'

// 3. Internal (absolute)
import { formatDate } from '@/lib/utils'

// 4. Relative
import { UserCard } from './UserCard'
```

### Import Style
- Use named imports when possible: `import { useState } from 'react'`
- Avoid wildcard imports unless necessary: `import * as utils from './utils'`

## 7. TypeScript Specific

### Type Annotations
- Always add type annotations for function parameters and return types
- Let TypeScript infer variable types when obvious
- Use `unknown` instead of `any` when type is truly unknown

### Interfaces vs Types
- Use `interface` for object shapes that might be extended
- Use `type` for unions, intersections, and primitives

### Avoid `any`
- Never use `any` unless absolutely necessary
- Use `unknown` and type guards instead

## 8. Formatting

### Indentation
- Use 2 spaces for indentation (not tabs)
- Consistent indentation throughout the file

### Line Length
- Maximum line length: 100 characters
- Break long lines at logical points

### Semicolons
- Use semicolons consistently (or omit consistently)
- Follow project's existing convention

### Trailing Commas
- Use trailing commas in multi-line arrays and objects
- Makes diffs cleaner when adding items

## 9. Prohibited Patterns

### ❌ Never Use
- `console.log` in production code (use proper logging)
- Emojis in code or comments
- Magic numbers without explanation
- Commented-out code (delete it, git remembers)
- `TODO` without a ticket reference
- `eval()` or similar unsafe functions

### ⚠️ Use Sparingly
- `any` type (prefer `unknown`)
- `@ts-ignore` (fix the type issue instead)
- Global variables
- Deeply nested ternaries

## 10. Best Practices

### DRY (Don't Repeat Yourself)
- Extract duplicate code into reusable functions
- Use utility functions for common operations

### KISS (Keep It Simple, Stupid)
- Prefer simple solutions over clever ones
- Code should be easy to understand

### YAGNI (You Aren't Gonna Need It)
- Don't add functionality until it's needed
- Avoid premature optimization

### Composition Over Inheritance
- Prefer composition and interfaces over class inheritance
- Use functional programming patterns when appropriate

## 11. Testing

### Test File Naming
- Test files should be named `*.test.ts` or `*.spec.ts`
- Place test files next to the code they test

### Test Structure
- Use `describe` blocks to group related tests
- Use descriptive test names: `it('should return user when ID is valid')`
- Follow Arrange-Act-Assert pattern

## 12. Performance

### Avoid Premature Optimization
- Write clear code first, optimize later if needed
- Measure before optimizing

### Be Mindful of Complexity
- Avoid O(n²) algorithms when O(n log n) is possible
- Use appropriate data structures (Map, Set, etc.)

## Enforcement

These rules are enforced by:
- ESLint configuration
- Prettier formatting
- Code review process
- Antigravity code-reviewer agent

**Remember**: Consistency is more important than personal preference. Follow the project's established patterns.


## rules\git-workflow.md

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


## rules\patterns.md

# Antigravity Common Patterns

**Reusable patterns and best practices for Antigravity projects.**

## API Response Format

### Standard Response Structure
```typescript
interface ApiResponse<T> {
  success: boolean
  data?: T
  error?: string
  meta?: {
    total?: number
    page?: number
    limit?: number
    timestamp?: string
  }
}
```

### Success Response
```typescript
return {
  success: true,
  data: {
    id: '123',
    name: 'John Doe'
  },
  meta: {
    timestamp: new Date().toISOString()
  }
}
```

### Error Response
```typescript
return {
  success: false,
  error: 'User not found',
  meta: {
    timestamp: new Date().toISOString()
  }
}
```

### Paginated Response
```typescript
return {
  success: true,
  data: items,
  meta: {
    total: 100,
    page: 1,
    limit: 20,
    timestamp: new Date().toISOString()
  }
}
```

## Custom Hooks Pattern

### Debounce Hook
```typescript
export function useDebounce<T>(value: T, delay: number): T {
  const [debouncedValue, setDebouncedValue] = useState<T>(value)

  useEffect(() => {
    const handler = setTimeout(() => {
      setDebouncedValue(value)
    }, delay)

    return () => {
      clearTimeout(handler)
    }
  }, [value, delay])

  return debouncedValue
}
```

### Local Storage Hook
```typescript
export function useLocalStorage<T>(key: string, initialValue: T) {
  const [storedValue, setStoredValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key)
      return item ? JSON.parse(item) : initialValue
    } catch (error) {
      return initialValue
    }
  })

  const setValue = (value: T | ((val: T) => T)) => {
    try {
      const valueToStore = value instanceof Function ? value(storedValue) : value
      setStoredValue(valueToStore)
      window.localStorage.setItem(key, JSON.stringify(valueToStore))
    } catch (error) {
      console.error('Error saving to localStorage:', error)
    }
  }

  return [storedValue, setValue] as const
}
```

### Async Data Hook
```typescript
export function useAsync<T>(asyncFunction: () => Promise<T>) {
  const [data, setData] = useState<T | null>(null)
  const [loading, setLoading] = useState(true)
  const [error, setError] = useState<Error | null>(null)

  useEffect(() => {
    let isMounted = true

    asyncFunction()
      .then(result => {
        if (isMounted) {
          setData(result)
          setLoading(false)
        }
      })
      .catch(err => {
        if (isMounted) {
          setError(err)
          setLoading(false)
        }
      })

    return () => {
      isMounted = false
    }
  }, [])

  return { data, loading, error }
}
```

## Repository Pattern

### Interface Definition
```typescript
interface Repository<T> {
  findAll(filters?: Filters): Promise<T[]>
  findById(id: string): Promise<T | null>
  create(data: CreateDto): Promise<T>
  update(id: string, data: UpdateDto): Promise<T>
  delete(id: string): Promise<void>
}
```

### Implementation Example
```typescript
export class UserRepository implements Repository<User> {
  async findAll(filters?: UserFilters): Promise<User[]> {
    let query = supabase.from('users').select('*')
    
    if (filters?.isActive) {
      query = query.eq('is_active', true)
    }
    
    const { data, error } = await query
    if (error) throw error
    return data
  }

  async findById(id: string): Promise<User | null> {
    const { data, error } = await supabase
      .from('users')
      .select('*')
      .eq('id', id)
      .single()
    
    if (error) return null
    return data
  }

  async create(data: CreateUserDto): Promise<User> {
    const { data: user, error } = await supabase
      .from('users')
      .insert(data)
      .select()
      .single()
    
    if (error) throw error
    return user
  }

  async update(id: string, data: UpdateUserDto): Promise<User> {
    const { data: user, error } = await supabase
      .from('users')
      .update(data)
      .eq('id', id)
      .select()
      .single()
    
    if (error) throw error
    return user
  }

  async delete(id: string): Promise<void> {
    const { error } = await supabase
      .from('users')
      .delete()
      .eq('id', id)
    
    if (error) throw error
  }
}
```

## Service Layer Pattern

### Service Interface
```typescript
export class UserService {
  constructor(private userRepository: UserRepository) {}

  async getActiveUsers(): Promise<User[]> {
    return this.userRepository.findAll({ isActive: true })
  }

  async getUserById(id: string): Promise<User> {
    const user = await this.userRepository.findById(id)
    if (!user) {
      throw new Error(`User not found: ${id}`)
    }
    return user
  }

  async createUser(data: CreateUserDto): Promise<User> {
    // Business logic here
    this.validateUserData(data)
    return this.userRepository.create(data)
  }

  private validateUserData(data: CreateUserDto): void {
    if (!data.email.includes('@')) {
      throw new Error('Invalid email format')
    }
  }
}
```

## Error Handling Pattern

### Custom Error Classes
```typescript
export class AppError extends Error {
  constructor(
    public message: string,
    public statusCode: number = 500,
    public isOperational: boolean = true
  ) {
    super(message)
    Object.setPrototypeOf(this, AppError.prototype)
  }
}

export class NotFoundError extends AppError {
  constructor(message: string) {
    super(message, 404)
  }
}

export class ValidationError extends AppError {
  constructor(message: string) {
    super(message, 400)
  }
}

export class UnauthorizedError extends AppError {
  constructor(message: string = 'Unauthorized') {
    super(message, 401)
  }
}
```

### Error Handler Middleware
```typescript
export function errorHandler(
  error: Error,
  req: Request,
  res: Response,
  next: NextFunction
) {
  if (error instanceof AppError) {
    return res.status(error.statusCode).json({
      success: false,
      error: error.message
    })
  }

  // Log unexpected errors
  console.error('Unexpected error:', error)

  return res.status(500).json({
    success: false,
    error: 'Internal server error'
  })
}
```

## Validation Pattern

### Zod Schema Validation
```typescript
import { z } from 'zod'

export const CreateUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(2).max(100),
  age: z.number().int().min(18).max(120).optional()
})

export type CreateUserDto = z.infer<typeof CreateUserSchema>

// Usage in API route
export async function POST(req: Request) {
  try {
    const body = await req.json()
    const validatedData = CreateUserSchema.parse(body)
    
    // Use validatedData (type-safe)
    const user = await createUser(validatedData)
    
    return Response.json({ success: true, data: user })
  } catch (error) {
    if (error instanceof z.ZodError) {
      return Response.json(
        { success: false, error: error.errors },
        { status: 400 }
      )
    }
    throw error
  }
}
```

## Caching Pattern

### Simple In-Memory Cache
```typescript
export class Cache<T> {
  private cache = new Map<string, { value: T; expiry: number }>()

  set(key: string, value: T, ttlMs: number): void {
    const expiry = Date.now() + ttlMs
    this.cache.set(key, { value, expiry })
  }

  get(key: string): T | null {
    const item = this.cache.get(key)
    if (!item) return null

    if (Date.now() > item.expiry) {
      this.cache.delete(key)
      return null
    }

    return item.value
  }

  delete(key: string): void {
    this.cache.delete(key)
  }

  clear(): void {
    this.cache.clear()
  }
}
```

### Redis Cache Pattern
```typescript
export class RedisCache {
  constructor(private redis: Redis) {}

  async get<T>(key: string): Promise<T | null> {
    const value = await this.redis.get(key)
    return value ? JSON.parse(value) : null
  }

  async set<T>(key: string, value: T, ttlSeconds: number): Promise<void> {
    await this.redis.setex(key, ttlSeconds, JSON.stringify(value))
  }

  async delete(key: string): Promise<void> {
    await this.redis.del(key)
  }
}
```

## Retry Pattern

### Exponential Backoff Retry
```typescript
export async function retryWithBackoff<T>(
  fn: () => Promise<T>,
  maxRetries: number = 3,
  baseDelayMs: number = 1000
): Promise<T> {
  let lastError: Error

  for (let attempt = 0; attempt < maxRetries; attempt++) {
    try {
      return await fn()
    } catch (error) {
      lastError = error as Error
      
      if (attempt < maxRetries - 1) {
        const delay = baseDelayMs * Math.pow(2, attempt)
        await new Promise(resolve => setTimeout(resolve, delay))
      }
    }
  }

  throw lastError!
}
```

## Skeleton Projects Pattern

When implementing new functionality:

### 1. Search for Battle-Tested Skeletons
```
User: I need to implement OAuth authentication

Antigravity: Let me search for proven OAuth implementations...
[Searches GitHub, npm, documentation]
```

### 2. Evaluate Options with Parallel Agents
```
Use multiple agents to assess:
- Security assessment (security-reviewer agent)
- Extensibility analysis (architect agent)
- Relevance scoring (planner agent)
- Implementation planning (planner agent)
```

### 3. Clone Best Match as Foundation
```bash
# Clone the skeleton project
git clone https://github.com/example/oauth-skeleton.git

# Extract relevant parts
cp -r oauth-skeleton/src/auth ./src/
```

### 4. Iterate Within Proven Structure
```
- Keep the proven architecture
- Customize for your needs
- Add tests
- Document changes
```

## Immutability Pattern

### Spread Operators
```typescript
// ❌ Bad - Mutation
user.name = 'John'
array.push(item)
object.property = value

// ✅ Good - Immutable
const updatedUser = { ...user, name: 'John' }
const updatedArray = [...array, item]
const updatedObject = { ...object, property: value }
```

### Array Operations
```typescript
// Add item
const newArray = [...oldArray, newItem]

// Remove item
const filtered = oldArray.filter(item => item.id !== removeId)

// Update item
const updated = oldArray.map(item =>
  item.id === updateId ? { ...item, name: 'New Name' } : item
)

// Sort (without mutating)
const sorted = [...oldArray].sort((a, b) => a.value - b.value)
```

## Best Practices

### DO
- ✅ Use these patterns consistently
- ✅ Adapt patterns to your needs
- ✅ Document deviations from patterns
- ✅ Share new patterns with team
- ✅ Keep patterns simple and focused

### DON'T
- ❌ Blindly follow patterns without understanding
- ❌ Over-engineer simple solutions
- ❌ Mix multiple patterns inconsistently
- ❌ Ignore project-specific requirements
- ❌ Create patterns for one-off use cases

**Remember**: Patterns are guidelines, not rules. Use them to solve problems, not to create complexity.


## rules\performance.md

# Antigravity Performance Optimization

**Performance guidelines for Antigravity projects and AI-assisted development.**

## Model Selection Strategy

Choose the right Gemini model for the task:

### Gemini 2.0 Flash (Fast & Efficient)
**Use for:**
- Lightweight agents with frequent invocation
- Pair programming and code generation
- Worker agents in multi-agent systems
- Simple refactoring tasks
- Documentation updates
- Code reviews (non-security)

**Characteristics:**
- Fast response time
- Lower cost
- Good for 90% of tasks
- Suitable for iterative workflows

### Gemini 2.0 Pro Exp (Balanced)
**Use for:**
- Main development work
- Orchestrating multi-agent workflows
- Complex coding tasks
- Architectural decisions
- Security reviews
- Test generation

**Characteristics:**
- Best coding model
- Balanced speed and capability
- Default choice for most agents
- Good reasoning ability

### Gemini 2.0 Ultra (Deep Reasoning)
**Use for:**
- Complex architectural decisions
- Maximum reasoning requirements
- Research and analysis tasks
- Novel problem solving
- System design
- Critical security analysis

**Characteristics:**
- Deepest reasoning
- Slower response time
- Higher cost
- Use sparingly for complex tasks

## Context Window Management

### Avoid Last 20% of Context Window

**High context sensitivity tasks:**
- Large-scale refactoring
- Feature implementation spanning multiple files
- Debugging complex interactions
- Multi-file code generation

**Strategy:**
- Monitor context usage
- Break large tasks into smaller chunks
- Use subagents for isolated work
- Clear context when approaching limit

### Lower Context Sensitivity Tasks

**Safe in high context usage:**
- Single-file edits
- Independent utility creation
- Documentation updates
- Simple bug fixes
- Code formatting

## Enhanced Thinking Mode

For complex tasks requiring deep reasoning:

### 1. Use Enhanced Thinking
```
User: [Enable enhanced thinking mode]
I need to design a distributed caching system
```

### 2. Enable Plan Mode
```
User: [Plan mode enabled]
Break down the implementation into phases
```

### 3. "Rev the Engine"
```
Multiple critique rounds:
1. Initial design
2. Security review
3. Performance analysis
4. Scalability assessment
5. Final refinement
```

### 4. Split Role Sub-Agents
```
Use diverse agents for analysis:
- Architect agent: System design
- Security reviewer: Vulnerability analysis
- Performance optimizer: Bottleneck identification
- Planner: Implementation strategy
```

## Build Troubleshooting

### Incremental Error Fixing

**Use build-error-resolver agent:**

```
User: Build is failing with TypeScript errors

Antigravity: [Invokes build-error-resolver agent]

Build Error Resolver:
1. Collecting all errors...
2. Categorizing by severity...
3. Fixing errors one by one...
4. Verifying after each fix...
```

**Strategy:**
1. Fix one error at a time
2. Verify build after each fix
3. Don't introduce new errors
4. Minimal diffs only

## Code Performance

### Algorithm Complexity

**Be mindful of Big O:**
```typescript
// ❌ Bad - O(n²)
for (const item1 of array1) {
  for (const item2 of array2) {
    if (item1.id === item2.id) {
      // Do something
    }
  }
}

// ✅ Good - O(n)
const map = new Map(array2.map(item => [item.id, item]))
for (const item1 of array1) {
  const item2 = map.get(item1.id)
  if (item2) {
    // Do something
  }
}
```

### Use Appropriate Data Structures

```typescript
// For lookups: Use Map or Set
const userMap = new Map(users.map(u => [u.id, u]))
const activeIds = new Set(activeUsers.map(u => u.id))

// For ordered data: Use Array
const sortedItems = items.sort((a, b) => a.priority - b.priority)

// For unique values: Use Set
const uniqueValues = new Set(values)
```

### Memoization

```typescript
// Expensive calculation
const memoizedCalculation = useMemo(() => {
  return expensiveOperation(data)
}, [data])

// Expensive component
const MemoizedComponent = memo(({ data }) => {
  return <ExpensiveRender data={data} />
})
```

## Database Performance

### Query Optimization

```typescript
// ❌ Bad - N+1 queries
const users = await getUsers()
for (const user of users) {
  user.posts = await getPosts(user.id) // N queries
}

// ✅ Good - Single query with join
const users = await supabase
  .from('users')
  .select('*, posts(*)')
```

### Indexing

```sql
-- Add indexes for frequently queried columns
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_created_at ON posts(created_at DESC);
```

### Pagination

```typescript
// Always paginate large datasets
const { data, error } = await supabase
  .from('posts')
  .select('*')
  .range(0, 19) // First 20 items
  .order('created_at', { ascending: false })
```

## Frontend Performance

### Code Splitting

```typescript
// Lazy load routes
const Dashboard = lazy(() => import('./pages/Dashboard'))
const Settings = lazy(() => import('./pages/Settings'))

// Lazy load heavy components
const Chart = lazy(() => import('./components/Chart'))
```

### Image Optimization

```typescript
// Use Next.js Image component
import Image from 'next/image'

<Image
  src="/hero.jpg"
  alt="Hero"
  width={1200}
  height={600}
  priority // For above-the-fold images
/>
```

### Debouncing

```typescript
// Debounce search input
const debouncedSearch = useDebounce(searchQuery, 300)

useEffect(() => {
  if (debouncedSearch) {
    performSearch(debouncedSearch)
  }
}, [debouncedSearch])
```

## Bundle Size Optimization

### Tree Shaking

```typescript
// ❌ Bad - Imports entire library
import _ from 'lodash'

// ✅ Good - Import only what you need
import debounce from 'lodash/debounce'
import throttle from 'lodash/throttle'
```

### Dynamic Imports

```typescript
// Load heavy library only when needed
async function processData(data) {
  const { default: heavyLib } = await import('heavy-library')
  return heavyLib.process(data)
}
```

### Analyze Bundle

```bash
# Analyze bundle size
npm run build -- --analyze

# Check what's in the bundle
npx webpack-bundle-analyzer dist/stats.json
```

## Caching Strategies

### API Response Caching

```typescript
// Cache API responses
const cache = new Map()

async function fetchWithCache(url: string, ttl: number = 60000) {
  const cached = cache.get(url)
  if (cached && Date.now() - cached.timestamp < ttl) {
    return cached.data
  }

  const data = await fetch(url).then(r => r.json())
  cache.set(url, { data, timestamp: Date.now() })
  return data
}
```

### Redis Caching

```typescript
// Cache expensive operations in Redis
async function getMarkets() {
  const cached = await redis.get('markets')
  if (cached) return JSON.parse(cached)

  const markets = await fetchMarketsFromDB()
  await redis.setex('markets', 300, JSON.stringify(markets)) // 5 min TTL
  return markets
}
```

## Monitoring and Profiling

### Performance Monitoring

```typescript
// Measure operation time
console.time('operation')
await expensiveOperation()
console.timeEnd('operation')

// Use Performance API
const start = performance.now()
await operation()
const duration = performance.now() - start
console.log(`Operation took ${duration}ms`)
```

### React Profiler

```typescript
import { Profiler } from 'react'

<Profiler id="Dashboard" onRender={onRenderCallback}>
  <Dashboard />
</Profiler>

function onRenderCallback(
  id, phase, actualDuration, baseDuration, startTime, commitTime
) {
  console.log(`${id} took ${actualDuration}ms`)
}
```

## Agent Performance

### Parallel Agent Execution

```
When possible, run agents in parallel:

User: Review security and performance

Antigravity:
[Invokes security-reviewer agent] (parallel)
[Invokes performance analyzer] (parallel)

Results combined after both complete
```

### Agent Context Optimization

```
Keep agent contexts focused:
- Provide only relevant files
- Use specific file paths
- Avoid loading entire codebase
- Clear context between tasks
```

## Best Practices

### DO
- ✅ Choose appropriate model for task
- ✅ Monitor context window usage
- ✅ Use memoization for expensive operations
- ✅ Optimize database queries
- ✅ Implement caching strategies
- ✅ Lazy load heavy components
- ✅ Profile before optimizing
- ✅ Measure performance improvements

### DON'T
- ❌ Use Ultra model for simple tasks
- ❌ Ignore context window limits
- ❌ Premature optimization
- ❌ Load entire datasets without pagination
- ❌ Skip caching for expensive operations
- ❌ Import entire libraries
- ❌ Optimize without measuring
- ❌ Sacrifice readability for micro-optimizations

## Performance Checklist

Before deploying:
- [ ] Database queries optimized
- [ ] Indexes added for frequent queries
- [ ] API responses cached
- [ ] Images optimized
- [ ] Code split by route
- [ ] Heavy libraries lazy loaded
- [ ] Bundle size analyzed
- [ ] Performance metrics measured
- [ ] No N+1 queries
- [ ] Pagination implemented

**Remember**: Premature optimization is the root of all evil. Measure first, optimize second. Use the right tool (model) for the job.


## rules\security.md

# Antigravity Security Rules

**Mandatory security guidelines for all code generated or reviewed in Antigravity.**

## 1. Secrets Management
- **NEVER** hardcode secrets (API keys, passwords, tokens) in code.
- Use environment variables (e.g., `.env`) for all sensitive data.
- Ensure `.env` is added to `.gitignore`.
- Scan committed code for accidental secret leaks.

## 2. Input Validation
- Validate ALL user inputs on both client and server sides.
- Sanitize data before using it in SQL queries, shell commands, or HTML rendering.
- Use parameterized queries or ORMs to prevent SQL injection.
- Encode output to prevent Cross-Site Scripting (XSS).

## 3. Authentication & Authorization
- Implement robust authentication (e.g., OAuth, JWT).
- Enforce strict authorization checks (RBAC/ABAC) on every sensitive endpoint.
- Use secure session management (HttpOnly, Secure cookies).
- Implement rate limiting to prevent brute-force attacks.

## 4. dependency Management
- Regularly update dependencies to patch known vulnerabilities.
- Audit `package.json` (or equivalent) for insecure packages.
- Pin dependency versions to avoid unexpected breaking changes or malicious updates.

## 5. Data Protection
- Encrypt sensitive data at rest and in transit (HTTPS/TLS).
- minimize data collection (Privacy by Design).
- Implement proper logging for security audits (without logging sensitive data).

## 6. Error Handling
- Do not expose stack traces or internal system details to end-users.
- Log errors securely for debugging purposes.
- Fail securely (e.g., deny access by default).


## rules\testing.md

# Antigravity Testing Requirements

**Mandatory testing guidelines for all code in Antigravity projects.**

## Minimum Test Coverage: 80%

All code must maintain at least 80% test coverage across:
- Branches: 80%
- Functions: 80%
- Lines: 80%
- Statements: 80%

## Test Types (ALL Required)

### 1. Unit Tests
- Test individual functions, utilities, components in isolation
- Mock external dependencies
- Fast execution (< 1ms per test)
- Located next to source files: `utils.test.ts` next to `utils.ts`

### 2. Integration Tests
- Test API endpoints and database operations
- Test component integration
- Use real dependencies when possible
- Located in `tests/integration/`

### 3. E2E Tests (Critical Flows Only)
- Test complete user journeys
- Use Playwright for browser automation
- Test critical paths (auth, payments, core features)
- Located in `tests/e2e/`

## Test-Driven Development (MANDATORY)

**You MUST follow this workflow:**

### Step 1: Write Test First (RED)
```typescript
describe('calculateTotal', () => {
  it('should calculate total with tax', () => {
    const result = calculateTotal(100, 0.1)
    expect(result).toBe(110)
  })
})
```

### Step 2: Run Test - It Should FAIL
```bash
npm test
# ❌ Test fails - function doesn't exist yet
```

### Step 3: Write Minimal Implementation (GREEN)
```typescript
export function calculateTotal(price: number, taxRate: number): number {
  return price * (1 + taxRate)
}
```

### Step 4: Run Test - It Should PASS
```bash
npm test
# ✅ Test passes
```

### Step 5: Refactor (IMPROVE)
- Remove duplication
- Improve names
- Optimize performance
- Keep tests passing

### Step 6: Verify Coverage (80%+)
```bash
npm run test:coverage
# Check coverage report
```

## Test Structure

### Arrange-Act-Assert Pattern
```typescript
it('should do something', () => {
  // Arrange - Set up test data
  const input = { value: 42 }
  
  // Act - Execute the function
  const result = processInput(input)
  
  // Assert - Verify the result
  expect(result.processed).toBe(true)
  expect(result.value).toBe(42)
})
```

### Descriptive Test Names
```typescript
// ❌ Bad
it('works', () => { ... })
it('test 1', () => { ... })

// ✅ Good
it('should return user when ID is valid', () => { ... })
it('should throw error when ID is missing', () => { ... })
it('should handle concurrent requests correctly', () => { ... })
```

## What to Test

### Always Test
- ✅ Public APIs and exported functions
- ✅ Business logic and calculations
- ✅ Error handling and edge cases
- ✅ User input validation
- ✅ Authentication and authorization
- ✅ Data transformations
- ✅ API endpoints
- ✅ Critical user flows

### Don't Test
- ❌ Third-party libraries (trust they're tested)
- ❌ Framework internals
- ❌ Trivial getters/setters
- ❌ Configuration files
- ❌ Type definitions

## Edge Cases to Test

**ALWAYS test these scenarios:**

1. **Null/Undefined**
   ```typescript
   it('should handle null input', () => {
     expect(() => processData(null)).toThrow()
   })
   ```

2. **Empty Values**
   ```typescript
   it('should handle empty array', () => {
     expect(processArray([])).toEqual([])
   })
   ```

3. **Boundary Values**
   ```typescript
   it('should handle maximum value', () => {
     expect(validate(Number.MAX_SAFE_INTEGER)).toBe(true)
   })
   ```

4. **Invalid Types**
   ```typescript
   it('should reject invalid type', () => {
     expect(() => processNumber('not a number')).toThrow()
   })
   ```

5. **Error Conditions**
   ```typescript
   it('should handle network error', async () => {
     mockFetch.mockRejectedValue(new Error('Network error'))
     await expect(fetchData()).rejects.toThrow('Network error')
   })
   ```

## Mocking External Dependencies

### Mock API Calls
```typescript
jest.mock('@/lib/api', () => ({
  fetchUser: jest.fn(() => Promise.resolve({ id: 1, name: 'John' }))
}))
```

### Mock Database
```typescript
jest.mock('@/lib/supabase', () => ({
  supabase: {
    from: jest.fn(() => ({
      select: jest.fn(() => Promise.resolve({ data: mockData, error: null }))
    }))
  }
}))
```

### Mock Environment Variables
```typescript
beforeEach(() => {
  process.env.API_KEY = 'test-key'
})

afterEach(() => {
  delete process.env.API_KEY
})
```

## Test Isolation

### Each Test Should Be Independent
```typescript
// ❌ Bad - Tests depend on each other
let user
test('creates user', () => {
  user = createUser()
})
test('updates user', () => {
  updateUser(user) // Depends on previous test
})

// ✅ Good - Tests are independent
test('creates user', () => {
  const user = createUser()
  expect(user).toBeDefined()
})
test('updates user', () => {
  const user = createUser() // Create fresh user
  updateUser(user)
  expect(user.updated).toBe(true)
})
```

### Clean Up After Tests
```typescript
afterEach(() => {
  jest.clearAllMocks()
  cleanup() // Clean up DOM, timers, etc.
})
```

## Coverage Requirements

### Standard Coverage: 80%
All code must meet 80% coverage minimum.

### Critical Code: 100%
These areas require 100% coverage:
- Financial calculations
- Authentication logic
- Authorization checks
- Security-critical code
- Payment processing
- Data validation

### Verify Coverage
```bash
# Run tests with coverage
npm run test:coverage

# View HTML report
open coverage/lcov-report/index.html

# Check coverage thresholds
npm run test:coverage -- --coverage-threshold='{"global":{"branches":80,"functions":80,"lines":80,"statements":80}}'
```

## Troubleshooting Test Failures

### 1. Use tdd-guide Agent
```
User: My tests are failing

Antigravity: [Invokes tdd-guide agent to help debug]
```

### 2. Check Test Isolation
- Are tests sharing state?
- Are mocks being cleared between tests?
- Are timers or async operations completing?

### 3. Verify Mocks
- Are mocks returning expected values?
- Are mocks being called with correct arguments?
- Are all external dependencies mocked?

### 4. Fix Implementation, Not Tests
- If tests fail, the implementation is usually wrong
- Only fix tests if they're testing the wrong thing
- Don't make tests pass by lowering expectations

## Test Performance

### Keep Tests Fast
- Unit tests: < 1ms each
- Integration tests: < 100ms each
- E2E tests: < 5s each

### Parallelize Tests
```bash
# Run tests in parallel
npm test -- --maxWorkers=4
```

### Skip Slow Tests in Watch Mode
```typescript
test.skip('slow integration test', async () => {
  // Only run in CI
})
```

## Continuous Integration

### Pre-commit Hook
```bash
# .git/hooks/pre-commit
#!/bin/bash
npm test
```

### CI/CD Pipeline
```yaml
# .github/workflows/test.yml
- name: Run tests
  run: npm test -- --coverage --ci

- name: Upload coverage
  uses: codecov/codecov-action@v3
```

## Agent Support

### tdd-guide Agent
Use PROACTIVELY for:
- Writing new features
- Fixing bugs
- Refactoring code
- Ensuring test coverage

```
User: Implement user authentication

Antigravity: [Invokes tdd-guide agent]
TDD Guide: Let's start with tests first...
```

### e2e-runner Agent
Use for:
- Critical user journeys
- Integration testing
- Browser automation
- Visual regression testing

```
User: Test the checkout flow

Antigravity: [Invokes e2e-runner agent]
E2E Runner: Creating Playwright test for checkout...
```

## Best Practices

### DO
- ✅ Write tests before implementation (TDD)
- ✅ Test behavior, not implementation
- ✅ Use descriptive test names
- ✅ Keep tests simple and focused
- ✅ Mock external dependencies
- ✅ Test edge cases and errors
- ✅ Maintain 80%+ coverage
- ✅ Run tests before committing

### DON'T
- ❌ Skip writing tests
- ❌ Write tests after implementation
- ❌ Test implementation details
- ❌ Share state between tests
- ❌ Ignore failing tests
- ❌ Lower coverage to make tests pass
- ❌ Commit code without tests
- ❌ Mock everything (prefer integration tests)

## Enforcement

These testing requirements are enforced by:
- Pre-commit hooks
- CI/CD pipeline
- Code review process
- Antigravity tdd-guide agent
- Coverage reports

**Remember**: Tests are not optional. They are the safety net that enables confident refactoring, rapid development, and production reliability. No code without tests!
