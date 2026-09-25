---
generated: '2026-09-25'
method: generated
name: get-a-free-key
description: Mint a free IBANforge key with no e-mail or card, raise it to 200 requests a month by claiming it, and read its usage.
api: openapi/ibanforge-api-keys-api-openapi.yml
operations: [generateApiKey, claimApiKey, getApiKeyUsage, rotateApiKey]
source: >-
  Grounded in the IBANforge OpenAPI 1.8.0 and https://ibanforge.com/docs/api-keys; operationIds verified
  in openapi/ibanforge-api-keys-api-openapi.yml.
---

# Get a free key

## Steps
1. **Generate** — `generateApiKey` (`POST /v1/keys/generate` with no body at all): returns an `ifk_` key that works on every endpoint, starting at 25 requests a month. Store it; it is shown once.
2. **Claim (optional, needs the human's consent)** — `claimApiKey` (`POST /v1/keys/claim`, key in the Authorization header): a 6-digit code is mailed and the same key rises to 200 a month. Never send an address your human has not handed you for this purpose.
3. **Check usage** — `getApiKeyUsage` (`GET /v1/keys/usage`): allowance, credits left and top-up links.
4. **If the key leaks** — `rotateApiKey` (`POST /v1/keys/rotate`) mints a replacement with the same plan and credits. `revokeApiKey` is irreversible.
