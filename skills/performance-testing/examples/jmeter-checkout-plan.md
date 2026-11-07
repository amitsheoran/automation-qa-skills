# Example — JMeter plan for checkout load

## Input

> Same checkout load test as the k6 example (SLO: p95 < 500ms, error rate < 1%, 300
> concurrent users for 10 minutes), but the team standardizes on JMeter because the
> perf engineer already has JMeter dashboards wired into Grafana.

## Output

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jmeterTestPlan version="1.2">
  <hashTree>
    <TestPlan testname="Checkout Load Test">
      <elementProp name="TestPlan.user_defined_variables" elementType="Arguments">
        <collectionProp name="Arguments.arguments">
          <elementProp name="BASE_URL" elementType="Argument">
            <stringProp name="Argument.value">https://staging.example.com</stringProp>
          </elementProp>
        </collectionProp>
      </elementProp>
    </TestPlan>
    <hashTree>
      <ThreadGroup testname="Checkout — 300 users / 10 min steady">
        <stringProp name="ThreadGroup.num_threads">300</stringProp>
        <stringProp name="ThreadGroup.ramp_time">180</stringProp>
        <longProp name="ThreadGroup.duration">900</longProp>
        <boolProp name="ThreadGroup.scheduler">true</boolProp>
      </ThreadGroup>
      <hashTree>
        <HTTPSamplerProxy testname="POST /api/checkout">
          <stringProp name="HTTPSampler.domain">${BASE_URL}</stringProp>
          <stringProp name="HTTPSampler.path">/api/checkout</stringProp>
          <stringProp name="HTTPSampler.method">POST</stringProp>
          <boolProp name="HTTPSampler.postBodyRaw">true</boolProp>
          <elementProp name="HTTPsampler.Arguments" elementType="Arguments">
            <collectionProp name="Arguments.arguments">
              <elementProp name="" elementType="HTTPArgument">
                <stringProp name="Argument.value">{"cartId":"cart-${__threadNum}-${__Random(1,99999)}","paymentMethod":"card"}</stringProp>
              </elementProp>
            </collectionProp>
          </elementProp>
        </HTTPSamplerProxy>
        <hashTree>
          <ResponseAssertion testname="Assert 201 Created">
            <stringProp name="Assertion.test_field">Assertion.response_code</stringProp>
            <stringProp name="49586">201</stringProp>
          </ResponseAssertion>
        </hashTree>
      </hashTree>
      <!-- Aggregate Report / Backend Listener (Grafana) configured outside this snippet -->
    </hashTree>
  </hashTree>
</jmeterTestPlan>
```

**SLO check in CI:** run headless with `jmeter -n -t checkout.jmx -l results.jtl`, then
assert on the generated summary: `p95 < 500ms` and `error% < 1` fail the build (via the
`jmeter-maven-plugin`'s threshold checks, or a post-run script parsing `results.jtl`).
