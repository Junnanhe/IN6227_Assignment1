
You are acting as a senior .NET migration architect.

I have a production .NET 6 solution named MDB Gateway containing approximately 46 projects. The solution includes ASP.NET Core APIs, class libraries, service and repository projects, batch or console applications, test projects, Kafka integrations, database access, dependency injection, authentication, configuration, logging and calls to an upstream MDB Core application.

The objective is to upgrade the complete solution from .NET 6 to .NET 10 as a strictly technical upgrade.

Primary requirement:

Preserve all existing business behaviour, API contracts, Kafka behaviour, database behaviour, security behaviour, configuration behaviour, logging behaviour and deployment behaviour. Do not introduce feature changes, redesigns, refactoring, cleanup or performance optimisation unless a change is strictly required for .NET 10 compatibility.

Important internal-package constraint:

The solution uses internal NuGet packages whose names contain “Loris”. Preserve the current versions of all Loris packages initially. Do not upgrade, replace or modify a Loris package merely because a newer version exists.

For every Loris package:

1. Record the current version.
2. Record which projects reference it.
3. Determine its declared target frameworks and dependency requirements where this information is available.
4. Check whether it restores, compiles and loads when consumed by a net10.0 project.
5. Classify it as:

   * compatible without changes;
   * compatible but requiring validation;
   * incompatible with supporting evidence;
   * compatibility unknown.
6. Recommend changing its version only when there is concrete evidence that the existing version is incompatible with .NET 10.
7. Do not guess package compatibility.

For this first phase, perform analysis only. Do not edit any files.

Analyse the entire solution and produce the following:

1. Project inventory

For every project, provide:

* Project name and path
* Project type
* Current TargetFramework or TargetFrameworks
* Direct project references
* Direct NuGet package references
* Whether it references a Loris package
* Whether it is an executable, deployable application, library, test project or database-related project
* Expected upgrade complexity: low, medium or high
* Main compatibility risks

2. Dependency graph and upgrade order

Build a project-reference dependency graph and identify:

* Leaf projects with no internal project dependencies
* Shared projects referenced by many other projects
* Circular dependencies, if any
* Deployable entry-point projects
* The safest project upgrade sequence

Do not order projects alphabetically. Order them according to project dependencies.

3. Package assessment

Classify package references into:

* Can remain at their current version
* Must be upgraded for net10.0 compatibility
* Should be upgraded only because of security or support concerns
* Internal Loris package requiring compatibility verification
* Obsolete or replaced package
* Unknown and requiring manual investigation

Do not bulk-upgrade all NuGet packages. Keep existing versions whenever they are compatible with net10.0.

For every recommended package update, explain:

* Current version
* Proposed version
* Why it is required
* Whether it could cause source, binary or behavioural changes
* Which projects are affected

4. Breaking-change assessment

Identify relevant breaking changes across the migration path from .NET 6 through .NET 7, .NET 8, .NET 9 and .NET 10.

Focus particularly on:

* ASP.NET Core
* Dependency injection
* Hosting and middleware
* Authentication and authorization
* HTTP clients
* JSON serialization
* Configuration binding
* Logging
* Cryptography
* Entity Framework Core, if used
* Dapper and SQL access
* Kafka clients and serialization
* Reflection
* Date and time handling
* File handling
* Background services
* Batch and console applications
* IIS hosting
* Containers, if used
* Build and deployment pipelines

Do not assume that compilation success means behavioural compatibility.

5. Proposed migration batches

Divide the 46 projects into small migration batches of approximately three to six logically related projects.

For every batch, show:

* Projects included
* Why they belong together
* Prerequisites
* Expected files to change
* Required validation
* Rollback boundary

6. Baseline validation plan

Before upgrading, specify how to capture the existing .NET 6 baseline, including:

* Restore result
* Release build result
* Existing warnings
* Automated test results
* API smoke-test results
* Important API request and response contracts
* Kafka topic, message key, headers and payload examples
* Batch execution results
* Database behaviour
* Authentication behaviour
* Configuration binding
* Logging output
* IIS and deployment behaviour

7. Risk register

Create a risk register containing:

* Risk
* Affected projects
* Likelihood
* Impact
* Detection method
* Mitigation
* Rollback method

8. Migration rules

The later implementation must follow these rules:

* Change TargetFramework only when the project is ready to be validated.
* Do not change business logic unless compilation or verified runtime compatibility requires it.
* Do not rename public APIs, DTO properties, configuration keys, Kafka fields, topics, routes or database objects.
* Do not change JSON serialization behaviour without explicit evidence and approval.
* Do not change nullable-reference-type settings merely to suppress warnings.
* Do not suppress new warnings globally.
* Do not add NoWarn entries unless individually justified.
* Do not replace libraries simply because a newer alternative exists.
* Do not upgrade all NuGet packages automatically.
* Do not perform formatting-only changes.
* Do not reorganise folders or namespaces.
* Do not convert controllers to minimal APIs.
* Do not redesign Program.cs or dependency injection unless required.
* Do not combine the migration with code cleanup.
* Keep every migration batch independently buildable, testable and reversible.
* Stop and report when a change could alter runtime behaviour instead of silently applying it.

9. Final output

Produce:

* Executive summary
* Complete project inventory
* Dependency-based upgrade order
* Package compatibility table
* Loris package compatibility table
* Breaking-change findings
* Migration batches
* Validation plan
* Risk register
* Exact recommended first batch

Do not modify files until I review and approve the plan.




Implement only migration batch [BATCH NUMBER] from the approved MDB Gateway .NET 6 to .NET 10 migration plan.

Projects included:

[PASTE PROJECT NAMES]

Before editing:

1. Confirm the current Git branch and working-tree status.
2. Confirm that there are no unrelated uncommitted changes.
3. Show the project dependencies for this batch.
4. Record the existing TargetFramework and package versions.
5. Record all Loris package references and preserve their versions.
6. Run or inspect the latest available baseline build and test results.
7. List the exact files you expect to change.

Implementation constraints:

* Target net10.0.
* This is a technical framework upgrade only.
* Preserve all existing features and behaviour.
* Make the minimum necessary changes.
* Do not refactor business logic.
* Do not rename public classes, methods, routes, request models, response models, configuration keys, Kafka topics, Kafka fields or database objects.
* Do not change JSON serialization settings unless required and explicitly explained.
* Do not change Loris package versions unless an actual restore, compilation or runtime incompatibility proves that the existing version cannot be used.
* Do not bulk-upgrade NuGet packages.
* Upgrade only packages that are incompatible with net10.0 or required to resolve a documented build or runtime issue.
* Do not suppress compiler warnings globally.
* Do not add nullable-forgiving operators merely to hide warnings.
* Do not perform formatting-only edits.
* Do not edit projects outside this batch unless a required shared dependency was explicitly included in the approved plan.
* Stop before making any change that may alter business behaviour and explain the issue.

Perform the following:

1. Update only the approved project target frameworks.
2. Restore the affected projects.
3. Resolve compilation failures using the smallest safe change.
4. Build the affected projects.
5. Build the complete solution in Release configuration.
6. Run affected tests.
7. Run the complete automated test suite where practical.
8. Compare warnings and test outcomes against the .NET 6 baseline.
9. Review every change for possible API, JSON, Kafka, database, authentication, dependency-injection, configuration or logging impact.

After implementation, provide:

* Files changed
* Target-framework changes
* Package changes, including justification for each one
* Loris package validation results
* Compilation issues found
* Code changes made and why each was required
* New warnings compared with baseline
* Test results
* Behavioural risks requiring manual validation
* Manual smoke tests for this batch
* Suggested Git commit message

Do not proceed to another batch.






Perform a final regression-focused review of the MDB Gateway migration from .NET 6 to .NET 10.

Do not implement new features or refactor code.

Compare the upgraded branch against the original .NET 6 baseline and identify every change that could affect runtime behaviour.

Review specifically:

* Public API routes, HTTP methods and status codes
* Request and response DTOs
* JSON property names, casing, null handling, converters and enum handling
* Authentication and authorization
* JWT validation
* Dependency-injection registrations and constructor selection
* Middleware order
* Application startup
* Configuration keys and configuration binding
* Environment-specific settings
* HttpClient configuration and outbound MDB Core calls
* Timeouts, retries and TLS behaviour
* Kafka topic names
* Kafka message keys, headers and payloads
* Kafka serializers and deserializers
* Database queries, Dapper mapping and transactions
* Date, time and timezone behaviour
* Batch application startup and exit codes
* Background services
* Logging content, level, structure and SIEM format
* Loris package references and runtime compatibility
* IIS hosting configuration
* Deployment and build pipelines
* Runtime and hosting bundle requirements

For each potentially behavioural change, report:

* File and code location
* Original .NET 6 behaviour
* Expected .NET 10 behaviour
* Risk level
* How to validate it
* Whether a code change is required

Also produce:

1. Full solution build result
2. Full test result
3. New warnings compared with the .NET 6 baseline
4. Package version difference report
5. Public API contract difference report
6. Kafka contract difference report
7. Configuration difference report
8. Deployment prerequisite checklist
9. Manual regression checklist
10. Go/no-go recommendation

Do not claim that the migration has no impact merely because the solution builds. Clearly distinguish compile-time compatibility from verified runtime compatibility.


