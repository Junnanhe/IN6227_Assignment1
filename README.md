
I have modified two ContractService constructors by adding IAuditHistoryService to resolve a .NET DI ambiguous constructor issue in the MDBCoreSync batch.

Please perform a solution-wide impact analysis. Do not only inspect ContractService.cs.

Check the following:

Find every place where ContractService is instantiated, resolved through DI, or registered (including IContractService, ServiceCollectionExtensions, Program.cs, unit tests, integration tests, batches, console apps, UI, APIs, etc.).
List every public constructor of ContractService and determine which projects/features use each constructor.
Verify that after adding IAuditHistoryService to these two constructors:
No project will fail DI resolution.
No new ContractService(...) call is missing the new parameter.
No unit tests or mocks will fail because of the additional constructor parameter.
No other batch jobs, console applications, scheduled jobs, or APIs are affected.
Search the entire solution for IAuditHistoryService registration and verify it is registered in every project that may resolve those constructors.
Check whether there are still any combinations of public constructors that could produce a .NET DI ambiguous constructor exception.
Explain why each affected or unaffected project is safe.

Finally, produce a table like this:

Project	Uses ContractService	Uses which constructor	IAuditHistoryService registered	Impact	Action Needed

If you find any potential runtime issue, show the exact file and line that may be affected and explain why.
