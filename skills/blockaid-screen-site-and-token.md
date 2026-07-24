---
name: Screen a dApp site and token before interacting
description: Use Blockaid to scan a dApp URL for phishing/drainer behavior and a token for rug-pull and financial risk before a user interacts.
api: openapi/blockaid-openapi-original.yml
operations: [scan-site, scan-token, scan-token-bulk]
---

# Screen a dApp site and token before interacting

## Auth
Send the `X-API-Key` header. Base URL: `https://api.blockaid.io`.

## Steps
1. `POST /v0/site/scan` (`scan-site`) with the dApp URL. Inspect the returned
   verdict and detected attack type (drainer, phishing, etc.).
2. `POST /v0/token/scan` (`scan-token`) with the chain and token address to get
   a financial-risk / rug-pull assessment (holders, market, malicious flags).
3. To screen many tokens at once, use `POST /v0/token-bulk/scan`
   (`scan-token-bulk`).
4. Combine both verdicts: block interaction when either the site or the token
   is flagged malicious.

## Conventions & errors
- Repeatable, no idempotency key required.
- Validation failures return 422 with a `detail[]` array; unreachable hosts
  return 400 (`errors/blockaid-problem-types.yml`).
- For continuous token monitoring, register a webhook via
  `POST /v0/token/hooks/{chain}` (`asyncapi/blockaid-token-webhooks.yml`).
