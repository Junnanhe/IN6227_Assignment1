
I need to migrate an existing Microsoft Playwright C# NUnit automation project to a new Playwright TypeScript project.

Source C# project:
`MDBGTW.PlayWright.AutomationTest`

Target TypeScript project:
`MDBGTW.Playwright.TypeScript`

A separate reference project called `Loris.BAILEY.Playwright` shows the TypeScript structure and conventions expected by the tech lead.

For this first task, do not migrate or modify anything yet.

1. Inspect the complete source C# project, including:

   * Pages
   * Tests
   * setup and teardown code
   * login and authentication logic
   * test data and configuration
   * NUnit assertions
   * browser and context creation

2. Inspect the target TypeScript project and its current `playwright.config.ts`.

3. Preserve the existing C# test behaviour, selectors, test coverage and assertions.

4. Plan how each C# file should map to TypeScript:

   * C# page classes → TypeScript page objects
   * NUnit test classes → `.spec.ts` files
   * NUnit setup/teardown → Playwright fixtures, hooks or global setup
   * configuration and credentials → `.env`
   * repeated login → reusable stored authentication state where appropriate

5. Follow these constraints:

   * Use `@playwright/test`.
   * Use TypeScript, not JavaScript.
   * Keep Page Object Model separation.
   * Use Playwright locators and web-first assertions.
   * Use the system-installed Chrome with `channel: 'chrome'`.
   * Do not download Playwright browsers.
   * Do not hardcode credentials.
   * Do not delete or modify the existing C# project.
   * Do not invent selectors, URLs, credentials or test data.
   * Do not migrate all files in one operation.

Output only:

* The proposed target folder structure
* A source-to-target file mapping table
* Authentication migration approach
* Recommended migration order
* Any missing information or risks found

Wait for my approval before editing files.
