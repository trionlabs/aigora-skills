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

### Problem / motivation
The service rows on the Register form offer three types — Web / REST, MCP, A2A — and the Web / REST row carries the hint:

    HTTPS · POST /invoke (SSE) · GET /health

That describes one specific agent-invocation convention: a single streaming entrypoint plus a health probe. It doesn't describe how x402 APIs on Celo are actually built, including mine.

Jahpay exposes six independent REST resources, each paywalled at its own price:

| Method | Endpoint | Price |
|---|---|---|
| GET  | /api/agent/catalog            | free (discovery) |
| GET  | /api/providers/mento-quotes   | 0.001 USDC |
| GET  | /api/swap/rates               | 0.001 USDC |
| POST | /api/agent/recommendation     | 0.001 USDC |
| POST | /api/agent/chat               | 0.001 USDC |
| GET  | /api/agent/premium-analysis   | 0.001 USDC |

There is no `/invoke`. There is no `/health`. Each resource is its own product with its own price, which is the whole point of pay-per-request — the granularity *is* the business model.

Two concrete gaps fall out of this:

1. **No way to declare the HTTP method.** Three of my endpoints are GET and two are POST. The form has no method field, so the pinned metadata records only a bare URL. A consumer agent reading my listing cannot tell whether to GET or POST, and has to guess or read human documentation — which defeats machine discovery.
2. **The hint misleads builders.** It reads as a spec, so a builder's reasonable conclusion is that their API is shaped wrong and needs an `/invoke` wrapper. Nothing enforces it (any URL is accepted), so the net effect is confusion rather than a constraint.

The field being non-blocking is what stopped this from being a bug report — I registered fine. But the resulting metadata is less useful than it could be, and I nearly restructured a working API because of a form hint.

### Proposed feature
1. **Add a method selector per service** (GET / POST, defaulting to GET), and record it in the pinned document alongside the endpoint and price.
2. **Loosen or rename the Web / REST type.** Either drop the `/invoke` + `/health` hint for the generic type, or keep that convention under a clearly named type (e.g. "Agent Invocation (SSE)") and add a plain "HTTP endpoint" type for individually-priced REST resources.
3. **Support importing an x402 catalog.** This is the one I'd value most. Jahpay already publishes machine-readable discovery at two well-known locations:
   - `https://www.jahpay.xyz/.well-known/x402`
   - `https://www.jahpay.xyz/api/agent/catalog`

   The catalog document already contains, per service: endpoint, method, price in atomic units, human price, description, and example params — plus the x402 network, asset and facilitator. If the Register form accepted a single catalog URL and imported the services from it, I'd have entered one field instead of six, the method and asset would be captured correctly, and the listing would stay in sync when I change prices instead of going stale the moment I redeploy.

### Alternatives
- **Registering only `/api/agent/catalog`** and letting buyers discover the rest from it. Cleaner to enter, but then only one service shows in the marketplace and the individual products are invisible to catalog search.
- **Adding an `/invoke` wrapper** over the six endpoints to match the hinted shape. Rejected — it would collapse six independently-priced products into one entrypoint and break the per-request pricing model, purely to satisfy a form hint.
- **Putting the method in the description text.** Not machine-readable, so it doesn't help discovery.

### Anything else
Sample of the existing catalog document, to show what an import could consume:

```shell
$ curl -s https://www.jahpay.xyz/api/agent/catalog | jq '{x402, service: .services[0]}'
{
  "x402": {
    "version": 1,
    "network": "celo",
    "facilitator": "https://api.x402.celo.org",
    "asset": "USDC"
  },
  "service": {
    "endpoint": "https://jahpay.xyz/api/providers/mento-quotes",
    "method": "GET",
    "priceUsd": 0.001,
    "priceAtomic": "1000",
    "description": "Oracle-priced Mento swap quote for a token pair and amount",
    "params": { "from": "USDC", "to": "USDT", "amount": "100" }
  }
}
```

The `.well-known/x402` document is richer still — it carries the payee address, the asset contract with its decimals, and the required amount in atomic units per resource:

```shell
$ curl -s https://www.jahpay.xyz/.well-known/x402
{
  "x402Version": 1,
  "network": "celo",
  "facilitator": "https://api.x402.celo.org",
  "payTo": "0x3E192d109d1dd323375Ac1Ed040f817918E82d63",
  "asset": {
    "address": "0xcebA9300f2b948710d2653dD7B07f33A8B32118C",
    "symbol": "USDC",
    "decimals": 6
  },
  "resources": [
    {
      "resource": "https://jahpay.xyz/api/providers/mento-quotes",
      "method": "GET",
      "maxAmountRequired": "1000",
      "description": "Oracle-priced Mento swap quote for a token pair ..."
    }
  ]
}
```

Everything Aigora's service rows ask for — and the asset, decimals and method they don't — is already in there, at a standard well-known path.

Related: the pinned metadata stores prices as a bare `"price": "0.001"` with no asset or decimals — I've raised that in my separate `x402Support` bug report, but an import path would fix it as a side effect, since the catalog carries the asset and atomic amount explicitly.
