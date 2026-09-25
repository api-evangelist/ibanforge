---
generated: '2026-09-25'
method: generated
name: clean-iban-list
description: Validate a customer or payout list of up to 100 IBANs per call, and generate register-backed test IBANs to exercise the flow first.
api: openapi/ibanforge-iban-api-openapi.yml
operations: [getTestIban, batchValidateIBAN]
source: >-
  Grounded in the IBANforge OpenAPI 1.8.0; operationIds verified in openapi/ibanforge-iban-api-openapi.yml
  and openapi/ibanforge-free-api-openapi.yml.
---

# Clean a list of IBANs

## Steps
1. **Rehearse (free)** — `getTestIban` (`GET /v1/test-iban?country=DE&count=…`): structurally valid IBANs with bank codes drawn from the national registers (CH, DE, AT, BE, SK); account digits are random and belong to nobody.
2. **Batch validate** — `batchValidateIBAN` (`POST /v1/iban/batch` with `{"ibans": [...]}`), at most 100 IBANs per request (`400 batch_too_large` beyond that). Each IBAN costs one request/credit on a key, or $0.002 via x402.

## Rules
- `402 monthly_quota_insufficient` / `credits_insufficient`: the batch needs more than the key holds and nothing was consumed — split the batch or recharge the key.
- Validation is read-only and safe to retry; respect `Retry-After` on `429`.
