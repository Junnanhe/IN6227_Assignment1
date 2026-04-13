Based on the MDBGTW Unit Testing Inventory provided earlier, generate a detailed unit test estimation sheet suitable for tech lead review.

Scope:
- Focus ONLY on high-value targets (service layer + critical controllers), not every endpoint.
- Prioritize methods with complex logic, external dependencies (MDB Core, Kafka, DB), or high business impact.
- Assume unit testing strategy only, not end-to-end or full integration testing.

I want the estimation to include BOTH:
1. One-time setup/foundation effort
2. Per-component test implementation effort

Part A — One-time setup effort
Please identify the one-time work required before writing meaningful unit tests, such as:
- creating or organizing the NUnit test project
- adding/verifying NuGet packages
- project references
- mock helper / base test setup
- test data builders / sample objects
- getting tests runnable in Visual Studio and optionally pipeline
- any other initial setup needed for MDBGTW unit testing

For each one-time setup item, provide:
- Setup Item
- Description
- Estimated Effort (days)
- Why it is needed

Part B — Per-component estimation
For EACH selected target (endpoint or service method), provide:

1. Component Name
2. Why it should be tested
3. Test Scenarios grouped into:
   - Positive Scenarios
   - Negative Scenarios

For EACH scenario include:
- Scenario Name
- Description
- Type (Positive / Negative)
- Dependencies involved (MDB Core / Kafka / DB / etc.)
- Expected Result

4. Complexity Assessment
- Low / Medium / High / Very High
- Brief justification

5. Estimated Effort
- Number of test cases
- Suggested dev effort in days
- Brief reasoning

6. Output format
- Structured table per component
- Then one final summary table:
  | Component | # Test Cases | Complexity | Effort (days) |

Guidelines:
- Assume external dependencies are mocked
- Include validation failures, external API failures, DB failures, Kafka failures, and branch/decision logic
- Avoid full E2E scenarios
- Keep the output concise, practical, and review-ready for a tech lead

Finally provide:
1. Top 5 components to implement first
2. Total one-time setup effort
3. Total component implementation effort
4. Grand total estimated effort
5. A suggested phased delivery plan
