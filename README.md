
Analyze the MDBGTW codebase from a Playwright UI automation testing perspective.

Goal:
Prepare a review-ready UI test estimation for MDBGTW only.

Important scope rules:
- Focus ONLY on user-visible behavior inside MDBGTW UI.
- Do NOT propose database assertions.
- Do NOT propose Kafka assertions.
- Do NOT propose MDB Core internal assertions.
- If a user action results in a visible success message, error message, validation message, redirect, modal, updated table row, disabled/enabled button, or changed page state inside MDBGTW, that is what the UI test should verify.
- Treat external systems (MDB Core, Kafka, DB) as black boxes behind the UI. For Playwright, we only validate UI outcome.

Please produce the output in these sections:

1. Core UI Business Features
Identify the main business features in MDBGTW that are suitable for Playwright UI automation.
Examples may include:
- Create contract
- Check contract creation eligibility
- Search contracts
- View contract details
- Resync contract
- Create amendment from funds
- Create amendment from hedge fund
- Save contract hedge fund
- DOT linkage / legal linkage flows
- Funds screens
- Reports screens
Only include features that are meaningful and testable from the UI.

For each feature, provide:
- Feature Name
- Main page / screen / route
- Main UI actions involved
- Why it is valuable for UI automation

2. Positive and Negative UI Scenarios
For each feature, list Playwright-suitable scenarios grouped into:
- Positive Scenarios
- Negative Scenarios

For each scenario, provide:
- Scenario Name
- User Steps (high level)
- Expected UI Result
- Notes on what Playwright would assert
  (examples: success toast, modal text, field validation message, redirect URL, row appears in table, button disabled, page title, etc.)

3. Exclusions / Not Asserted in UI Tests
For each feature, explicitly state what should NOT be asserted in Playwright UI tests
(for example: DB insert, Kafka publish, MDB Core internal state).

4. Complexity and Effort
For each feature, provide:
- Estimated number of Playwright test cases
- Complexity: Low / Medium / High
- Suggested automation effort in developer-days
- Brief reason for the estimate
  (e.g. simple form validation, complex multi-step modal flow, grid handling, authentication/session setup, dynamic selectors, download verification)

5. One-Time Playwright Setup Effort
Provide a separate section for one-time setup required for UI automation in MDBGTW, such as:
- Playwright project setup
- browser installation
- base page / test helpers
- login/session/auth handling
- stable selectors or data-test-id review
- test data strategy
- screenshots / tracing / CI setup if relevant

For each setup item, provide:
- Setup Item
- Description
- Effort (days)

6. Final Summary Table
Provide a final estimation summary table:
| Feature | # UI Test Cases | Complexity | Effort (days) |

7. Prioritization
Recommend the top 5 UI features to automate first for maximum value.

Additional guidance:
- Prefer business-critical user journeys over low-value navigation-only checks.
- Include field validation, warning dialogs, failure banners, duplicate/invalid input scenarios, and visible success outcomes.
- If a flow is too unstable or hard to automate reliably in UI, mention that explicitly.
- Keep the output practical and suitable for a tech lead estimation discussion.
