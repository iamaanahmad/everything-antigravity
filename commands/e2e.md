---
description: Generate and run end-to-end tests with Playwright. Creates test journeys, runs tests, captures screenshots/videos/traces, and identifies flaky tests.
---

# E2E Command

This command invokes the **e2e-runner** agent to generate, maintain, and execute end-to-end tests using Playwright.

## What This Command Does

1. **Generate Test Journeys** - Create Playwright tests for user flows
2. **Run E2E Tests** - Execute tests across browsers
3. **Capture Artifacts** - Screenshots, videos, traces on failures
4. **Identify Flaky Tests** - Quarantine unstable tests
5. **Generate Report** - HTML report with results and artifacts

## When to Use

Use `/e2e` when:
- Testing critical user journeys (login, payments, core flows)
- Verifying multi-step flows work end-to-end
- Testing UI interactions and navigation
- Validating frontend/backend integration
- Preparing for production deployment

## How It Works

The e2e-runner agent will:

1. Analyze user flow and identify test scenarios
2. Generate Playwright test using Page Object Model pattern
3. Run tests across multiple browsers (Chrome, Firefox, Safari)
4. Capture failures with screenshots, videos, and traces
5. Identify flaky tests and recommend fixes

## Test Artifacts

**On All Tests:**
- HTML Report with timeline and results

**On Failure Only:**
- Screenshot of the failing state
- Video recording of the test
- Trace file for step-by-step replay

## Quick Commands

```bash
npx playwright test                          # Run all E2E tests
npx playwright test tests/e2e/flow.spec.ts  # Run specific file
npx playwright test --headed                # See browser
npx playwright test --debug                 # Debug mode
npx playwright codegen http://localhost:3000 # Generate test code
npx playwright show-report                  # View HTML report
```

## Best Practices

- Use Page Object Model for maintainability
- Use `data-testid` attributes for selectors
- Wait for API responses, not arbitrary timeouts
- Never run tests against production

## Integration with Other Commands

- Use `/plan` to identify critical journeys to test
- Use `/tdd` for unit tests (faster, more granular)
- Use `/code-review` to verify test quality

## Related Agents

This command invokes the `e2e-runner` agent located at:
`.antigravity/agents/e2e-runner.md`
