
Please re-check the POC implementation for RequestController.CreateContractRequest end-to-end.

Current issue:
During the POC HTTP flow, execution reaches ContractService.CreateContractRequest, but it goes into the MDB response validation branch and fails because validation.StatusCodes is null.

Please inspect the full CreateContractRequest flow and the fake POC classes:
- PocMDBClientService
- PocContractRepository
- PocCounterpartyService
- PocKafkaMonitorRepository
- Program.cs PocMode registration

Goal:
For PocMode=true, the CreateContractRequest happy path must continue all the way to ProcessWorkflowUpdate and real Kafka publish.

Please identify and fix all missing fake response fields/dependencies required by the real code path.

Specifically check:
1. PocMDBClientService.PostContractAmendment must return HttpResponseMessage 200.
2. The response JSON must match the model expected by ContractService.CreateContractRequest.
3. If ContractService tries to deserialize PostContractAmendmentValidationResponse, StatusCodes must not be null.
4. If the success path expects a different response model, adjust the fake JSON to match it.
5. PocMDBClientService.GetContract must return metadata JSON that allows ProcessWorkflowUpdate to build Kafka payload.
6. PocContractRepository fake methods must return safe non-null values for all methods used in the happy path.
7. Any fake method not meant to fail should log and return safe default values.
8. Do not mock or replace KafkaProducerService. Kafka publish must remain real.
9. Keep all fake logic only under PocMode=true and clearly marked POC only.

Please provide the exact corrected code changes needed and explain which missing dependency or missing field caused the null StatusCodes error.
