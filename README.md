
Analyze this MDBGTW codebase from a unit testing perspective and first produce only:

1. All inbound endpoints exposed by MDBGTW
2. The main internal business-logic methods behind them
3. A priority ranking for Phase 1 unit testing

For each endpoint include:
- controller
- action
- route
- request model
- main services called
- external dependencies

For each internal business-logic method include:
- class
- method
- why it is important
- what dependencies it has

Then rank the top 10 highest-value unit test targets for Phase 1.

Do not yet list all detailed test cases.



Now take the top 10 Phase 1 unit test targets you identified and, for each one, provide:

1. Positive scenarios
2. Negative scenarios
3. Edge cases
4. Recommended test level
5. Dependencies to mock
6. Estimated number of test cases
7. Rough developer effort
8. Complexity and reason

Be concrete and use actual code behavior from this codebase.
Output in a format that can easily be copied into Excel.
