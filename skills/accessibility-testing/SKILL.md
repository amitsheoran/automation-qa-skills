---
name: accessibility-testing
description: Test UIs against WCAG 2.2 AA using automated (axe) plus manual keyboard, screen-reader, contrast, and focus checks — with actionable, prioritized findings.
---

# Purpose

Find and report accessibility barriers against **WCAG 2.2 AA**, combining automated scans with
the manual checks tools can't do (keyboard, screen reader, focus order).

## When to use

- Auditing a page/component for accessibility; adding a11y gates to CI.

## When NOT to use

- Visual layout bugs unrelated to a11y → functional/exploratory testing.

## Inputs

- The page/component/flow, and the target conformance level (default WCAG 2.2 AA).

## Workflow

1. **Automated scan** with `axe-core` (Playwright/Cypress plugin or browser extension) — catches ~30–40%.
2. **Keyboard only:** every action reachable/operable via Tab/Shift-Tab/Enter/Space/Esc; visible focus; no traps.
3. **Screen reader:** names/roles/values announced (labels, alt text, ARIA); dynamic updates via live regions.
4. **Contrast:** text ≥ 4.5:1 (3:1 large); don't rely on color alone.
5. **Structure:** logical heading order, landmarks, focus order matches visual order, form labels/errors associated.
6. Map each finding to a **WCAG success criterion** and severity; give a fix.

## Output format

| Issue | WCAG SC | Severity | Where | Fix |
|-------|---------|----------|-------|-----|
| Icon button has no accessible name | 4.1.2 Name, Role, Value | Critical | Header cart button | Add `aria-label="Cart"` |
| Text contrast 3.1:1 | 1.4.3 Contrast (Minimum) | High | Muted helper text | Darken to ≥ 4.5:1 |
| Focus not visible | 2.4.7 Focus Visible | High | Primary buttons | Add visible focus ring |

## Quality checklist

- [ ] Automated axe scan run AND manual keyboard/SR checks done.
- [ ] Each finding maps to a specific WCAG success criterion.
- [ ] Contrast and focus visibility verified.
- [ ] Findings prioritized with concrete fixes.

## Example

See `examples/checkout-audit.md` for a full worked example.
