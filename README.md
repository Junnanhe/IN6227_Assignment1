Generate a complete NUnit test class for Loris.MDBGateway.UI.Controllers.RequestController.CreateContractRequest(CreateContractRequestModel requestModel).

Use the real code currently opened as the source of truth.

Requirements:
1. Inspect the actual RequestController constructor and create all required private Mock<T> fields.
2. Generate a proper [SetUp] method that initializes all mocks and creates the controller instance.
3. If the controller or base controller depends on HttpContext, Request, Session, User, Url, or ControllerContext, set up the minimum required mock/context objects so the action can run.
4. Create one positive test case for CreateContractRequest where:
   - requestModel is valid
   - negotiator and manager pass the LGL validation
   - GroupContractingParty, Counterparty, and Product lookups succeed
   - _contractService.CreateContractRequest returns success with MdbReference, AmendmentNumber, and ErrorMessage = null
   - _contractService.GetContract returns the created contract
   - _mdbClientService.GetContract returns Success = true with metadata JSON
   - _contractService.UpdateContract returns the updated contract
   - _contractService.ProcessWorkflowUpdate returns true
   - the action returns a success IActionResult
5. Verify all key calls:
   - _settingsService.GetSettingValueFromKey(Settings.MDBBaseUrl)
   - _contractService.GetGroupContractingPartyById(...)
   - _counterpartyService.GetCounterpartyByRicosId(...)
   - _contractService.GetContractProductById(...)
   - _contractService.CreateContractRequest(...)
   - _contractService.GetContract(...)
   - _mdbClientService.GetContract(...)
   - _contractService.UpdateContract(...)
   - _contractService.ProcessWorkflowUpdate(...)
   - _auditHistoryService.Log(...)
6. Add TestContext.WriteLine logs for important flow steps so I can demo the flow to my manager and tech lead:
   - validation passed
   - contract created
   - metadata retrieved
   - workflow publish step invoked
   - audit log written
7. For the workflow publish step, explicitly explain in comments what the unit test CAN validate and what it CANNOT validate:
   - CAN validate that ProcessWorkflowUpdate was called with expected inputs
   - CANNOT validate actual Kafka broker delivery, topic existence, or downstream consumption
8. Use FluentAssertions for assertions where helpful.
9. Keep the code compile-oriented and use the actual method names/types from the codebase. Do not invent alternative interfaces or helper classes unless absolutely necessary.
10. If any constructor dependency or return type is unclear from the open files, leave a short TODO comment instead of hallucinating.

After generating the test class, also provide a short explanation of:
- which mocks I may still need to adjust manually
- which object types came from the codebase
- what part of Kafka publishing is only indirectly verified in this unit test


Refine the generated NUnit test so it matches the actual RequestController constructor and all actual return types in this solution.

Do not rewrite from scratch.
Only fix compile issues, missing mocks, missing using statements, incorrect model types, incorrect IActionResult casting, and missing HttpContext/ControllerContext setup.

Preserve:
- the positive test scenario
- TestContext.WriteLine demo logs
- verification of the workflow publish step through _contractService.ProcessWorkflowUpdate(...)
- comments explaining Kafka validation limitations
