
Analyze the currently opened code for:
- RequestController.CreateContractRequest(...)
- ContractService.ProcessWorkflowUpdate(...)

Goal:
I want to see whether I can create ONE hybrid NUnit test that starts from RequestController.CreateContractRequest(...) and, instead of mocking away the Kafka publish flow, uses the real ContractService.ProcessWorkflowUpdate(...) so I can capture and log the actual Kafka topic, key, and payload sent by Gateway.

Please do NOT change production code.

Tasks:
1. Inspect the real RequestController constructor and the real ContractService constructor.
2. Determine whether this hybrid test is feasible without changing production code.
3. If feasible, generate a compile-oriented NUnit test skeleton that:
   - creates a real RequestController
   - uses a real ContractService instance
   - mocks only the dependencies required around it
   - allows ProcessWorkflowUpdate(...) to execute for real
   - mocks IKafkaProducerService.Publish(...) with a Callback to capture:
     - topic
     - key/reference
     - payload string
   - writes these captured values using TestContext.WriteLine
4. In the generated code, clearly separate:
   - dependencies that must stay mocked
   - dependencies that must use the real implementation
5. Verify in the test:
   - the controller action succeeds
   - ProcessWorkflowUpdate(...) actually ran
   - Kafka publish was attempted once
   - the captured topic is not null/empty
   - the captured payload is not null/empty
   - the payload contains the expected MDBReference and workflow metadata fields if possible
6. If a single hybrid test is NOT realistically feasible because of constructor complexity, base controller dependencies, or too many required collaborators:
   - say so clearly
   - explain exactly why
   - then provide the best alternative:
     A. keep POC1 as controller test
     B. add a separate ContractService.ProcessWorkflowUpdate(...) unit test
   - generate that alternative service-level NUnit test instead
7. Add comments in the generated code that explain:
   - what this test CAN validate
   - what this test CANNOT validate
   Specifically mention:
   - it CAN validate topic/key/payload passed to IKafkaProducerService.Publish(...)
   - it CANNOT validate real Kafka broker delivery, topic existence in infrastructure, or downstream consumer receipt
8. Use the actual types, method signatures, and namespaces from the codebase.
9. If any dependency or return type is unclear, add a TODO comment instead of inventing fake code.
10. At the end, give me a short recommendation:
   - “hybrid controller+service test is feasible”
   or
   - “better to keep controller POC and create a dedicated service-level Kafka payload test”

Important:
- prioritize accuracy over completeness
- do not hallucinate helper classes unless absolutely necessary
- produce code I can paste into my existing NUnit test project and refine
