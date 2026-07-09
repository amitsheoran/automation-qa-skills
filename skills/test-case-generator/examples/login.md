# Example — Login feature

## Input (user story)

> As a registered user I want to log in with my email and password so I can access my dashboard.
>
> Acceptance criteria:
> - Valid email + valid password → redirected to dashboard.
> - Invalid credentials → generic error, no lockout hint.
> - 5 consecutive failures → account locked for 15 minutes.
> - Email is case-insensitive; password is case-sensitive.

## Output (test cases)

| ID | Title | Preconditions | Test Data | Steps | Expected Result | Priority | Severity | Type | Automation Scope |
|----|-------|---------------|-----------|-------|-----------------|----------|----------|------|------------------|
| TC-001 | Valid login | Registered active user | `user@x.com` / `Passw0rd!` | 1. Open /login 2. Enter creds 3. Submit | Redirected to /dashboard; session cookie set | P1 | Critical | Positive | Yes |
| TC-002 | Wrong password | Registered user | `user@x.com` / `wrong` | 1. Enter email + wrong pwd 2. Submit | Generic "Invalid email or password"; still on /login | P1 | High | Negative | Yes |
| TC-003 | Unregistered email | No such account | `ghost@x.com` / `x` | 1. Submit | Same generic error (no user-exists leak) | P2 | High | Security | Yes |
| TC-004 | Email case-insensitive | Registered `user@x.com` | `USER@X.COM` / `Passw0rd!` | 1. Submit | Login succeeds | P2 | Medium | Boundary | Yes |
| TC-005 | Password case-sensitive | Registered user | `user@x.com` / `passw0rd!` | 1. Submit | Login fails with generic error | P2 | Medium | Negative | Yes |
| TC-006 | Account lockout | Registered user | 5× wrong password | 1. Submit wrong pwd 5 times | After 5th, "Account locked for 15 minutes" | P1 | Critical | Edge | Candidate |
| TC-007 | Empty fields | — | blank / blank | 1. Submit empty form | Inline validation; no request sent | P3 | Low | Negative | Yes |
| TC-008 | SQL/HTML injection in email | — | `' OR 1=1--` | 1. Submit | Input rejected/escaped; generic error; no 500 | P2 | High | Security | Yes |

**Open questions:** Is lockout per-account or per-IP? Is there a "remember me" option?
