# Example — Protecting the product card component

## Input

> The product card component (price, image, "Add to cart" button) just got a CSS
> refactor. Protect it from future unintended visual changes. Desktop viewport only
> for now.

## Output

```ts
import { test, expect } from '@playwright/test';

test.describe('Product card — visual regression', () => {
  test('renders consistently at desktop width', async ({ page }) => {
    await page.setViewportSize({ width: 1280, height: 800 });
    await page.goto('/products/42');

    // Stabilize: kill animation/transition noise, freeze the live stock counter
    await page.addStyleTag({
      content: '*, *::before, *::after { animation: none !important; transition: none !important; }',
    });
    await page.waitForLoadState('networkidle');

    const card = page.getByTestId('product-card');
    await expect(card).toHaveScreenshot('product-card-desktop.png', {
      maxDiffPixelRatio: 0.01,
      mask: [page.getByTestId('live-stock-counter')],
    });
  });
});
```

**First run:** no baseline exists — Playwright writes `product-card-desktop.png` and
the test is reviewed and committed manually (not auto-accepted).

**Regression caught:** a later PR changed the card's padding via a shared CSS variable.
The test failed with a 3.8% pixel diff (above the 1% threshold), and the generated
`product-card-desktop-diff.png` artifact showed the button shifted 6px to the right —
caught before merge, not after a design review complaint in production.
