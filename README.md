
I want to build a POC integration flow for MDBGTW around RequestController.CreateContractRequest.

Goal:
- call the endpoint through a real HTTP POST from local
- keep the controller and service flow real
- simulate all MDB Core interactions (both POST and GET)
- simulate all GTW database save/update operations so no real DB persistence happens
- keep Kafka publishing real, using the configured INT Kafka broker/topic
- consume/read back the published Kafka message using the existing Loris.MDBGateway.Misc.KafkaConsumerWebApp
- verify the topic, key, and payload content from the consumed message

Please analyse this codebase and provide a practical implementation plan specific to this solution.

I need you to identify:

1. All code paths inside RequestController.CreateContractRequest that call MDB Core and must be faked/simulated
2. All repository/service calls in this flow that save or update GTW database state and must be faked/simulated
3. The minimum fake responses needed so the happy path can continue all the way to Kafka publish
4. The best way to register these fake implementations only for local development / POC mode
5. Whether to do this through DI replacement in Program.cs / Startup, and exactly which services/interfaces should be replaced
6. The exact Kafka topic used in this flow and whether local MDBGTW can publish to INT Kafka using current config
7. How to run and use Loris.MDBGateway.Misc.KafkaConsumerWebApp to:
   - clear previous messages
   - start consuming workflow topic
   - retrieve consumed messages
8. How to uniquely identify the POC message in Kafka so we can verify the correct payload
9. A step-by-step manual runbook for the POC:
   - start consumer
   - fetch anti-forgery token
   - call CreateContractRequest
   - wait/poll
   - read message from KafkaConsumerWebApp
   - verify topic/key/payload
10. A minimal code change approach only — avoid broad refactoring

Important constraints:
- Do not suggest Docker or local Kafka
- Kafka must remain real
- MDB Core and GTW DB writes must be simulated
- The result should be suitable for a POC/demo, not a production-grade test framework

Please be specific to this codebase and name the exact interfaces/classes to replace.








Now generate the actual POC implementation for the approach above.

Please provide:

1. Fake service implementations for all MDB Core calls in the CreateContractRequest happy path
2. Fake repository/service implementations for all GTW DB save/update calls in the same flow
3. DI registration changes for local development / POC mode only
4. Any config flag needed, for example UsePocFakes=true
5. The minimum fake data values required so the flow reaches Kafka publish successfully
6. A sample HTTP request for CreateContractRequest
7. Exact HTTP requests for KafkaConsumerWebApp:
   - clear
   - start
   - messages
8. A verification checklist showing what proves success:
   - endpoint returns success
   - Kafka message appears
   - topic matches
   - key matches
   - payload contains expected fields

Keep the changes isolated and practical.




