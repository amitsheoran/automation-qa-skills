---
name: visual-regression-testing
description: Catch unintended UI/visual changes by comparing screenshots against an approved baseline, using Playwright's built-in snapshot testing (or Percy/Chromatic for cross-browser cloud diffing), with flake-resistant capture and clear triage of real regressions vs. noise.
---

# Purpose

Produce a reliable visual regression suite that catches unintended UI changes (layout
shifts, broken styles, regressions from a CSS or dependency upgrade) by comparing
rendered screenshots against an approved baseline — while filtering out noise from
fonts, animations, and dynamic content.

## When to use

- A component library or key UI flows need protection against unintended visual changes.
- After a CSS/design-system/dependency upgrade, to catch regressions scripted
  functional tests miss.

## When NOT to use

- Functional correctness (does the button work) → `playwright` / `cypress` / `selenium-java`.
- One-off pixel debugging on a single page — not worth a maintained baseline.

## Inputs

- The pages/components to protect, target viewport(s)/breakpoint(s), and acceptable diff threshold.

## Workflow

1. Pick the tool: **Playwright's `toHaveScreenshot()`** for self-hosted, zero-cost
   baselines; **Percy / Chromatic / Applitools** when you need cross-browser cloud
   rendering and a review UI for designers/PMs.
2. **Stabilize the page before capture:** disable animations/transitions, freeze or
   mock dynamic content (dates, counters, ads), wait for fonts/images to load, mask
   known-volatile regions (e.g. a live chat widget).
3. **Capture baselines deliberately** — review and commit them like code, not by
   rubber-stamping the first run.
4. Set a **diff threshold** (pixel ratio / anti-aliasing tolerance) tight enough to
   catch real regressions, loose enough to survive font/OS rendering noise.
5. On failure, generate a **visual diff artifact** (before/after/diff image) for triage.
6. Update the baseline only via an **explicit, reviewed step** — never auto-accept a
   changed baseline in CI.

## Output format — Playwright

```ts
import { test, expect } from '@playwright/test';

test('product card renders consistently', async ({ page }) => {
  await page.goto('/products/42');
  await page.addStyleTag({
    content: '*, *::before, *::after { animation: none !important; transition: none !important; }',
  });
  await expect(page.getByTestId('product-card')).toHaveScreenshot('product-card.png', {
    maxDiffPixelRatio: 0.01,
    mask: [page.getByTestId('live-stock-counter')],
  });
});
```

## Quality checklist

- [ ] Animations/transitions disabled and dynamic content masked or mocked before capture.
- [ ] Diff threshold tuned — not failing on font/anti-aliasing noise, not missing real regressions.
- [ ] Baselines are reviewed and committed deliberately, not auto-accepted.
- [ ] Failures produce a before/after/diff artifact for triage.
- [ ] Covers the breakpoints/viewports that actually matter for the page.

## Example

See `examples/product-card-regression.md` for a full worked example.
