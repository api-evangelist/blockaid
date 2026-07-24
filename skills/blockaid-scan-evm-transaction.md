---
name: Scan an EVM transaction before signing
description: Use Blockaid to simulate and validate an EVM transaction and get a malicious/benign verdict before a wallet signs it.
api: openapi/blockaid-openapi-original.yml
operations: [scan-transaction, scan-json-rpc, scan-user-operations]
---

# Scan an EVM transaction before signing

Blockaid returns a risk verdict (benign / warning / malicious) for a transaction
so a wallet or agent can block a drainer or scam before signing.

## Auth
All requests send the `X-API-Key` header (see `authentication/blockaid-authentication.yml`).
Base URL: `https://api.blockaid.io`.

## Steps
1. Build the transaction object (from, to, value, data) and the interacting
   `metadata.domain` (the dApp origin).
2. `POST /v0/evm/transaction/scan` (`scan-transaction`) with the chain,
   account address, transaction, and metadata. Read `validation.result_type`
   and `simulation` for asset diffs.
3. If the caller only has a raw JSON-RPC request, use
   `POST /v0/evm/json-rpc/scan` (`scan-json-rpc`) instead.
4. For ERC-4337 account-abstraction flows, use
   `POST /v0/evm/user-operation/scan` (`scan-user-operations`).
5. Block or warn the user when the verdict is `Malicious` / `Warning`.

## Conventions & errors
- No idempotency key; scans are safe to repeat (`conventions/blockaid-conventions.yml`).
- 422 returns a `{ detail: [ { loc, msg, type } ] }` validation envelope;
  scan errors return `{ status, error, error_details, description }`
  (`errors/blockaid-problem-types.yml`).
- Correlate support requests with the response `x-request-id`.
