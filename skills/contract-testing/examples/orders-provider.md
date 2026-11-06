# Example — web-storefront consumes orders-service

## Input

> Consumer: `web-storefront`. Provider: `orders-service`. Interaction: the storefront calls
> `GET /orders/{id}` and needs the `id`, `status`, and `total` fields to render an order page.

## Output

```js
// consumer: web-storefront
provider.addInteraction({
  state: 'order 7781 exists and is shipped',
  uponReceiving: 'a request for order 7781',
  withRequest: { method: 'GET', path: '/orders/7781' },
  willRespondWith: {
    status: 200,
    headers: { 'Content-Type': 'application/json' },
    body: like({ id: 7781, status: 'shipped', total: 84.5 }),
  },
});
```

**Provider verification (orders-service):**
1. Pull the latest `web-storefront` pact from the broker (tag: `main`).
2. Seed provider state "order 7781 exists and is shipped" in the test DB.
3. Run `pact-provider-verifier` against the real `orders-service` staging instance.
4. Publish verification results back to the broker.

**CI gate:** `pact-broker can-i-deploy --pacticipant orders-service --version $GIT_SHA
--to-environment production` must pass before the deploy job runs.
