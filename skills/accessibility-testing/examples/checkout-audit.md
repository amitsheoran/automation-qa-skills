# Example — Checkout page audit

## Input

> Audit the `/checkout` page (cart summary, promo code field, "Place order" button)
> against WCAG 2.2 AA before this sprint's release.

## Output

**Automated scan (axe-core via Playwright):** 6 issues flagged.
**Manual checks:** keyboard-only pass, screen reader pass (VoiceOver + Safari), contrast spot-check.

| Issue | WCAG SC | Severity | Where | Fix |
|-------|---------|----------|-------|-----|
| Icon-only "Apply" button for promo code has no accessible name | 4.1.2 Name, Role, Value | Critical | Promo code field | Add `aria-label="Apply promo code"` |
| Cart line-item delete icon not reachable by keyboard | 2.1.1 Keyboard | Critical | Cart summary rows | Make delete control a real `<button>`, not a `<div onclick>` |
| Order total text is 3.2:1 against background | 1.4.3 Contrast (Minimum) | High | Cart summary | Darken text to `#595959` or raise weight/size to the large-text threshold |
| Promo code error isn't announced to screen readers | 4.1.3 Status Messages | High | Promo code field | Wrap the error text in `role="alert"` / `aria-live="polite"` |
| "Place order" button loses its focus outline | 2.4.7 Focus Visible | High | Checkout footer | Restore the default focus ring; don't set `outline: none` without a replacement |
| Heading order skips from `h1` to `h3` | 1.3.1 Info and Relationships | Medium | Page structure | Change "Order Summary" to `<h2>` |

**Keyboard-only pass:** Tab order follows visual order except the delete icon (see finding
above). Esc does not close the promo code tooltip — add a handler.
**Screen reader pass:** cart item count is announced on add/remove; the promo code error
above was silent.

**Verdict:** 2 Critical, 3 High, 1 Medium. Critical issues block release sign-off.
