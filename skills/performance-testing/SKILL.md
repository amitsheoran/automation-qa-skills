---
name: performance-testing
description: Design load, stress, spike, and soak tests with clear SLOs and workload models, using k6/JMeter/Gatling, and interpret latency/throughput/error results.
---

# Purpose

Plan and script performance tests tied to explicit SLOs, then interpret the results to find
bottlenecks and capacity limits — not just "run a load test."

## When to use

- Validating latency/throughput SLOs, capacity planning, or diagnosing slowness under load.

## When NOT to use

- Functional correctness → `api-testing` / `test-case-generator`.

## Inputs

- Target SLOs (p95 latency, throughput, error rate), the critical transactions, and expected traffic shape.

## Workflow

1. Define **SLOs/SLAs**: p95/p99 latency, throughput (RPS), error rate, resource ceilings.
2. Build a **workload model**: user journeys, think times, arrival rate, data variety.
3. Choose test **types**: load (expected), stress (find the knee), spike, soak (memory leaks).
4. Script with **k6 / JMeter / Gatling**; parameterize data; ramp gradually.
5. Monitor **both sides**: client latency/throughput/errors **and** server CPU/mem/DB/queues.
6. Report **percentiles (not averages)**, the bottleneck, and the safe capacity.

## Output format — k6

```js
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '2m', target: 100 },  // ramp up
    { duration: '5m', target: 100 },  // steady load
    { duration: '2m', target: 0 },    // ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<400'],  // SLO: p95 < 400ms
    http_req_failed:   ['rate<0.01'],  // < 1% errors
  },
};

export default function () {
  const res = http.get('https://staging.example.com/api/products');
  check(res, { 'status 200': (r) => r.status === 200 });
  sleep(1);
}
```

## Quality checklist

- [ ] SLOs defined before running (thresholds encode them).
- [ ] Realistic workload model (think times, data variety, ramp).
- [ ] Server-side metrics correlated with client results.
- [ ] Results reported as percentiles with the identified bottleneck and safe capacity.
