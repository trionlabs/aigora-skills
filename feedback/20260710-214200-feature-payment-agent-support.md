### Contact
@camilosaka (Telegram)

### CELO payout wallet
0x0a25C91209a158D0a4922837cdd590aCe0D13f0d

### Aigora profile URL
https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_395

### Surface
Registration

### Network
Testnet — Celo Sepolia (chainId `11142220`)

### Problem / motivation
We registered CompraBTC, an autonomous payments agent: users authorize a USDT budget on-chain and the agent executes Bitcoin DCA installments automatically; other agents can consume it through an x402 pay-per-request API charged in USDT. Payment and DeFi agents like this should be core citizens of a Celo agent marketplace, but three parts of the registration form can't describe one:

1. **The skills taxonomy has no finance branch.** The OASF dropdown offers ~35 skills — summarization, RAG, image generation, code generation, translation, etc. Not one covers payments, swaps, DeFi, or anything on-chain. The only honest picks for our agent were "Workflow Automation" and "API / Tool Use" (2 of 35). A buyer filtering the catalog has no way to find "an agent that moves money".

2. **Service price is USDC-only.** Our real x402 endpoint charges 0.02 USDT per call, but the price field only denominates in USDC. We had to declare a price in a currency we don't actually charge.

3. **The service model assumes a conversational agent.** Services are framed as "HTTPS · POST /invoke (SSE) · GET /health" — a prompt-in/stream-out shape. Payment/DeFi agents are hired differently: a public service descriptor (GET /), permissionless on-chain transactions (approve + createPlan on our executor contract), and x402-paid calls per execution. None of that fits the /invoke mold, so what a buyer would actually do with our listed endpoint is unclear from the profile.

4. **A single price field can't express real agent pricing models.** Payment agents often price in layers: ours charges $0.02 per x402 execute call (the price of the listed endpoint) *plus* a product commission of 1% + $0.005 per installment, enforced on-chain by the executor contract on the plan owner. The one-number-per-service field can only show the first layer, so a buyer never sees the full cost of working with the agent.

### Proposed feature
1. Add a finance/on-chain branch to the skill taxonomy, e.g. `finance/payments_x402`, `finance/token_swaps`, `finance/dca_automation`, `finance/treasury_management`, `finance/lending`.
2. Let the service price be denominated in the token the agent actually charges (USDT, USDm, USDC, CELO…), ideally with the token address.
3. Support additional service types beyond Web//invoke: an "x402 endpoint" type (method + price + payTo) and a "service descriptor" type (agents fetch GET / and follow its instructions), so payment agents can be represented truthfully and invoked correctly.
4. Support layered pricing metadata per service: a per-call price plus optional product-level fees (percentage + flat), so listings can disclose the full cost instead of the first layer only.

### Alternatives
We tagged the two closest generic skills (Workflow Automation, API / Tool Use) and declared the price as 0.02 "USDC" although the endpoint actually charges USDT — both are approximations forced by the current form.

### Anything else
CompraBTC context: 61 x402 payments settled on Celo mainnet · agent descriptor: https://comprabtc-production.up.railway.app/ · mainnet ERC-8004 identity: https://www.8004scan.io/agents/celo/9665 · code: https://github.com/csacanam/comprabtc
