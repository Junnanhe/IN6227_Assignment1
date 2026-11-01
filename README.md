
Follow MDBGTW_Playwright_Playbook.md. Investigate execution time in contract-detail-funds.spec.ts first.

Use existing reports/traces to identify the slowest cases and measure time spent in authentication, navigation, actions, waits, locks and cleanup. Distinguish excessive case count from individually slow tests.

Implement evidence-based improvements while preserving coverage, assertions, CSV conventions, isolation and cleanup. Split the spec into focused files only where data ownership makes concurrent execution safe. Keep conflicting mutations coordinated; do not enable fullyParallel globally.

Verify improvements at four workers with retries disabled, following the playbook. Compare before/after durations and failures. Keep generated artifacts and temporary scripts out of Git changes.
