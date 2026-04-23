Please analyse this solution/codebase and prepare an integration testing inventory focused on HTTP-level tests for all controller/API entry points that eventually trigger Kafka publishing.

Context:
- We are no longer focusing on pure unit tests.
- We want HTTP/integration-style tests that simulate real API usage, similar to Postman or frontend calls.
- This includes:
  1. external APIs exposed by MDBGTW
  2. internal UI/controller endpoints that are called through HTTP and eventually trigger Kafka publishing
- Main goal: identify all controller endpoints/actions that, directly or indirectly, lead to Kafka publish logic.

For each endpoint/action, produce the output in a structured table format with these columns:
1. Project
2. Controller
3. Action / Endpoint
4. Route
5. HTTP Method
6. Entry Type (external API / internal UI / internal controller)
7. Business Purpose
8. Downstream Kafka publish method eventually reached
9. Kafka topic(s) involved
10. Whether Kafka payload can be verified with mocked producer in integration test
11. Whether real Kafka read-back is feasible from current codebase/test setup
12. Main positive scenarios
13. Main negative scenarios
14. Key dependencies to simulate/mock
15. Suggested integration test approach
16. Estimated development effort

Important requirements:
- Only include endpoints/actions that eventually trigger Kafka publishing.
- Trace the full flow from controller action -> service -> Kafka publish method.
- Distinguish clearly between:
  A. verifying that the app ATTEMPTS to publish the correct topic/key/payload using a mocked Kafka producer
  B. verifying that the message can actually be read back from a real Kafka topic
- If real Kafka read-back is not realistically feasible with the current codebase/test setup, explicitly say so and explain why.
- If real Kafka read-back would require additional infrastructure (real broker, topic setup, consumer, test container, environment config, etc.), list that separately.
- For each endpoint, list both positive and negative scenarios at a business level, not only technical null-check scenarios.
- Effort estimate should include:
  - integration test project/setup
  - WebApplicationFactory or equivalent HTTP test host setup
  - dependency override / DI mocking setup
  - auth/session/header setup if required
  - Kafka mock/capture setup
  - optional real Kafka setup if feasible
  - test data preparation / seeding
  - writing each scenario
  - debugging/stabilisation

Please prioritise the output in this order:
1. highest business value
2. highest regression risk
3. endpoints most frequently used
4. endpoints with the most Kafka complexity

After the table, provide:
A. Top 10 highest-priority HTTP integration test targets
B. Which targets should use mocked Kafka capture only
C. Which targets might justify real Kafka integration later
D. Key risks / blockers / assumptions
E. A recommended phased approach for implementation with rough total effort

Do not give generic testing advice.
Base everything on the actual codebase, routes, controllers, services, and Kafka publish methods in this solution.
If any route/controller/service cannot be confirmed from the code, mark it as “needs manual verification”.




Now take the endpoints/actions you identified that eventually trigger Kafka publishing and expand them into a detailed estimation sheet.

For each endpoint/action, create a scenario-level table with these columns:
1. Endpoint / Action
2. Scenario ID
3. Scenario Type (positive / negative)
4. Scenario Description
5. Input / Preconditions
6. Mocked / simulated dependency behavior
7. Expected HTTP result
8. Expected business result
9. Expected Kafka behavior
10. Kafka validation type
   - mocked publish capture only
   - real Kafka read-back
   - not feasible in current setup
11. Key assertions
12. Estimated dev effort
13. Notes / risks / assumptions

Rules:
- For each endpoint, include realistic business scenarios, not only simple validation failures.
- Include at least:
  - 1 happy path
  - failures before Kafka publish
  - failures during Kafka publish
  - scenarios where Kafka should not be called
  - scenarios where topic / key / payload content should be validated
- If a scenario requires real Kafka broker or consumer setup, clearly mark the extra setup effort separately.
- Separate reusable setup effort from incremental effort for additional scenarios on the same endpoint.
- Highlight which scenarios are best for initial POC/demo to tech lead/manager.

At the end, give:
A. Recommended POC scenarios to implement first
B. Shared setup effort vs per-scenario effort
C. Total effort by endpoint
D. Total effort by phase
