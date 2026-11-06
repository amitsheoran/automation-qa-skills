# Example — Searching from the dashboard

## Input

> E2E test: a logged-in user searches for a product on the dashboard and sees matching
> results. TypeScript, authenticated via a `storageState` fixture.

## Output

```ts
import { test, expect } from '@playwright/test';

test.use({ storageState: 'auth/standard-user.json' });

test('searching for a product shows matching results', async ({ page }) => {
  await page.goto('/dashboard');
  await page.getByRole('searchbox', { name: 'Search products' }).fill('wireless mouse');
  await page.getByRole('button', { name: 'Search' }).click();

  await expect(page).toHaveURL(/\/search\?q=wireless\+mouse/);
  const results = page.getByRole('listitem', { name: /mouse/i });
  await expect(results.first()).toBeVisible();
  await expect(results).toHaveCount(await results.count()); // sanity: list rendered, not empty
  expect(await results.count()).toBeGreaterThan(0);
});
```

`playwright.config.ts`: `baseURL: 'https://staging.example.com'`, `trace: 'on-first-retry'`,
`retries: 2` in CI. The `auth/standard-user.json` storage state is generated once in a setup
project so every test starts already logged in.
