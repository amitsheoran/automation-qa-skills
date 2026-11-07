---
name: mobile-web-testing
description: Test responsive web apps on mobile viewports and touch interactions — breakpoints, touch gestures, mobile-specific UI (bottom sheets, hamburger nav), and emulated/real-device checks — distinct from native app automation.
---

# Purpose

Verify a web app actually works on mobile browsers, not just that it resizes: touch
targets, gestures, mobile-only UI, and real device quirks (viewport units, safe areas,
the on-screen keyboard).

## When to use

- A responsive web app needs mobile-browser coverage (Safari iOS, Chrome Android).

## When NOT to use

- Native or hybrid mobile apps → `appium`.
- Desktop-only flows → `playwright` / `cypress` / `selenium-java`.

## Inputs

- The flow, target breakpoints/devices, and whether emulated or real-device coverage is required.

## Workflow

1. **Emulate first, fast:** Playwright's `devices` presets (viewport, user agent, touch)
   for CI; confirm on BrowserStack/real devices before release for the highest-risk flows.
2. **Test touch interactions explicitly:** tap targets ≥ 44x44px, swipe/scroll,
   pinch-zoom where relevant — `click()` alone doesn't prove a touch target works.
3. **Cover mobile-specific UI:** hamburger/bottom nav, bottom sheets, pull-to-refresh,
   the on-screen keyboard shifting the layout.
4. **Check viewport correctness:** `100vh` vs `dvh` issues, safe-area insets
   (notch/home indicator), orientation change (portrait/landscape).
5. **Verify under constrained conditions:** throttled network/CPU for at least the
   critical path.
6. **Confirm parity with desktop** for the same acceptance criteria — don't let
   "mobile" quietly mean "less tested."

## Output format — Playwright

```ts
import { test, expect, devices } from '@playwright/test';

test.use({ ...devices['iPhone 13'] });

test('hamburger menu opens and a nav link works on mobile', async ({ page }) => {
  await page.goto('/');
  await page.getByRole('button', { name: 'Menu' }).tap();
  await expect(page.getByRole('navigation')).toBeVisible();
  await page.getByRole('link', { name: 'Pricing' }).tap();
  await expect(page).toHaveURL(/\/pricing/);
});
```

`playwright.config.ts`: add `Mobile Safari` / `Mobile Chrome` projects using
`devices['iPhone 13']` / `devices['Pixel 7']` alongside the desktop projects.

## Quality checklist

- [ ] Tested via `tap()`/touch interactions, not just `click()`.
- [ ] Covers at least one iOS and one Android device profile.
- [ ] Mobile-specific UI (nav, sheets, keyboard) explicitly checked.
- [ ] Viewport/safe-area and orientation-change issues checked.
- [ ] Same acceptance criteria as desktop — no silently lower bar for mobile.

## Example

See `examples/mobile-nav-check.md` for a full worked example.
