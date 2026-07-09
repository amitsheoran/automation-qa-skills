---
name: test-data-generator
description: Generate realistic, boundary-aware, PII-safe synthetic test data (valid, invalid, edge, and localized) for manual and automated tests.
---

# Purpose

Produce the *right* data to exercise a feature: valid records, invalid inputs, boundary values,
and edge/localized cases — always synthetic, never real customer PII.

> Suggested addition beyond the original spec — most flaky/weak tests come from weak data.

## When to use

- Seeding tests, filling forms, load fixtures, or covering boundary/negative inputs.

## When NOT to use

- Designing the *cases* themselves → `test-case-generator`.

## Inputs

- The data model (fields, types, constraints), volume needed, and locale(s).

## Workflow

1. For each field, derive **valid**, **invalid**, and **boundary** values from its constraints.
2. Include **edge cases**: empty, min/max length, unicode/emoji, leading/trailing spaces, SQL/HTML injection strings, timezone/DST dates, negative/zero numbers.
3. Add **localization**: names, addresses, phone formats, RTL text, non-ASCII.
4. Keep it **PII-safe**: synthesize (Faker-style) or mask; never copy production data.
5. Ensure **referential validity** where records relate (FKs resolve).
6. Emit in the requested format (JSON / CSV / SQL inserts / code fixtures) and make it **reproducible** (fixed seed).

## Output format

```json
// valid
{ "email": "ada.lovelace@example.com", "age": 30, "country": "GB" }
// boundary
{ "email": "a@b.co", "age": 0, "country": "GB" }          // min age
{ "email": "x".repeat(64) + "@example.com", "age": 150 }   // max lengths
// invalid (should be rejected)
{ "email": "not-an-email", "age": -1, "country": "ZZ" }
// edge / security
{ "email": "'; DROP TABLE users;--@x.com", "name": "José 🚀 مرحبا" }
```

## Quality checklist

- [ ] Valid, invalid, boundary, and edge sets all provided.
- [ ] No real PII — fully synthetic or masked.
- [ ] Localized / unicode / injection cases included.
- [ ] Related records keep referential integrity.
- [ ] Generation is reproducible (seeded) and in the requested format.
