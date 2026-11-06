---
name: security-testing
description: Perform authorized, defensive security testing against the OWASP Top 10 — auth, access control, injection, and misconfig — with reproducible findings and remediation.
---

# Purpose

Find and report common web/API security weaknesses (OWASP Top 10) as part of **authorized**
QA/security testing, with clear reproduction and remediation guidance.

> **Scope & ethics:** Use only on systems you own or are explicitly authorized to test
> (pentest engagement, staging, CTF, security research). Never test third-party production
> without written permission. This skill is defensive — it finds and helps fix issues.

## When to use

- Security review of your own app/API; adding security checks to the QA cycle.

## When NOT to use

- Unauthorized targets, DoS, or exfiltration — out of scope, do not proceed.

## Inputs

- The authorized target, scope/rules of engagement, roles, and endpoints.

## Workflow

1. Confirm **authorization & scope** before anything else.
2. **Authentication:** weak passwords, missing lockout, token expiry, session fixation, MFA gaps.
3. **Access control (IDOR/BOLA):** access another user's object by changing an id; privilege escalation; forced browsing.
4. **Injection:** SQL/NoSQL, command, XSS (reflected/stored), SSRF — use safe probe payloads.
5. **Misconfiguration:** security headers (CSP, HSTS), CORS, verbose errors, default creds, open storage.
6. **Sensitive data:** secrets in responses/logs, TLS, PII exposure.
7. Report each finding with **severity (CVSS-style), reproduction, impact, and remediation**.

## Output format

```
Finding:   Broken Object-Level Authorization on GET /api/orders/{id}
Severity:  High
Repro:     Auth as user A; request order id owned by user B → 200 with B's data.
Impact:    Any user can read any order (PII, totals).
Fix:       Enforce ownership check server-side (order.userId == session.userId).
Ref:       OWASP API1:2023 (BOLA)
```

## Quality checklist

- [ ] Authorization/scope confirmed before testing.
- [ ] OWASP Top 10 categories considered (authn, access control, injection, misconfig).
- [ ] Findings are reproducible with concrete steps.
- [ ] Each finding has severity, impact, and a remediation.
- [ ] No destructive actions; no real user data exfiltrated.

## Example

See `examples/orders-api-authz.md` for a full worked example.
