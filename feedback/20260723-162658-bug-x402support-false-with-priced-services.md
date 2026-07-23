### Contact
caxtonacollins@gmail.com · Telegram @caxtonacollins

### CELO payout wallet
0x3E192d109d1dd323375Ac1Ed040f817918E82d63

### Aigora profile URL
https://aigora.org/services/9719

### Surface
Registration

### Network
Mainnet — Celo (chainId 42220)

### What happened?
I registered my agent (Jahpay, agent id 9719) through the Register form on Celo mainnet. I added six Web/REST services and entered a price in USDC for every one of them (0.001 USDC each). Five of the six are x402-paywalled endpoints — they return HTTP 402 and settle through the Celo x402 facilitator at api.x402.celo.org. (The sixth, `/api/agent/catalog`, is my free discovery endpoint; that price is my own data-entry error, not an Aigora bug.)

After both signatures completed, I read the on-chain tokenURI and fetched the pinned metadata. The agent.json Aigora pinned contains:

    "x402Support": false

Every service in the same document carries a USDC price. Nothing in the form asked me whether I support x402, and nothing let me correct this after the fact.

I expected that entering a per-service USDC price would either mark the agent as x402-capable, or that the form would expose an explicit x402 toggle so I could set it myself.

The practical impact: on a marketplace built for onchain payments, any consumer filtering the catalog for x402-capable agents will skip an agent whose entire catalog is x402. My listing now advertises six paid endpoints and simultaneously declares that it does not support the payment protocol those endpoints use.

### Steps to reproduce
1. Connect a wallet, switch to Owner mode, open the Register tab on mainnet (chainId 42220).
2. Add at least one Web/REST service with a public https URL.
3. Enter a price in the USDC field for that service (e.g. `0.001`).
4. Fill the remaining required fields and complete both signatures.
5. Read `tokenURI(agentId)` from the identity registry `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`.
6. Fetch the resulting `ipfs://` document and inspect the top-level `x402Support` key — it is `false`.

### Logs / console output
```shell
$ cast call 0x8004A169FB4a3325136EB29fA0ceB6D2e539a432 "tokenURI(uint256)(string)" 9719
ipfs://QmYUmFFTbnGjoEtyHABL61HK2Dwdbkj7RMsctCfWb197BS

$ curl -s https://ipfs.io/ipfs/QmYUmFFTbnGjoEtyHABL61HK2Dwdbkj7RMsctCfWb197BS | jq '{x402Support, services: [.services[] | select(.price) | {endpoint, price}]}'
{
  "x402Support": false,
  "services": [
    { "endpoint": "https://www.jahpay.xyz/api/agent/catalog",            "price": "0.001" },
    { "endpoint": "https://www.jahpay.xyz/api/providers/mento-quotes",   "price": "0.001" },
    { "endpoint": "https://www.jahpay.xyz/api/swap/rates",               "price": "0.001" },
    { "endpoint": "https://www.jahpay.xyz/api/agent/recommendation",     "price": "0.001" },
    { "endpoint": "https://www.jahpay.xyz/api/agent/chat",               "price": "0.001" },
    { "endpoint": "https://www.jahpay.xyz/api/agent/premium-analysis",   "price": "0.001" }
  ]
}
```

Confirming the endpoints really are x402-paywalled:

```shell
$ curl -s -o /dev/null -w '%{http_code}\n' https://www.jahpay.xyz/api/agent/premium-analysis
402
```

### Transaction / agent ID
Agent id 9719 — https://aigora.org/services/9719
tokenURI: ipfs://QmYUmFFTbnGjoEtyHABL61HK2Dwdbkj7RMsctCfWb197BS
Identity registry: eip155:42220:0x8004A169FB4a3325136EB29fA0ceB6D2e539a432

### Anything else
Suggested fix, in rough order of preference:

1. Derive `x402Support` from the services — if any service has a non-empty price, set it to `true`.
2. Or add an explicit x402 toggle to the Register form so the builder sets it deliberately, defaulting to on when prices are present.
3. Either way, make the field editable after registration so existing agents can correct it without a full re-register.

It would also help to record *which* asset and network the price is denominated in. The form's price field is labelled USDC but the pinned document stores a bare `"price": "0.001"` string with no asset, decimals, or chain reference — a consumer can't tell 0.001 USDC from 0.001 CELO from the metadata alone.
