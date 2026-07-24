---
name: Screen an address for risk and compliance exposure
description: Use Blockaid to validate whether an address is associated with malicious activity and to compute its compliance risk exposure.
api: openapi/blockaid-openapi-original.yml
operations: [scan-address, address-risk-exposure, scan-address-bulk]
---

# Screen an address for risk and compliance exposure

## Auth
Send the `X-API-Key` header. Base URL: `https://api.blockaid.io`.

## Steps
1. `POST /v0/evm/address/scan` (`scan-address`) with the chain and address to
   get a validation verdict (benign / malicious) and the reasons.
2. For screening many addresses in one call, use
   `POST /v0/evm/address-bulk/scan` (`scan-address-bulk`).
3. For compliance/AML posture, `POST /v0/address/risk-exposure`
   (`address-risk-exposure`) returns category risk against your organization's
   configured verdict thresholds
   (`GET /v0/platform/organization/configuration/risk-exposure`).
4. Take the enforcement action (allow / warn / block) your policy dictates.

## Conventions & errors
- Repeatable, no idempotency key.
- Paginated management/search endpoints use `page` / `page_size`.
- 422 validation envelope `{ detail: [...] }`; correlate with `x-request-id`
  (`conventions/blockaid-conventions.yml`, `errors/blockaid-problem-types.yml`).
