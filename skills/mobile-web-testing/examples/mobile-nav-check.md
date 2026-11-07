# Example — Mobile hamburger navigation

## Input

> On mobile viewports, the top nav collapses into a hamburger menu. Verify it opens,
> the links are tappable, and the on-screen keyboard doesn't break the layout when a
> user taps the search field inside it. Cover iPhone and a Pixel device.

## Output

```ts
import { test, expect, devices } from '@playwright/test';

for (const [name, device] of Object.entries({
  'iPhone 13': devices['iPhone 13'],
  'Pixel 7': devices['Pixel 7'],
})) {
  test.describe(`Mobile nav — ${name}`, () => {
    test.use({ ...device });

    test('hamburger menu opens and links are tappable', async ({ page }) => {
      await page.goto('/');
      const menuButton = page.getByRole('button', { name: 'Menu' });
      await expect(menuButton).toBeVisible();

      const box = await menuButton.boundingBox();
      expect(box!.width).toBeGreaterThanOrEqual(44);
      expect(box!.height).toBeGreaterThanOrEqual(44);

      await menuButton.tap();
      const nav = page.getByRole('navigation');
      await expect(nav).toBeVisible();

      await page.getByRole('link', { name: 'Pricing' }).tap();
      await expect(page).toHaveURL(/\/pricing/);
    });

    test('search field keeps the layout intact when the keyboard opens', async ({ page }) => {
      await page.goto('/');
      await page.getByRole('button', { name: 'Menu' }).tap();
      await page.getByRole('searchbox', { name: 'Search' }).tap();

      // Viewport height shrinks when the virtual keyboard opens on real devices;
      // assert the nav stays reachable rather than being pushed off-screen.
      await expect(page.getByRole('navigation')).toBeInViewport();
    });
  });
}
```

**Result:** passed on iPhone 13; failed on Pixel 7 — the nav's `height: 100vh` pushed
it off-screen once the on-screen keyboard reduced the visible viewport. Fixed by
switching to `height: 100dvh`.
