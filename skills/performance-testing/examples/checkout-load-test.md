# Example — Checkout load test for peak sale traffic

## Input

> Validate checkout can sustain Black-Friday-level traffic. SLO: p95 < 500ms, error rate
> < 1%. Expected peak: 300 concurrent users for 10 minutes.

## Output

```js
import http from 'k6/http';
import { check, sleep } from 'k6';

export const options = {
  stages: [
    { duration: '3m', target: 300 },  // ramp up to peak
    { duration: '10m', target: 300 }, // sustained peak
    { duration: '3m', target: 0 },    // ramp down
  ],
  thresholds: {
    http_req_duration: ['p(95)<500'],
    http_req_failed:   ['rate<0.01'],
  },
};

export default function () {
  const res = http.post('https://staging.example.com/api/checkout', JSON.stringify({
    cartId: `cart-${__VU}-${__ITER}`,
    paymentMethod: 'card',
  }), { headers: { 'Content-Type': 'application/json' } });
  check(res, { 'status 201': (r) => r.status === 201 });
  sleep(1);
}
```

**Results interpretation:**

```
p50: 180ms   p95: 640ms   p99: 1.9s   Error rate: 2.3%
Server-side: payments-service CPU pegged at 95% from minute 4 onward; DB connection
  pool (max 50) was saturated during the sustained-peak window.
Bottleneck: payments-service DB connection pool, not the checkout API itself.
Safe capacity: ~220 concurrent users before p95 breaches the 500ms SLO.
Recommendation: raise the connection pool to 100 and re-test before relying on this
  for the Black Friday traffic estimate of 300 concurrent users.
```
