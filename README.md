
MDBGTW Test Runner portal — Copilot Agent prompt
Act as the implementation agent. Build and verify a working portal, not just a plan or UI mockup. Continue through implementation and verification without asking me to approve routine choices. Preserve existing user changes. Ask only for information that genuinely blocks progress, and continue unblocked work.
1. Context and scope
- The existing Razor Pages project Loris.MDBGateway.TestRunner.Web already runs locally. Extend it; do not recreate it or upgrade its framework/packages without a demonstrated need. Inspect its actual target framework.
- The existing TypeScript Playwright suite is inside src/Loris.MDBGateway.Test/Playwright. Discover the actual paths instead of assuming the folder and .csproj names match.
- Keep Playwright in its current location. Do not convert the C# test project into a web app or add a project reference to it. The portal starts the existing Node.js Playwright runner.
- Build for the existing suite, not a hardcoded single-spec demo. Only live verification is limited to one selected business test, plus required authentication/setup dependencies.
- Use Razor Pages, C#, the existing Bootstrap assets and minimal JavaScript. No database, new SPA framework, external CDN, cloud service or pipeline integration is required.
- The final destination is INT only. Build and run on localhost now. Do not deploy or change existing deployment/delivery files in this task.
- Read applicable repository instructions and MDBGTW_Playwright_Playbook.md if present. Preserve its test-quality rules. For THIS portal task, my explicit verification scope overrides full-suite, full-spec and two-consecutive-run requirements: run one suitable existing business test only, plus required dependencies. Do not rewrite existing tests to satisfy the portal.
2. Inspect first, then implement
Record the initial Git status. Inspect the new web project, Playwright/package versions, config/projects/dependencies, CSV loaders and schemas, stable case identifiers, fixtures, authentication/storage-state files, failure diagnostics, cleanup and contract-lock helpers.

Map specs to the CSV files they actually consume; some may use multiple files or shared datasets. Check whether CSV rows generate test cases at discovery time or only provide runtime values. Preserve non-portal CLI execution and existing CSV defaults.

Use focused searches and batch independent reads. Reuse findings rather than repeatedly scanning the entire solution. Choose practical defaults and start implementing. Do not introduce broad refactors or edit MDBGTW application code.
3. User interface
Create a polished, responsive portal titled MDBGTW Test Runner, using plain business-friendly labels. Provide:
1. A searchable test catalogue grouped by module/spec, expandable to individual cases, with checkboxes, Select All/Clear Selection and a selected-case count. Show the actual browser/project where applicable. Keep setup dependencies distinguishable from selectable business tests.
2. An input panel showing datasets required by the selection. Users can download CSV templates/current inputs, upload edited CSVs, preview rows and directly edit values in a browser table. Users should not have to download/upload a file merely to change one value.
3. Clear validation messages naming the dataset, row, field and problem. Show which cases use the data. Offer Reset to Default and CSV export of the current edited inputs.
4. A clear Run Selected button, current run status and Cancel action. Prevent accidental duplicate submissions. Show a server-configured target-environment label; do not let users enter arbitrary target URLs or commands.
5. Live progress: completed/total, passed/failed/skipped/flaky counts where relevant, currently running case(s), elapsed time, setup phase, per-case status/duration and a readable error summary. Use text and icons as well as colour.
6. Persistent run history and a details page with selected cases, input snapshot download, results, View Report and Download Report ZIP.
Handle empty, loading, validation-error, unavailable-runner, queued, setup, running, cancelling, cancelled, interrupted, failed and completed states. Use accessible labels, keyboard navigation and responsive tables. Keep paths, stack traces and raw logs in an optional technical-details area. Do not claim unsupported features work or display fake results.
4. Test discovery and precise selection
Use the installed Playwright version's supported discovery/reporting interfaces, such as list mode with structured output or a small list reporter. Do not scrape spec source with regex as the main discovery method. Discovery must not run test bodies or authentication workflows.

Return real case metadata: spec, project, full title and a stable selection identifier. Exclude archives, node_modules and generated artifacts according to the real Playwright configuration. Handle discovery failures visibly.

Map every request to server-validated discovered tests. Use supported exact selection with file/project constraints and properly escaped full titles when needed. Detect ambiguity; never silently fall back to running the entire suite. Preserve required setup project dependencies.

If edited CSVs affect generated cases/titles, rediscover against the validated input snapshot and reconcile the selection before execution. Do not silently run stale selections. For specs with unsupported or unclear input mappings, show a clear limitation rather than guessing or rewriting their logic.
5. CSV inputs without redeployment
Reuse existing schemas/loaders. Introduce the smallest common extension needed for an explicit per-run input root or manifest supplied through child-process environment/configuration. Keep ordinary Playwright runs using their existing defaults when no override is supplied.

Use a maintainable spec-to-dataset mapping; do not duplicate business input values in C#. Support multiple required CSVs. Copy the resolved inputs into an immutable snapshot before queueing so further edits/uploads cannot change a running or queued test. Preserve shared datasets consistently.

Validate both uploads and browser edits server-side. Use a real CSV parser for quoted values, commas, newlines and UTF-8/BOM handling. Check required headers, types, allowed values, duplicate identifiers, empty datasets and cross-file references as defined by existing loaders. Enforce reasonable upload/row limits. Treat CSV values as data, never executable code. Protect spreadsheet exports against formula injection without silently changing values passed to tests.

Changing supported data values must require neither a Git commit nor deployment. Adding/changing test logic still requires code changes. Keep credentials and authentication tokens out of downloadable input files.
6. Execution and lifecycle
Use a queued background service behind an execution interface. Return a run ID immediately; do not keep the original HTTP request open or use untracked fire-and-forget tasks.

Allow ONE active run initially and a bounded queue. Enforce this server-side, not only by disabling a button. Preserve existing worker configuration and contract locking; do not increase workers or enable fullyParallel. Prevent simultaneous local portal instances using the same run store from launching conflicting executions, or explicitly enforce single-instance operation.

Launch the installed local Playwright CLI through a configured Node executable using ProcessStartInfo, UseShellExecute=false, ArgumentList, the correct working directory and per-process environment variables. Do not build shell commands from user input, modify machine-wide environment variables, or invoke npx in a way that downloads packages during execution.

Reuse existing headless browser settings, authentication dependencies, role isolation, fixtures, failure diagnostics and cleanup. Do not reuse the portal user's browser cookies as the test account. Keep authentication state private and prevent discovery/run processes from racing over shared auth files. Do not expose .env or storage-state files through the portal or report ZIP.

Capture stdout/stderr asynchronously without deadlocks, use bounded log storage, observe process exit and finalize run status even for startup failures, setup failures, crashes or missing report events. A failed test is a completed execution with failing results, not a portal crash.

For cancellation, request graceful termination and allow bounded time for cleanup/reporting; terminate only the owned process tree if necessary. Do not claim forced termination guarantees cleanup. Preserve partial artifacts and show incomplete cleanup explicitly. Closing the browser page must not cancel a run. On app shutdown/restart, handle owned processes and mark unfinished runs honestly; never leave a permanent Running status or automatically repeat a mutating run.
7. Real progress and reporting
Add a small Playwright reporter using the installed supported Reporter API. Emit structured events for run start, case start/end, errors and run end. Keep progress events separate from ordinary console output and redact secrets.

Use a thread-safe server status model and browser polling approximately every two seconds. Reloading/reopening the run page must restore progress. Handle concurrent workers, retries, skips, setup failures and multiple active cases correctly; do not invent percentage estimates or double-count retries as separate completed cases.

Generate live events, HTML and machine-readable final results during the SAME execution. Put all reports, traces and existing diagnostic attachments in the run's own output directory; preserve existing CLI reporter behaviour outside portal runs. Do not rerun tests just to obtain another report format.

Serve reports through validated run-relative paths with their required assets and correct content types. ZIP the complete report and relevant diagnostics, not just index.html. Exclude credentials, auth state, unrelated files and arbitrary source directories. Disable report download until files are finalized; label partial reports clearly when available.
8. Storage, configuration and access
No database: persist each run's manifest, input snapshot, events, status and artifacts under a configurable data directory outside wwwroot and outside published application files. Use atomic status writes. Keep private auth/configuration files separate from downloadable artifacts.

Persist selection, dataset fingerprints, timestamps, relevant non-secret run settings, final outcome and artifact locations. Provide configurable retention by age/count, never delete active runs, and handle disk-full/read-only directories gracefully. For now history is shared within the local prototype; do not imply per-user privacy without implementing identity/authorization.

Validate configured Node, Playwright, browser and writable-data paths. Show actionable prerequisite errors. Prefer installed assets/dependencies, minimize new packages, and do not silently upgrade Playwright or install system tools. Document missing corporate-machine prerequisites if unavailable.

Use antiforgery protection on mutations, safe JSON/HTML rendering, upload/path validation, server-generated file names and no arbitrary file-serving endpoint. Bind the prototype to localhost. Before any non-local/INT hosting, require an explicitly configured portal authentication/authorization policy; use the organization's established approach if available, otherwise fail closed and document the integration still needed. Do not fabricate a working corporate login or confuse operator identity with test-account authentication.
9. INT preparation, without deployment
Document the future INT package layout: published portal plus separately packaged Playwright source/config/data templates and production runtime dependencies, Node.js, browser binaries, .NET hosting prerequisites, service identity/network access, permissions and separate persistent run storage. Do not package node_modules blindly from the developer machine or include secrets.

Document an INT-only deployment job/site, independent of MDBGTW UI, with no PRE/PROD deployment. Do not assume a C# project reference packages the TypeScript suite or that adding appsettings.INT.json restricts deployment. Use the environment conventions actually found in this repository.

Long runs in an IIS web process can be interrupted by recycling/shutdown. Keep the execution interface separable so a dedicated worker/service or pipeline agent can be used later. Clearly document this local background-service limitation and the INT execution-host decision still required; do not claim uninterrupted or durable execution merely because status is saved.
10. Verification, Git hygiene and completion
- Build the new web project and run focused checks for CSV validation/round-trip editing, exact selection, queue/state handling and path/argument safety. Reuse the existing test framework where practical; avoid unnecessary scaffolding and broad solution builds.
- Start the portal locally and verify actual browser interactions if browser tools are available. Use one short, read-only existing business test with suitable data to prove selection -> edited/uploaded inputs -> execution -> real progress -> final result -> report view/download. Required authentication/setup dependencies may also run. Prefer a case where a valid changed input can be observed in the execution evidence.
- Use zero retries for this smoke verification, including applicable project overrides through a portal-only configuration; preserve ordinary suite defaults. Do not run the entire spec/suite, the two-run gate or unrelated mutating tests. Rerun the one case only if an implementation fix makes it necessary, and report actual attempts/results honestly.
- Verify input defaults remain unchanged, refreshing the run page works, persisted history reopens, the ZIP contains the complete report assets, and a second submission cannot start an overlapping run. Use controlled checks rather than another long business-test run where possible.
- If browser/INT access, credentials or dependencies are unavailable, finish all unblocked implementation, identify the exact blocker and give the shortest verification steps. Never claim a test passed if it was not executed. Do not weaken assertions or hide application defects.
- Put generated uploads/reports/events/logs/temp files in ignored storage. Remove only your own temporary development files; never discard existing user changes. Do not commit secrets/auth.json, generated reports or node_modules. Check final Git status/diff and report pre-existing tracked generated files separately rather than silently untracking/deleting them.
- Finish with changed files, startup commands, local URL, configuration/prerequisites, supported specs/input mappings, checks performed, the exact smoke-test result and report location, remaining limitations, and INT preparation steps. Stop after completing this portal task.
Implement now. Choose simple maintainable solutions, keep progress updates concise, and make the real end-to-end flow work before adding cosmetic extras.
