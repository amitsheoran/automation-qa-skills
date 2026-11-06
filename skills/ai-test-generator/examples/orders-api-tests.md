# Example — Generating tests for POST /orders

## Input

> Source of truth: `POST /orders` OpenAPI spec (requires `items[]`, `customerId`; returns
> `201` + `Location` header) plus user story US-118 ("a customer can't check out with an
> empty cart"). Framework: REST Assured (Java). Generate tests.

## Output

```java
@Test
void createOrder_withItems_returns201AndLocation() {
    given().contentType(JSON).body(Map.of("customerId", 42, "items", List.of(Map.of("sku","A1","qty",1))))
    .when().post("/orders")
    .then().statusCode(201).header("Location", notNullValue());
}

@Test
void createOrder_emptyCart_returns400() {
    given().contentType(JSON).body(Map.of("customerId", 42, "items", List.of()))
    .when().post("/orders")
    .then().statusCode(400).body("errors.items", notNullValue());
}
```

**Review note:**

```
Source of truth: user story US-118 + /orders OpenAPI spec
Framework: REST Assured (Java)
Generated: 9 tests (5 happy, 3 negative, 1 boundary)
Reviewed: removed 1 tautological assert (`response != null`); fixed 1 wrong expected
  status for empty cart — the agent asserted 200, but the spec and US-118 both say 400.
Verified: mutated the empty-cart guard in OrderController → 6/9 failed as expected
  (the remaining 3, unrelated to the guard, correctly kept passing).
Gaps for manual follow-up: concurrent checkout of the last unit of stock, idempotency-key reuse.
```
