# Example — Testing the orders endpoint

## Input

> Test coverage for `POST /orders` and `GET /orders/{id}` on the orders service. Auth via
> bearer JWT. Business rule: an order total must equal sum(line items) + tax.

## Output

```java
@Test
void createOrder_returns201_andPersistsWithCorrectTotal() {
    String id =
      given().auth().oauth2(validToken).contentType(JSON)
        .body(Map.of("items", List.of(Map.of("sku","A1","price",20.0,"qty",2)), "taxRate", 0.1))
      .when().post("/orders")
      .then().statusCode(201)
             .header("Location", notNullValue())
             .body("total", equalTo(44.0f)) // (2 * 20.0) * 1.1
             .extract().path("id");

    when().auth().oauth2(validToken).get("/orders/{id}", id)
      .then().statusCode(200).body("total", equalTo(44.0f));
}

@Test
void createOrder_missingAuth_returns401() {
    given().contentType(JSON).body(Map.of("items", List.of()))
    .when().post("/orders")
    .then().statusCode(401);
}

@Test
void getOrder_notFound_returns404() {
    given().auth().oauth2(validToken)
    .when().get("/orders/{id}", "nonexistent-id")
    .then().statusCode(404);
}
```
