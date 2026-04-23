
Please validate the current manual runbook against the actual codebase and correct any guessed values.

Specifically verify:

1. Does Loris.MDBGateway.Misc.KafkaConsumerWebApp really expose POST /api/auth/login?
   - If yes, show the exact controller/action and expected request body.
   - If no, correct the login/authentication step.

2. What exact local port does KafkaConsumerWebApp run on by default?

3. What exact local port does Loris.MDBGateway.UI run on by default?

4. Is the username/password in the runbook real or guessed?
   - Show where the consumer app auth configuration comes from.

5. Confirm whether the expected HTTP response for POST /Contracts/CreateRequest will really be:
   - mdbReference = POC-2026-00001
   - amendment = 000
   - contractName = POC ISDA Agreement
   - message containing POC-2026-00001
   based on the fake classes proposed earlier.

6. Confirm whether links.GTW will really contain localhost:5000/contracts/POC-2026-00001/000 in this codebase, or correct it.

Please output a corrected final runbook with only verified values.
