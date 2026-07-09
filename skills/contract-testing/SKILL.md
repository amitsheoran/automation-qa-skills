---
name: contract-testing
description: Design consumer-driven contract tests (Pact / Spring Cloud Contract) so microservices stay compatible without slow, brittle end-to-end integration runs.
---

# Purpose

Catch breaking API changes between services *before* deploy by verifying each provider against
the contracts its consumers depend on — fast and independent of full E2E.

> Suggested addition beyond the original spec — increasingly essential for microservice QA.

## When to use

- Multiple services communicating over HTTP/messaging where independent deploys risk breakage.

## When NOT to use

- A single monolith, or you just need endpoint correctness → `api-testing`.

## Inputs

- The consumer(s), the provider, and the interactions (request → expected response) that matter.

## Workflow

1. **Consumer side:** write expectations for each interaction; generate a **contract (pact)**.
2. Publish contracts to a **broker** (Pact Broker) tagged by consumer version/branch.
3. **Provider side:** replay contracts against the real provider (with provider states/seed data).
4. Gate deploys with **`can-i-deploy`**: only ship if all consumer contracts still verify.
5. Version and tag contracts; verify against `main` and feature branches.

## Output format — Pact (JS consumer)

```js
provider.addInteraction({
  state: 'user 42 exists',
  uponReceiving: 'a request for user 42',
  withRequest: { method: 'GET', path: '/users/42' },
  willRespondWith: {
    status: 200,
    headers: { 'Content-Type': 'application/json' },
    body: like({ id: 42, email: 'a@x.com', name: 'A' }),
  },
});
```

Provider verification (pseudo): load pacts from broker → set provider state `user 42 exists`
→ run verification → publish results. CI runs `pact-broker can-i-deploy` before release.

## Quality checklist

- [ ] Contracts are consumer-driven (only fields the consumer needs).
- [ ] Provider states seed the data each interaction requires.
- [ ] Contracts published to a broker and versioned/tagged.
- [ ] `can-i-deploy` gate wired into CI before release.
