# Example — Authorized access-control review of the orders API

## Input

> Authorized internal security review of `orders-service` on staging. Scope: authentication
> and access control for `/api/orders/*`. Two test accounts provided (A, B), no production
> data involved.

## Output

```
Finding:   Broken Object-Level Authorization on GET /api/orders/{id}
Severity:  High
Repro:     Authenticate as account A; request /api/orders/{id} using an order id owned
           by account B → 200 OK with B's full order data (items, address, total).
Impact:    Any authenticated user can read any other user's order history and address.
Fix:       Enforce an ownership check server-side (order.accountId == session.accountId)
           before returning the resource; return 404 (not 403) to avoid existence leaks.
Ref:       OWASP API1:2023 (BOLA)
```

```
Finding:   No rate limiting on POST /api/login
Severity:  Medium
Repro:     Sent 200 login attempts for account A within 60 seconds from one IP; all were
           accepted with no lockout, delay, or CAPTCHA triggered.
Impact:    Enables credential-stuffing / brute-force attacks against user accounts.
Fix:       Add per-account and per-IP rate limiting with exponential backoff; alert on
           sustained high-volume failure patterns.
Ref:       OWASP API4:2023 (Unrestricted Resource Consumption)
```
