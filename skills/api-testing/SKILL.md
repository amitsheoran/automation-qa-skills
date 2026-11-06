---
name: api-testing
description: Design and automate REST/GraphQL API tests — status codes, schema, headers, auth, negative cases, and data integrity — with REST Assured / Postman / requests examples.
---

# Purpose

Produce thorough API test coverage: functional correctness, schema validation, auth,
error handling, and data integrity — ready to automate.

## When to use

- Testing REST/GraphQL endpoints at the service layer.

## When NOT to use

- Consumer/provider compatibility across services → `contract-testing`.
- UI flows → `playwright` / `selenium-java`.

## Inputs

- Endpoint(s), method, request/response schema, auth scheme, and business rules.

## Workflow

1. For each endpoint, cover: **happy path**, **validation errors (400)**, **auth (401/403)**,
   **not found (404)**, **conflict (409)**, and **server behavior on bad input**.
2. Assert **status code, response schema, headers, and body values** — not just 200.
3. Validate **auth**: missing/expired/invalid token, wrong role.
4. Check **data integrity**: create → read back → update → delete round-trips.
5. Test **boundaries**: pagination, large payloads, required vs optional fields, types.
6. Keep tests **independent**; set up/tear down data via API.

## Output format — REST Assured (Java)

```java
@Test
void createUser_returns201_andPersists() {
    String id =
      given().contentType(JSON).body(Map.of("email","a@x.com","name","A"))
      .when().post("/users")
      .then().statusCode(201)
             .header("Location", notNullValue())
             .body("email", equalTo("a@x.com"))
             .body("id", notNullValue())
             .extract().path("id");

    when().get("/users/{id}", id)
      .then().statusCode(200).body("name", equalTo("A"));
}

@Test
void createUser_missingEmail_returns400() {
    given().contentType(JSON).body(Map.of("name","A"))
    .when().post("/users")
    .then().statusCode(400).body("errors.email", notNullValue());
}
```

## Quality checklist

- [ ] Positive + negative + auth + boundary cases per endpoint.
- [ ] Assert status, schema, headers, and body — not only status.
- [ ] Data integrity verified via round-trip (CRUD).
- [ ] Tests independent; data seeded/cleaned via API.

## Example

See `examples/orders-endpoint.md` for a full worked example.
