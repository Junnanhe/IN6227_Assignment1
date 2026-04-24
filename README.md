
Now that the manual POC flow works, please create an NUnit integration test based on the same POC flow.

Goal:
Create an automated NUnit test that calls the real local HTTP endpoint:
POST /Contracts/CreateRequest

The test should follow the same POC behavior:
- PocMode=true
- MDB Core calls are simulated
- GTW database writes are simulated
- Kafka publish remains REAL
- After the HTTP call succeeds, the test must consume/read from the Kafka workflow topic and verify the published message.

Please implement this as a POC integration test, not a normal unit test.

Requirements:
1. Use NUnit.
2. Start from the existing working POC configuration.
3. Call the API using HttpClient against the local running MDBGTW UI app.
4. Send this request body:

{
  "negotiator": "UT2M63",
  "manager": "UT2M63",
  "groupContractingParty": 1,
  "counterparty": "0000290006",
  "product": 2,
  "creationForNewOrAmendment": "New",
  "mdbReference": null
}

5. If anti-forgery token is required, first call:
GET /Contracts/Create
Then extract:
- anti-forgery cookie
- __RequestVerificationToken
Then include both in the POST.

6. Assert the CreateContractRequest response:
- HTTP 200
- mdbReference = POC-2026-00001
- amendment = 000
- message contains contract created

7. After the POST succeeds, consume/read the Kafka workflow topic:
taas.cacib.mdbgtw-dev.queue.workflow.json

8. Verify that at least one consumed Kafka message has:
- key = POC-2026-00001
- payload.reference = POC-2026-00001
- payload.internal_id = 99999
- payload.product.type_of_contract = ISDA
- payload.links exists
- payload.last_published_workflow_date is not null

9. Add detailed TestContext.WriteLine logs for every important step:
- test start
- local base URL
- GET token response
- extracted token/cookie
- POST request body
- POST response status
- POST response body
- Kafka topic
- Kafka bootstrap servers
- Kafka consumer group id
- every consumed message key
- every consumed message payload
- final matching message found / not found

10. Important:
Do not commit offsets during this POC test if possible.
Use a unique consumer group id per test run, for example:
mdbgtw-poc-test-{Guid}

11. Add timeout/polling:
Poll Kafka for up to 30 seconds after POST.
Fail the test if no matching message is found.

12. Please inspect existing KafkaConsumerWebApp / KafkaConsumerService and reuse existing configuration/classes if suitable.
If easier, implement direct Kafka consumption in the NUnit test using Confluent.Kafka.

13. Keep this test clearly marked:
[Explicit] or [Category("POCIntegration")]
because it depends on local app + DEV Kafka.

14. Do not change production behavior.
Any helper code should be clearly marked POC/test only.

Please generate the NUnit test class and any helper methods needed.
Also explain exactly what config must be present before running the test.
