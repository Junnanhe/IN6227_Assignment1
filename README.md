
I want to build a proof-of-concept integration test for MDBGTW using a real HTTP call to RequestController.CreateContractRequest.

Target scenario:
- happy path only
- request goes through RequestController.CreateContractRequest
- flow eventually publishes to Kafka workflow topic
- after publish, I want to verify the produced Kafka message by consuming it via the existing Loris.MDBGateway.Misc.KafkaConsumerWebApp

Please analyse this codebase and tell me:

1. whether this end-to-end POC is feasible with the current solution
2. the exact HTTP route, method, headers, auth/session/anti-forgery requirements for CreateContractRequest
3. the minimum valid request body for a happy-path contract creation
4. the exact method chain from CreateContractRequest to Kafka publish
5. which Kafka topic is used
6. whether KafkaConsumerWebApp can consume that exact message for verification
7. the exact sequence of steps for the POC:
   - clear consumer messages
   - start consumer
   - call CreateContractRequest
   - retrieve consumed messages
8. what blockers may prevent the POC from working
9. what must be mocked, seeded, or configured
10. whether this should be implemented as a true automated integration test or as a manual/POC runbook first

Please keep the answer practical and specific to this codebase.



Now help me implement the POC step by step.

I want a happy-path integration flow that:
1. starts KafkaConsumerWebApp consumption for workflow topic
2. performs a real HTTP POST to RequestController.CreateContractRequest
3. retrieves consumed workflow messages from KafkaConsumerWebApp
4. verifies that the expected message is present

Please provide:
- exact request examples for KafkaConsumerWebApp start/clear/messages endpoints
- exact HTTP request example for CreateContractRequest
- any required headers/cookies/anti-forgery tokens
- how to identify the created message uniquely (for example MDB reference or correlation id)
- a minimal runbook I can execute manually first
- then a suggestion for how to automate it later in NUnit if feasible

