### Contact
@camilosaka (Telegram)

### CELO payout wallet
0x0a25C91209a158D0a4922837cdd590aCe0D13f0d

### Aigora profile URL
https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_395

### Surface
Agent profile page

### Network
Testnet — Celo Sepolia (chainId `11142220`)

### Problem / motivation
After registering our agent, we couldn't find any way to confirm it is actually hireable end-to-end — as a builder, the question "is everything OK with my listing? could another agent really contract me?" has no answer inside Aigora today. The advisory verify ping at registration checks an MCP handshake, but there is no buyer-side dry-run of the listed services.

For comparison: Virtuals Protocol (we have used it to create an agent before) has a "hire my agent" simulator — a chat that exercises the listed agent in natural language, so the builder can watch a third party actually contracting it and confirm the listing works. Something in that spirit is missing here, and it matters double while the profile's Services and Capabilities tabs are unresponsive (reported separately): right now there is no way at all to validate a listing from the buyer's perspective.

### Proposed feature
A "Try this agent" panel on the profile page:

- For Web//invoke services: a minimal chat/invoke runner that calls the endpoint and streams the response — the Virtuals-style simulator.
- For other service shapes (x402 endpoints, descriptor-driven agents): a scripted dry-run that shows what a buyer would experience — fetch the descriptor (GET /), display the x402 requirements from a 402 response (price, token, payTo), and simulate the flow without settling a payment.

Builders get a green check that their listing is consumable; buyers get to kick the tires before paying.

### Alternatives
We manually curl our own endpoints to check they respond — but that verifies our infrastructure, not what Aigora's catalog actually presents to a buyer.

### Anything else
Our agent for context: CompraBTC — autonomous Bitcoin DCA on Celo, hired via public descriptor (https://comprabtc-production.up.railway.app/) + on-chain plan creation + x402-paid executions ($0.02 per call, 61 settled on mainnet).
