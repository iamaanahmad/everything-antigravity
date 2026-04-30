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
