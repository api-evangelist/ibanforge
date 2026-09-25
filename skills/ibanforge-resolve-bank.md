---
generated: '2026-09-25'
method: generated
name: resolve-bank
description: Resolve a BIC/SWIFT code or a Swiss BC-Nummer / IID into the institution behind it, with its source register.
api: openapi/ibanforge-bic-api-openapi.yml
operations: [lookupBIC, lookupChClearing]
source: >-
  Grounded in the IBANforge OpenAPI 1.8.0; operationIds verified in openapi/ibanforge-bic-api-openapi.yml
  and openapi/ibanforge-swiss-clearing-api-openapi.yml.
---

# Resolve the bank behind a code

## Steps
1. **BIC** — `lookupBIC` (`GET /v1/bic/{code}`, 8 or 11 characters): bank name, country, city, LEI and the register it came from. An unknown BIC is HTTP 200 with `found: false`, not a 404.
2. **Swiss IID** — `lookupChClearing` (`GET /v1/ch/clearing/{iid}`, 1-5 digits): institution, SIC / euroSIC / Instant Payments participation and QR-IID data from SIX BankMaster.

## Rules
- Send a real value, never the OpenAPI placeholder literal (`400 placeholder_literal`).
- `sanctions.listed: null` means a list was not loaded — unknown, not clear.
