---
name: playwright
description: Generate reliable Playwright tests (TypeScript/Python) using role-based locators, web-first assertions, fixtures, and auto-waiting — no manual sleeps.
---

# Purpose

Produce fast, stable end-to-end tests with Playwright, leaning on auto-waiting, user-facing
locators, and web-first assertions.

## When to use

- E2E/UI automation for modern web apps; cross-browser needs.

## When NOT to use

- Pure API contract checks → `api-testing`.
- Existing Java/Selenium estate you must extend → `selenium-java`.

## Inputs

- The flow, base URL, and preferred language (TypeScript or Python).

## Workflow

1. Use **role/label/text locators** (`getByRole`, `getByLabel`, `getByTestId`) over CSS/XPath.
2. Rely on **auto-waiting** and **web-first assertions** (`await expect(locator).toBeVisible()`).
3. Use **fixtures** for setup (auth via `storageState`, base URL, test data).
4. Isolate tests; avoid shared mutable state; run in parallel.
5. Capture **trace/video/screenshot on failure**; add to CI.

## Output format — TypeScript

```ts
import { test, expect } from '@playwright/test';

test('valid login redirects to dashboard', async ({ page }) => {
  await page.goto('/login');
  await page.getByLabel('Email').fill('user@x.com');
  await page.getByLabel('Password').fill('Passw0rd!');
  await page.getByRole('button', { name: 'Log in' }).click();

  await expect(page).toHaveURL(/\/dashboard/);
  await expect(page.getByRole('heading', { name: 'Dashboard' })).toBeVisible();
});
```

`playwright.config.ts`: set `baseURL`, `trace: 'on-first-retry'`, `retries: 2` in CI.

## Quality checklist

- [ ] Role/label/test-id locators, not brittle CSS/XPath.
- [ ] Web-first `expect` assertions (no manual `waitForTimeout`).
- [ ] Auth/setup via fixtures & `storageState`.
- [ ] Trace/video on failure enabled in CI.
