# Example — Signup form test data

## Input

> Generate test data for the signup form: `email` (unique, valid format), `age` (13-120),
> `country` (ISO-3166 alpha-2), `displayName` (1-50 chars, unicode allowed). JSON, seeded
> for reproducibility.

## Output

```json
// seed: 20261106 (reproducible across runs)

// valid
{ "email": "ada.lovelace@example.com", "age": 34, "country": "GB", "displayName": "Ada Lovelace" }

// boundary
{ "email": "a@b.co", "age": 13, "country": "GB", "displayName": "A" }
{ "email": "x".repeat(60) + "@example.com", "age": 120, "country": "JP", "displayName": "x".repeat(50) }

// invalid (should be rejected by validation)
{ "email": "not-an-email", "age": 12, "country": "ZZ", "displayName": "" }
{ "email": "ada.lovelace@example.com", "age": 121, "country": "GB", "displayName": "x".repeat(51) }

// edge / localization / security
{ "email": "jose.garcia@example.com", "age": 29, "country": "ES", "displayName": "José García 🎉" }
{ "email": "'; DROP TABLE users;--@x.com", "age": 30, "country": "US", "displayName": "مرحبا بالعالم" }
```

All records use a fixed seed, so re-running the generator produces the same data set —
useful for reproducing a specific test failure.
