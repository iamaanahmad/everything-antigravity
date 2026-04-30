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
