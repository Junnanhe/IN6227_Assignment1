
I want to create a proof-of-concept NUnit unit test for the MDBGTW contract creation flow starting from UI RequestController.CreateContractRequest().

Please analyze the REAL code in this solution and generate production-appropriate unit test code using the ACTUAL constructor dependencies, method names, models, namespaces, and return types from this codebase. Do not invent classes or interfaces.

My goals for this POC are:
1. Start from RequestController.CreateContractRequest()
2. Include at least:
   - 1 simple negative test
   - 1 positive test
3. For the positive test, add custom test logs / TestContext progress messages so I can show manager and tech lead the flow clearly
4. If this flow eventually triggers Kafka publish in the service layer, verify in unit test that:
   - the Kafka producer dependency was called
   - the expected topic value was used
   - the message/payload object passed to Kafka is correct at unit-test level
5. Clearly explain what the unit test can validate and what it cannot validate

Please produce the output in this structure:

A. Testing strategy
- Explain whether RequestController.CreateContractRequest() alone is enough to verify Kafka behavior
- If not, explain which additional service-level test is needed and why

B. Testability conclusion
- State exactly what is possible in a unit test:
  - controller result
  - validation branches
  - service method invocation
  - Kafka producer method invocation
  - topic argument passed to Kafka
  - payload object / serialized content passed to Kafka
- State exactly what is NOT possible in a pure unit test:
  - whether the Kafka topic actually exists
  - whether Kafka broker accepts the message
  - whether downstream consumer receives it
  - whether the message can really be consumed from Kafka
  - network / broker / auth / serialization issues outside mocked code

C. Recommended POC scope
Recommend the smallest POC that is realistic:
1. Controller unit test for CreateContractRequest negative scenario
2. Controller unit test for CreateContractRequest positive scenario
3. Service-level unit test for the method inside the create flow that actually publishes to Kafka

D. Generate the actual NUnit test code
Please generate:
1. RequestControllerTests.cs
   - full test class
   - Moq setup for real dependencies
   - [SetUp] method
   - one negative test:
     CreateContractRequest_WhenRequestModelIsNull_ShouldThrowArgumentNullException
     or the real simplest negative path based on actual code
   - one positive test:
     valid request returns success result
   - add readable POC logs using TestContext.Progress.WriteLine(...) for each important step:
     "Arrange: build valid request"
     "Arrange: mock negotiator validation"
     "Arrange: mock manager validation"
     "Arrange: mock MDB Core success response"
     "Act: calling CreateContractRequest"
     "Assert: success response returned"
     "Assert: contract service method invoked"
2. If Kafka is not directly triggered in the controller path, also generate:
   ContractServiceTests.cs or the correct service test file for the actual Kafka-publishing method
   - include one positive test that verifies:
     - Kafka producer was called
     - correct topic argument was passed
     - payload content is inspected and asserted
   - also add TestContext.Progress.WriteLine(...) logs so I can show the sequence in Test Explorer output

E. Logging for manager / tech lead POC
Show me the exact TestContext.Progress.WriteLine statements to add so the test output reads like a mini execution flow.
Example style:
- Step 1: validating request
- Step 2: calling service
- Step 3: mocking MDB Core response
- Step 4: verifying Kafka publish request
- Step 5: confirming expected topic and payload

F. Kafka verification details
If the payload object is complex, show how to capture the actual argument passed into the mocked Kafka producer and print key fields to test output, for example:
- topic
- contract reference
- amendment number
- event status
- any important metadata field

G. Final note for manager
Write a short plain-English explanation I can say:
- what this unit test proves
- what this unit test does not prove
- why Kafka broker delivery/consumption requires integration or end-to-end testing instead of unit testing

Important constraints:
- use NUnit + Moq
- use the actual codebase types only
- no pseudo-code
- no fake dependencies
- if constructor setup is too large, simplify only by using the minimum real dependencies needed
- if some dependency names are unclear, tell me exactly which file/class I should open next
