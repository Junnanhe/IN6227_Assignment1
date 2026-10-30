
6. Make every test independent and safe to repeat
Before implementing a mutating case, define its preconditions, record ownership, postconditions, and cleanup strategy.
- Each test must run alone, in the file, and again on the same environment without relying on another business test.
- Reuse the supported auth setup as a dependency, but use a fresh isolated context per test. Never share a mutable page/session across unrelated tests.
- Browser-context isolation does not reset server-side records. Explicitly manage contract/fund data and permission/role state.
- Do not rely on a prior create-contract test or a mutable shared created-contract file to supply the target. Provision data in the test/fixture or use a validated dedicated CSV fixture.
- Prefer dedicated read-only records for queries and disposable records for mutations.
- Register cleanup as soon as an owned resource is created. Use fixture teardown/finally so cleanup runs after assertion failures. Record enough identifying data to clean up after partial creation.
- Restore the exact prior business values, or delete only records created by this test through a supported action. Do not delete shared records.
- Verify cleanup completion. Report cleanup failure as a failure and retain record IDs plus recovery steps; never swallow it or mark the run clean.
- If accept/reject/delete or a workflow transition cannot be safely reversed, first establish supported disposable-data provisioning/reset. If that is unavailable, document that specific case as blocked instead of consuming shared data.
- Keep UI behavior assertions through the UI. Use only existing approved setup/cleanup mechanisms; do not introduce database scripts, new infrastructure, or an API-testing scope.
- Inspect and reuse contract-lock.ts when shared contract state requires it. Verify the lock covers actual worker/process concurrency and releases on failure. An in-memory lock does not protect separate Playwright workers.
- Prefer isolated records to serial execution. If genuine shared-state constraints require serialization, document them and apply the smallest scope; do not globally force workers=1 to hide races.
- Make failed-run recovery idempotent and limited to test-owned data. A manual cleanup between the two acceptance runs is not acceptable.
7. Use the existing diagnostics and healing workflow
Read the shared fixture and helpers before implementing. Reuse the existing capture path; do not introduce a second competing "healer" framework.

For each failure, capture available evidence before the page/context closes:
- Original error and stack.
- Screenshot of the failing page/dialog.
- Current URL and case ID.
- ARIA/page snapshot and, when necessary, narrowly scoped live DOM.
- Trace retained on failure.
- Relevant console errors and failed requests, if the existing helper supports them.
- Scenario identifier, project, worker/run identifier, and owned record IDs.
Use testInfo.outputPath/attachments or the repository equivalent so files do not overwrite each other across cases, workers, or runs. Capture original failure state before cleanup changes it. Capture cleanup evidence separately.

Use trace retention that works with retries=0; on-first-retry alone will not capture a trace when there is no retry. Ensure manually created contexts receive appropriate diagnostics too.

Redact secrets and keep evidence out of Git. Diagnostic collection failures must not replace the original test error.

When a test fails:
1. Inspect its error, screenshot, snapshot/trace, relevant page object, CSV scenario, and UI source.
2. Classify the cause: test/locator issue, timing/race, invalid preconditions/data, application defect, environment outage, or missing evidence.
3. Make the smallest evidence-based fix to the test/helper when justified.
4. Preserve meaningful assertions. Do not treat skip, catch-and-continue, force-clicks, broad fallback locators, or more retries as a repair.
5. Rerun the failing test. After any edit, restart the final two-run verification.
Healing means diagnosing and correcting a justified test problem. It must not silently change test meaning, retry business submissions, or auto-accept product defects.
8. Ask for missing runtime DOM precisely
If the deployed page cannot be inspected with available approved tools and source is insufficient, request only the missing evidence.

Specify the page/tab and UI state, the exact element, and the needed artifact: e.g. outerHTML of the DOT requests table header and one row, the open modal, widget dropdown, or failure snapshot. Explain what you already checked and which selector/expectation is unresolved.

Ask for relevant permission and state information when needed. Do not request the whole database or full page source if a small fragment suffices. Continue other cases and clearly identify any blocked test. Never claim unobserved runtime selectors were verified.
9. Verification and acceptance
Use the installed Playwright version and existing commands. Do not install tools, upgrade packages, or require MCP services to carry out this workflow.

Verification sequence:
1. Confirm discovery of the target cases under their correct project, including setup dependencies. Verify CSV loading and intended test counts.
2. Run each newly added or materially changed business case independently using a unique title/case-ID filter. Setup dependencies may still run.
3. Run the COMPLETE target spec twice as TWO separate sequential commands with the same input dataset, default intended worker settings, and retries disabled. The second command must start only after the first and its teardown finish.
4. Preserve a separate result summary and available artifacts for each run before the next can overwrite them. No code changes or manual business-data repair between the two successful runs.
5. If either run fails or cleanup leaks state, diagnose/fix and restart the two-run gate from run 1. If the cause is an application/environment blocker, report it honestly instead of claiming completion.
6. If shared code changed, run the affected existing regression specs at the intended concurrency. Verify shared-state interactions relevant to the change. Run required relevant type/lint checks using the existing project tooling.
Example commands, replacing placeholders with the real spec and project:
npx playwright test "tests/<target>.spec.ts" --project=<actual-project> --retries=0 --forbid-only --trace=retain-on-failure
Run that command once, wait for completion, preserve its results, then run it again. Include --headed if the established local setup needs it. Check per-suite retry overrides too; acceptance must not include retried passes.

Use the correct project(s) for the target. Report setup tests separately from business cases so they do not inflate coverage. --repeat-each=2 is useful additional stress testing, but it is not a substitute for two separate whole-spec runs.

Acceptance requires:
- All implemented target business cases pass on both full runs with zero retries.
- No newly hidden, skipped, fixme, expected-failure, or only-marked target cases.
- Independent-case runs succeed.
- No unhandled cleanup failures or leaked test-owned mutable state.
- Existing affected coverage has no new regressions.
- Blocked/deferred cases and pre-existing unrelated failures are explicitly disclosed.
Two successful runs meet this acceptance gate; they are evidence of repeatability, not proof that flakiness is impossible. Do not claim "fully stable" or "all modules complete."

Do not automatically repeat the entire long suite until it happens to pass. Keep iteration focused, investigate causes, and give progress updates during long verification.
10. Final handoff and stopping point
Finish with:
- Target spec and files changed, with the reason for supporting changes.
- Implemented cases mapped to draft IDs/source evidence; already-covered/deferred/blocked cases and reasons.
- Exact commands and independent-case results.
- Run 1 and Run 2 business-case counts, setup counts, failures, skips, retries, and cleanup outcome.
- Result/artifact paths for each run.
- Any affected regression results, pre-existing failures, product bugs, remaining flaky behavior, or manual recovery needed.
- A clear status: Verified, Partially verified, or Blocked.
Stop after this ONE spec and its supporting verification. Suggest the next module, but wait for the next task before implementing it. If runtime access or data is missing, deliver the completed unblocked work and specific blockers without inventing a passing result.
Starter for the current module
Read and follow this playbook. Current scope: Contract Details > Funds > DOT Linkage Requests. Analyze my attached UI and draft Excel screenshots, the related MDBGTW UI source, and existing Playwright coverage. Extend an existing spec if it owns this feature; otherwise create one focused DOT linkage requests spec. Verify source-supported filters, table behavior, permission boundaries, and applicable request actions, choosing meaningful coverage rather than every possible combination. Accept/reject/delete cases require safe disposable data and verified cleanup. Keep inputs in the existing CSV format, reuse the shared fixture/diagnostics/locking, and implement only this spec. Run changed cases independently and pass the complete spec twice consecutively with retries=0. Request exact runtime DOM only where evidence is insufficient. Report existing failures and genuine product bugs honestly.
Reference guidance
The workflow above is tailored to this repository and the supplied screenshots. The agent must verify the actual source and tools.
- Playwright best practices: https://playwright.dev/docs/best-practices
- Playwright CLI options: https://playwright.dev/docs/test-cli
- Playwright trace configuration: https://playwright.dev/docs/test-use-options#recording-options
