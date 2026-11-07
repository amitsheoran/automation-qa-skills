# Example — Gatling simulation for checkout load

## Input

> Same checkout load test again (SLO: p95 < 500ms, error rate < 1%, 300 concurrent
> users for 10 minutes), for a Scala/JVM team already using Gatling for their other
> load tests.

## Output

```scala
import io.gatling.core.Predef._
import io.gatling.http.Predef._
import scala.concurrent.duration._

class CheckoutLoadSimulation extends Simulation {

  val httpProtocol = http
    .baseUrl("https://staging.example.com")
    .contentTypeHeader("application/json")

  val checkoutScenario = scenario("Checkout")
    .exec(
      http("POST /api/checkout")
        .post("/api/checkout")
        .body(StringBody(session =>
          s"""{"cartId":"cart-${session.userId}","paymentMethod":"card"}"""))
        .check(status.is(201))
    )
    .pause(1)

  setUp(
    checkoutScenario.inject(
      rampUsersPerSec(0).to(30).during(3.minutes), // ramp to ~300 concurrent
      constantUsersPerSec(30).during(10.minutes),  // sustained peak
      rampUsersPerSec(30).to(0).during(3.minutes), // ramp down
    )
  ).protocols(httpProtocol)
    .assertions(
      global.responseTime.percentile(95).lt(500), // SLO: p95 < 500ms
      global.failedRequests.percent.lt(1.0),       // SLO: error rate < 1%
    )
}
```

**CI gate:** `mvn gatling:test` (or the sbt/gradle equivalent) exits non-zero when the
`assertions` block fails, so the SLO check is enforced in the pipeline without parsing
a separate report.
