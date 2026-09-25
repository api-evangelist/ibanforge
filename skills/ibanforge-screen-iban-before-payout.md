---
generated: '2026-09-25'
method: generated
name: screen-iban-before-payout
description: Validate a counterparty IBAN, then run the pre-payout compliance check (sanctions, SEPA/VoP readiness, risk score) before sending funds.
api: openapi/ibanforge-iban-api-openapi.yml
operations: [formatCheckIBAN, validateIBAN, complianceCheck]
source: >-
  Grounded in the IBANforge OpenAPI 1.8.0 (https://api.ibanforge.com/openapi.json); operationIds verified
  in openapi/ibanforge-iban-api-openapi.yml, openapi/ibanforge-free-api-openapi.yml and
  openapi/ibanforge-compliance-api-openapi.yml.
---

# Screen an IBAN before a payout

## Auth
- `Authorization: Bearer ifk_…`, or an x402 payment header, or none: `POST /v1/iban/validate` serves 25 keyless validations a week per source address. See `authentication/ibanforge-authentication.yml`.

## Steps
1. **Free pre-flight** — `formatCheckIBAN` (`GET /v1/iban/format?iban=…`): structure and check digits only, no key, no payment.
2. **Validate and enrich** — `validateIBAN` (`POST /v1/iban/validate` with `{"iban": "…"}`). An invalid IBAN is HTTP 200 with `valid: false` and an `error` code (`wrong_length`, `checksum_failed`, …) — branch on it, it is not an HTTP error.
3. **Compliance check** — `complianceCheck` (`POST /v1/iban/compliance` with exactly one of `iban` or `bic`): sanctions, SEPA Instant reachability, VoP participant status and a 0-100 risk score. It does not check the payee's name.

## Rules
- A `402` means pay or recharge: read `cause.reason` (`trial_exhausted`, `monthly_quota_exhausted`, `credits_exhausted`…). See `errors/ibanforge-problem-types.yml`.
- `429` after 100 requests a minute per IP: wait `Retry-After`. See `rate-limits/ibanforge-rate-limits.yml`.
- A register that is not loaded answers "not consulted" (`null`), never "no" — treat `null` as unknown.
