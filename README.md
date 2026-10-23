
I want to improve this Playwright TypeScript project with a reusable, common AI-assisted diagnostics and test-discovery layer.

Please inspect the existing project structure and follow the existing coding style, Page Object Model, auth setup, and Playwright configuration. Do not redesign the whole project unless necessary.

The goals are:

1. Playwright + Copilot should be able to inspect a page and understand the available UI elements so that we can identify missing test scenarios.
2. Whenever a Playwright test fails, reusable common functions should automatically capture enough diagnostic information for Copilot to understand the failure and propose/fix the test.
3. The failure/healing infrastructure must be shared across all current and future spec files. Do not duplicate diagnostic logic in each spec.
4. The solution must use Playwright and built-in Node/TypeScript functionality only. Do not add third-party libraries unless absolutely necessary.
5. Do not weaken or change business assertions just to make tests pass.

Please implement this incrementally.

First, create a reusable common utility or fixture for failure diagnostics, for example under:

tests/utils/failure-diagnostics.ts

or a shared fixture location if that fits the existing architecture better.

On test failure, automatically capture and save:

- full Playwright error / stack trace
- current URL
- full-page screenshot
- Playwright ARIA/page snapshot suitable for AI analysis
- useful visible page information
- trace.zip using Playwright's existing trace functionality
- console errors if practical
- failed test name and test file

Store everything under the normal Playwright test-results folder, scoped to the failed test, so Copilot Agent can easily inspect the artifacts.

Use the Playwright 1.62.1 APIs available in this project.

Prefer using:

page.ariaSnapshot({ mode: 'ai' })

when supported.

Create a shared mechanism so tests do not need to manually repeat the same afterEach logic. Prefer a reusable fixture/base test if that fits the existing suite. Existing tests should need minimal changes.

Second, create a reusable page-inspection/test-discovery utility.

Its purpose is NOT to automatically click random elements. Its purpose is to inspect a page and produce a structured artifact that Copilot can analyze to suggest missing test cases.

For a given page, capture:

- ARIA/accessibility snapshot
- current URL
- visible headings
- buttons
- links
- textboxes
- comboboxes/dropdowns
- checkboxes/radio buttons
- tables
- table headers
- forms
- required fields where detectable
- validation/error messages where detectable
- important data-testid/id/name/aria attributes where useful
- visible disabled/enabled states where practical

Save the result into a human-readable file such as:

test-results/page-inspection/<page-name>/page-snapshot.yml

or a similar structure.

Create a reusable function such as:

inspectPage(page, pageName)

so I can call it for pages such as:

- Contracts dashboard
- Create Contract
- Contract Detail
- MDB CORE pages later

Third, create a reusable workflow/instructions file for Copilot Agent, for example:

.github/copilot-instructions.md

or another appropriate project-level instruction file.

The instructions should tell Copilot that when a Playwright test fails, it should:

1. Inspect the failing .spec.ts.
2. Inspect the relevant Page Object.
3. Inspect the generated failure artifacts.
4. Determine whether the failure is caused by:
   - locator changes
   - page structure changes
   - synchronization/waiting
   - navigation
   - test data
   - genuine application/business behavior
5. If it is a technical automation issue, propose or implement the smallest safe fix.
6. Rerun only the failed test first.
7. Continue until the technical issue is resolved.
8. Never change expected business values, validation rules, statuses, or assertions merely to make a test pass.
9. If the failure appears to be a real application defect, stop and report it rather than modifying the test to hide it.

For test discovery, Copilot should be instructed to:

1. Read the generated page-inspection artifact.
2. Read the existing tests for that page.
3. Compare the available controls/behaviors against existing coverage.
4. Suggest missing:
   - positive scenarios
   - negative scenarios
   - validation scenarios
   - boundary scenarios
   - state-transition scenarios
   - combinations that are meaningful
5. Avoid duplicate or low-value test cases.
6. Do not automatically implement every discovered scenario. First produce a proposed test-plan file for review.

For example:

specs/generated/create-contract-test-plan.md

The generated plan should include:

- scenario name
- purpose
- preconditions
- test data required
- steps
- expected result
- whether an equivalent automated test already exists

Important design rules:

- Keep selectors inside Page Objects wherever possible.
- Prefer stable Playwright locators: getByRole, getByLabel, getByTestId, stable IDs, and scoped locators.
- Avoid brittle selectors such as long CSS chains or nth-child unless no better option exists.
- Avoid arbitrary waitForTimeout calls. Wait for meaningful page state or responses.
- Do not let "healing" silently change business meaning.
- Every automated healing change must remain visible in the Git diff.
- Do not auto-commit.
- Keep all generated artifacts outside application source files.
- Make the utilities generic so they work for future MDB CORE tests as well.

After implementation, demonstrate it using ONE existing test only, preferably create-contract.spec.ts:

1. Run the test normally.
2. Deliberately simulate or identify a safe locator failure.
3. Show that the failure artifacts are generated.
4. Use those artifacts to diagnose the issue.
5. Restore/fix the locator.
6. Rerun the test and confirm it passes.

Do not expand to the entire suite until this proof of concept works cleanly.

Before changing existing behavior, explain the files you plan to add or modify and why.
