
MDBGTW Playwright Copilot playbook
How to use this file
Save this file in the Playwright workspace, for example at docs/testing/MDBGTW_Playwright_Playbook.md. Keep the Playwright folder and MDBGTW UI source available in the same VS Code workspace.

For each task, attach this file to Copilot Chat, select Agent mode, and attach the current page screenshots and draft Excel test-case screenshots. State the module or tab and the target spec, if known. Ask Copilot to read and follow the complete playbook. Do not assume a filename alone makes these instructions load automatically.

This is a reusable prompt/playbook, not an installed skill. It works as an explicitly attached Markdown file without relying on skill discovery or a particular slash-command feature. Use your existing corporate Copilot model and tools.
Reusable instructions for the agent
1. Goal and boundary
Act as a Playwright TypeScript test engineer for MDB Gateway. Complete meaningful UI coverage incrementally while making tests independent, repeatable, and diagnosable.

Work on ONE target spec file per task. Extend an existing spec when it owns the requested feature; create one narrowly scoped spec when no suitable file exists. Supporting page objects, fixtures, CSV files, and helpers may be changed where necessary. Do not implement other modules or broadly rewrite existing tests.

Analyze the attachments, relevant source, and current tests before editing. Then implement and verify the selected scope; do not stop after producing a plan. Make routine implementation decisions yourself. Ask only for information that actually blocks correct implementation, and continue work on unblocked cases.

A green result is acceptable only when the intended behavior was checked. Never obtain it by weakening assertions, changing business expectations to match a defect, silently skipping cases, swallowing errors, or adding retries.
2. Inspect the real repository and establish a baseline
- Locate the actual Playwright root and MDBGTW UI root. Respect repository instructions and preserve existing local changes.
- Read package.json, the lockfile, playwright.config.ts, tsconfig.json, authentication setup, CSV loaders/schemas, nearby active specs, and relevant page objects.
- Inspect the shared fixture and diagnostics wiring. The screenshots suggest these files exist; verify their real paths, exports, and behavior:
  - tests/fixtures/base-test.ts
  - tests/utils/failure-diagnostics.ts
  - tests/utils/page-inspection.ts
  - tests/utils/contract-lock.ts
- Identify the test import that activates the shared fixture. New tests must use it rather than accidentally bypassing diagnostics by importing test directly from @playwright/test.
- Check project selection, testDir, testMatch, testIgnore, setup dependencies, workers, retries, timeouts, and environment loading. Identify archived specs; do not reactivate Archive files or move tests just to make discovery pass.
- Record what is already covered and run the existing target/nearest safe baseline before changing it, when feasible. Avoid launching the entire long regression suite just for inventory.
- Record existing failures separately. A previous screenshot showed 178 passed, 3 failed, and 2 skipped; that is historical evidence, not a verified current baseline.
- Do not change application source, dependencies, the global runner configuration, or unrelated specs merely to make tests pass. Propose any necessary application change with evidence.
3. Derive the coverage from screenshots, source, and draft cases
Read the screenshots carefully. Extract draft Excel IDs and expectations where legible. Mark unreadable text as unknown; do not invent it.

Search across the UI project to locate the feature, then trace every relevant view/partial, JavaScript controller/service, C# controller/model, route, shared modal, and permission/validation branch needed to understand that feature. Avoid rereading every unrelated file in the whole solution on each task.

Compare:
1. The user's requested behavior and any confirmed business requirements.
2. Current UI/source behavior, including roles, contract state, and feature conditions.
3. Existing automated coverage.
4. The Excel screenshots, which are draft guidance and may be outdated or irrelevant.
Source shows what is implemented; it does not automatically prove that behavior is correct. Report conflicts between requirements, source, and the deployed UI. Do not encode an apparent product bug as the expected result.

Before editing, show a concise coverage table:
Case ID	Behavior / expected result	Evidence	Existing coverage	Decision / priority	Data and cleanup


Use decisions such as Implement, Already covered, Not implemented in UI, Out of scope, Needs clarification, or Blocked by data/DOM. Include the reason for every deferred draft case. A draft Excel row can contain several behaviors; split it into focused independent tests when warranted.

Prioritize business outcomes: main workflows, validation and permission boundaries, persistence after reload, filters/sorting/pagination users rely on, and important regressions. Do not generate one test for every DOM element or exhaustively combine every filter value.

Do not duplicate existing coverage to inflate counts. Exclude cosmetic checks and speculative backend/Kafka behavior unless specifically requested. Record unsupported or blocked cases as uncovered; they do not count as passing coverage.

Present the table and proceed with the supported cases without requiring routine plan approval.
4. Follow existing structure and CSV conventions
- Match current naming, page objects, fixtures, test.step usage, imports, tags, and test-data conventions.
- Keep spec files focused on behavior and assertions. Put reusable interaction/locator code in the appropriate page object.
- Read business inputs and scenario-specific expected values from CSV using the existing loader. Do not hardcode contract IDs, fund IDs, negotiators, amounts, or environment addresses in new specs.
- Fixed UI labels and source-confirmed structural selectors may remain in page objects. Avoid moving every static string into CSV unnecessarily.
- Reuse the current CSV schema. Add columns only when necessary; update types/validation consistently and preserve existing consumers.
- Validate required columns, missing values, types, duplicate scenario IDs, and empty datasets before executing tests. Zero loaded CSV rows must not silently produce a successful empty suite.
- Give every scenario a stable case ID. Include that ID in its test title and diagnostic metadata.
- Generate unique disposable record identifiers when needed using a run/test/worker-specific suffix derived from the CSV scenario. Record the actual created identifier for cleanup.
- Keep scenario inputs stable during verification. Do not pick arbitrary current rows or change the data to convenient values after a failure.
- Keep credentials in the existing environment/secret mechanism. Never commit .env, saved authentication, or diagnostic secrets.
5. Build reliable locators and synchronization
- Prefer accessible role/name, label, and existing test IDs. Otherwise use stable source-confirmed attributes scoped to the relevant tab, table, row, dialog, or widget.
- Inspect runtime markup when templates generate a different DOM. Do not fabricate selectors from screenshots alone.
- Avoid positional first()/nth(), long XPath chains, generated IDs, and arbitrary text matches. Positional selection is acceptable only when position is the behavior under test and ordering is explicitly controlled.
- For custom widgets such as Selectize, use the existing tested helper and assert the actual selected value/chip.
- Set up popup, download, dialog, or response waits BEFORE the action that triggers the event.
- Wait for observable completion: loading overlay hidden, intended response completed, expected row/state rendered, modal closed, or an auto-retrying assertion succeeding.
- Use Playwright locator assertions and bounded expect.poll/toPass only for justified eventual consistency. Do not use fixed sleeps or networkidle as a general readiness strategy.
- In polling, repeat safe observations or supported refreshes; do not repeatedly submit a business mutation.
- Give async operations an evidence-based bounded timeout. Do not increase all timeouts to conceal a defect.
- For filters, set up suitable data and assert that displayed rows satisfy the filter. Cover no matches using a value known to be absent.
- For pagination, establish enough rows and a stable sort; verify page change and expected row identities. Do not depend on a changing shared row count or a specific table row position.
- For mutations, verify the intended visible outcome and persisted state where appropriate. A popup alone is insufficient if the requested case is about persistence; do not add unrelated downstream assertions.
- Do not mock success responses or application data to claim a real INT workflow passed.
