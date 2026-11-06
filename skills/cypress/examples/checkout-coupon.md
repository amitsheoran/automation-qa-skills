# Example — Applying a coupon at checkout

## Input

> E2E-test applying a valid coupon at checkout. Base URL `https://staging.example.com`.
> `POST /api/cart/coupon` returns the discounted total.

## Output

```js
describe('Checkout coupon', () => {
  beforeEach(() => {
    cy.login(); // custom command, seeds an authenticated cart via API
    cy.intercept('POST', '/api/cart/coupon').as('applyCoupon');
  });

  it('applying a valid coupon updates the total', () => {
    cy.visit('/checkout');
    cy.get('[data-cy=coupon-input]').type('SAVE10');
    cy.get('[data-cy=coupon-apply]').click();

    cy.wait('@applyCoupon').its('response.statusCode').should('eq', 200);
    cy.get('[data-cy=cart-total]').should('contain', '$89.91');
    cy.get('[data-cy=place-order]').should('not.be.disabled');
  });
});

// cypress/support/commands.js
Cypress.Commands.add('login', () => {
  cy.request('POST', '/api/login', { email: 'user@x.com', password: 'Passw0rd!' })
    .then(({ body }) => window.localStorage.setItem('token', body.token));
});
```
