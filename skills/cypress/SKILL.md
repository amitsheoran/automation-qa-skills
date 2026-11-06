---
name: cypress
description: Generate Cypress end-to-end tests with data-* selectors, custom commands, network stubbing (cy.intercept), and retry-aware assertions.
---

# Purpose

Produce readable, resilient Cypress E2E tests that use stable selectors, custom commands for
reuse, and network control for deterministic runs.

## When to use

- E2E/component testing for JS/TS web apps already using Cypress.

## When NOT to use

- Cross-browser at scale or multi-tab flows → consider `playwright`.
- API-only validation → `api-testing`.

## Inputs

- The flow, base URL, and any API responses to stub.

## Workflow

1. Select via **`data-cy` / `data-testid`** attributes, not CSS classes.
2. Extract reusable steps into **custom commands** (`cy.login()`), and use **API-based setup** to seed state fast.
3. Control the network with **`cy.intercept`**; assert on requests where relevant.
4. Rely on Cypress's **automatic retries**; avoid fixed `cy.wait(ms)` — wait on aliases instead.
5. Keep tests independent; reset state via `beforeEach`.

## Output format

```js
describe('Login', () => {
  beforeEach(() => cy.intercept('POST', '/api/login').as('login'));

  it('valid login redirects to dashboard', () => {
    cy.visit('/login');
    cy.get('[data-cy=email]').type('user@x.com');
    cy.get('[data-cy=password]').type('Passw0rd!');
    cy.get('[data-cy=login-submit]').click();

    cy.wait('@login').its('response.statusCode').should('eq', 200);
    cy.location('pathname').should('eq', '/dashboard');
    cy.contains('h1', 'Dashboard').should('be.visible');
  });
});

// cypress/support/commands.js
Cypress.Commands.add('login', (u, p) => {
  cy.request('POST', '/api/login', { email: u, password: p })
    .then(({ body }) => window.localStorage.setItem('token', body.token));
});
```

## Quality checklist

- [ ] `data-cy`/`data-testid` selectors only.
- [ ] Reusable flows in custom commands; state seeded via API.
- [ ] Network stubbed/aliased with `cy.intercept`; no fixed `cy.wait(ms)`.
- [ ] Tests independent and self-resetting.

## Example

See `examples/checkout-coupon.md` for a full worked example.
