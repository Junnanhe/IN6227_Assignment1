
Good. Now help me implement this POC step by step in code.

Please do not redesign the approach.

Based on the verified runbook, generate the exact minimal code changes for:
1. PocMDBClientService
2. PocContractRepository
3. PocCounterpartyService if needed
4. PocKafkaMonitorRepository if needed
5. Program.cs DI replacement under PocMode = true
6. minimal warning logs matching the runbook
7. appsettings.Development.json changes

Constraints:
- keep real HTTP call to POST /Contracts/CreateRequest
- keep real Kafka publish
- keep real Kafka consumer read-back
- fake MDB Core POST/GET
- fake GTW DB writes
- minimal code changes only
- development/POC only, not production refactor

Please generate the files one by one, starting with PocMDBClientService.
