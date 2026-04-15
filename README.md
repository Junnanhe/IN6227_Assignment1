
Analyze the MDBGTW codebase from a unit testing estimation perspective.

Goal:
Identify all important components / processes / endpoints in MDBGTW that eventually trigger Kafka publishing at some step in their flow.

Important clarification:
- Kafka is the selection criterion for which components are in scope.
- For each selected component, I do NOT want only Kafka-related test scenarios.
- I want the FULL set of meaningful unit test scenarios for that component, including:
  - positive scenarios
  - negative scenarios
  - validation failures
  - branching logic
  - external dependency failures
  - repository / DB-related behavior
  - and Kafka-related assertions where applicable

So the goal is:
1. Find all components/flows that eventually lead to Kafka publish
2. For each one, provide the full unit test scope for that component
3. Include Kafka topic/message verification only as part of the expected results where relevant

Please produce the output in these sections:

1. In-Scope Kafka-Producing Components / Flows
For each item provide:
- Flow / Component Name
- Trigger Type (UI action / external API / internal service / batch)
- Entry Point
- Main internal method chain
- Where Kafka is triggered in the flow
- Kafka topic(s) involved
- Why this component is in scope

2. Full Unit Test Scenario Coverage Per Component
For each selected component, list:
- Positive Scenarios
- Negative Scenarios
- Edge Cases

For each scenario include:
- Scenario Name
- Description
- Dependencies involved
- Expected Result
- Whether Kafka should be:
  - published
  - not published
  - published with fallback behavior on failure

3. Kafka Assertions Within the Component
For each selected component, explain:
- what Kafka behavior should be asserted
- correct topic
- correct payload/message
- whether publish should happen or not happen
- fallback / retry / KafkaMonitor behavior if publish fails

4. Recommended Test Level
For each component recommend:
- controller unit test
- service unit test
- orchestrator unit test
- helper/validator unit test
and explain which level gives the best coverage

5. Estimation
For each component provide:
- estimated number of test cases
- complexity (Low / Medium / High / Very High)
- suggested effort in developer-days
- brief reasoning

6. Final Summary Table
Provide a final summary table:
| Component / Flow | Entry Point | Kafka Step | # Test Cases | Complexity | Effort (days) |

7. Prioritization
Recommend the top 5 components to implement first.

Guidelines:
- Focus on MDBGTW-owned logic only
- Assume external dependencies are mocked
- Do not reduce the scope to Kafka-only scenarios
- Kafka is only the filter to identify which components are included
- Scenario coverage must reflect the full component behavior
- Keep the output practical and suitable for tech lead review
