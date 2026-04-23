
Continue from the step-by-step manual runbook and key constraints summary.

Now help me finish the POC execution plan for this solution.

I want you to provide the next practical details for the manual POC, specifically:

1. The exact logging/monitoring I should add in MDBGTW during the POC so I can see:
   - request received
   - PocMode active
   - fake MDB Core response used
   - fake GTW DB write path used
   - Kafka publish started
   - Kafka publish succeeded or failed
   - topic name, key, and important payload fields
   - correlation id / mdbReference used for tracing

2. The exact logging/monitoring I should add in KafkaConsumerWebApp so I can see:
   - consumer started
   - environment and topicType selected
   - whether commit=false is being used
   - message received
   - key and payload captured
   - total buffered message count

3. A recommended minimal logging format for the POC demo so the output is easy to show to manager and tech lead.

4. The exact verification checklist after running the manual POC:
   - what HTTP response must be returned from CreateContractRequest
   - what log lines should appear in MDBGTW
   - what log lines should appear in KafkaConsumerWebApp
   - what fields must exist in the consumed Kafka payload
   - how to confirm that the consumed message is the one from this POC run

5. Any corrections needed to the current step 9 manual runbook based on the actual codebase routes, auth, anti-forgery, and config.

6. A final condensed runbook I can copy into my notes for execution tomorrow.

Important:
- stay in manual POC mode
- do not generate NUnit automation yet
- do not suggest Docker or local Kafka
- keep Kafka real
- keep MDB Core and GTW DB writes simulated via PocMode
- be specific to this codebase
