
Read and follow the attached completely.

Current scope: Contract Details → Funds → DOT Linkage Requests.

Use the attached application screenshots and draft Excel test-case screenshots. Inspect the relevant MDBGTW UI views, partials, JavaScript, C# controllers/models, and existing Playwright tests.

First show a concise coverage table identifying:
- Important cases to implement.
- Cases already covered.
- Draft cases that are irrelevant, not implemented, or blocked.

Then proceed with ONE spec file and its necessary supporting changes. Extend the existing owning spec if appropriate; otherwise create one focused spec.

Preserve our CSV input format and existing page-object conventions. Inspect and reuse the shared fixture, failure diagnostics, page inspection, and contract-lock helpers.

Every test must run independently and repeatedly without leaving data that breaks later runs. Accept/reject/delete cases require disposable data and verified cleanup.

Run changed cases individually, then run the complete target spec twice consecutively with retries disabled. After any fix, restart the two-run verification.

If you cannot establish a reliable locator, request the exact runtime DOM fragment you need. Report genuine application defects and existing unrelated failures honestly.

Finish with implemented/deferred cases, changed files, exact commands, both run results, cleanup results, and diagnostic artifact paths. Stop after this spec.


Read and follow MDBGTW_Playwright_Playbook.md.

Task: diagnose and stabilize the existing MDBGTW Playwright suite. Preserve existing coverage, business expectations, CSV inputs, and project conventions. Do not add new module coverage during this task.

This task overrides the playbook’s stopping rule: repair ONE spec at a time, verify it, then automatically continue to the next failing spec until all identified failures are addressed or specifically blocked.

1. Establish the baseline
- Inspect existing reports, traces, screenshots, logs, and test configuration before rerunning.
- Inventory failing tests, known intermittent failures, existing skips, and discovered test counts.
- Start with the failures shown in my screenshot: Managed Funds pagination, Fund Counterparty Name filtering with no matches, and the unauthorized-user/create-contract/re-login-as-admin scenario.
- Confirm their actual names and locations in the current repository.
- Do not assume every failure is flakiness; distinguish reproducible failures from intermittent ones.

2. Diagnose before editing
Read the failing spec, relevant page objects, CSV data, authentication fixtures, diagnostics helpers, and related MDBGTW UI source.

Investigate:
- Unstable or ambiguous locators.
- Missing waits for table redraws, filters, pagination, modals, or asynchronous updates.
- Shared records, concurrent mutations, stale locks, and incomplete cleanup.
- Role/session leakage, reused authentication state, and incorrect login/logout handling.
- Assumptions about row ordering, counts, existing records, or execution order.
- Actual application defects and environment problems.

For each failure, record the evidence, likely cause, and proposed correction. Do not guess from the test title alone.

3. Make focused repairs
- Reuse the existing base fixture, failure diagnostics, page inspection, and locking helpers.
- Ensure failures capture screenshots, snapshots, URLs, errors, and traces before cleanup.
- Keep business inputs in CSV.
- Preserve meaningful assertions and independently provision or validate prerequisites.
- Ensure cleanup runs after failures and restores test-owned state.
- For role-switching tests, verify the effective user/role and prevent authentication state from leaking into other tests.
- Do not mask failures using retries, fixed sleeps, blanket timeout increases, forced clicks, swallowed errors, skipped tests, weakened expectations, or globally reduced parallelism.
- Do not modify application code to make tests pass. Report genuine product defects with reproduction evidence.
- Request precise runtime DOM when available source and diagnostics are insufficient.

4. Verify each repair
- Run each repaired case independently.
- For previously intermittent cases, perform a focused five-execution check with retries disabled.
- Then run the complete owning spec twice consecutively as separate commands, with retries disabled and no manual data repair between runs.
- Use intended worker settings. A workers=1 run may help diagnose a race, but does not prove the normal configuration is fixed.
- After any further edit, restart the relevant verification.
- Check affected consumers whenever shared helpers change.

5. Verify the suite
After completing the individual repairs, run the full intended suite twice consecutively with retries disabled.

Compare discovery, executed cases, and skips against the baseline. No existing coverage may silently disappear. Explain existing intentional skips; do not add skips to obtain a green result.

If a full-suite run exposes another interaction failure, investigate it, repair the owning spec/helper, repeat focused verification, and restart the final two-run gate. Do not rerun unchanged failures repeatedly hoping for a pass.

6. Report honestly
Provide:
- Each failure, its root cause, fix, and evidence.
- Changed files and shared-helper impact.
- Exact commands and focused/spec/full-suite results.
- Cleanup outcomes and report/artifact locations.
- Remaining product defects, environment blockers, or unverified cases.

Continue useful unblocked work without asking for approval after each spec. If blocked, state exactly what information or access is needed. Never claim the suite is stable unless the required verification actually completed.
