
Analyze this MDBGTW codebase from a unit testing perspective.

I want to identify all relevant entry points and internal logic that should be covered by unit tests.

Please include BOTH:

1. Inbound APIs exposed by MDBGTW
   - Endpoints called by external systems (e.g. MDB Core)
   - Include controller, route, HTTP method, request model

2. Internal entry points triggered within MDBGTW
   - UI-driven flows (e.g. Create Contract from GTW UI)
   - MVC controllers, API endpoints, or action methods used internally
   - Example: CreateContractRequest, GetContractCreationEligibility

For each item, provide:
- Controller / class
- Action / method
- Route (if applicable)
- Layer (UI / API / Service)
- Main services/methods called
- External dependencies used (MDB Core client, Kafka, repositories, etc.)

Then:

3. Identify the main internal business logic methods behind these entry points
   - Service methods
   - Validators
   - Helpers
   - Mapping/orchestration logic
   Only include items with meaningful logic worth unit testing

4. Group everything into:
- External-facing endpoints
- UI/internal entry points
- Core business logic methods

5. Rank the top 10–15 highest-value unit test targets based on:
- business criticality
- complexity
- regression risk
- change frequency

Do NOT list test cases yet.

Goal:
Produce a complete inventory of what we should unit test in MDBGTW, not just external APIs.
