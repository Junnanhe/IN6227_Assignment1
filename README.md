
Review and stabilize my existing Playwright specs using MDBGTW_Playwright_Playbook.md.

Work through one spec at a time, then continue to the next. Identify fragile, redundant or inconsistent cases; consolidate genuine duplicates while preserving meaningful coverage and explaining any removals.

Follow our CSV, page-object, fixture, diagnostics, authentication and cleanup conventions. Fix root causes without weakening assertions, adding skips or masking failures with retries. Verify changed cases independently and each complete spec twice consecutively with retries disabled. Report application/environment blockers honestly.

Keep Git changes clean: record existing changes first and preserve them. Put temporary scripts and run artifacts in ignored locations, remove only temporary files you created, and add narrowly scoped ignore rules where needed. Do not commit credentials, reports, traces, screenshots or authentication state. Never discard unrelated changes.

Before finishing, inspect Git status/diff and leave only intentional source, CSV and configuration changes. Summarize repairs, coverage changes, verification results and remaining issues. Follow the playbook’s efficiency guidance.
