### Contact
@allenrel

### CELO payout wallet
0xB98cFAC37b8bD7f549789718aC17F8aEE7cE0c37

### Aigora profile URL
https://aigora.org/services/42220_0x8004a169fb4a3325136eb29fa0ceb6d2e539a432_9745

### Surface
Agent discovery / catalog

### Network
Mainnet — Celo (chainId `42220`)

### Your feedback
We built Remifi, a hireable USDC payroll and revenue-split agent on Celo mainnet (ERC-8004 agent #9745). Our public agent.json declares web, A2A, OASF skills, x402 support, and live endpoints at https://remifi.up.railway.app/agent.json. Aigora indexed us from ERC-8004, which is useful for marketplace presence.

While preparing our hackathon Track 4 submission we hit a few marketplace friction points:

1. Profile load reliability — opening our Aigora service URL sometimes returns "Agent profile could not be loaded" with no actionable recovery path. That makes it hard for judges or other agents to verify the listing from the URL alone.
2. Capability discovery — as a buyer looking for a payout / payroll service, it is not obvious how to filter the catalog for payments, payroll, or x402-hireable agents versus generic listings.
3. x402 hire signals — Remifi settles hires through the Celo x402 facilitator to our agent payTo wallet, then executes tagged USDC payouts. The profile/catalog does not clearly surface "hire via x402" as a first-class service mode for composable agent-to-agent payment legs.
4. Metadata freshness — after publishing an updated agent.json (endpoints, skills, description), it is unclear how / when the Aigora profile re-syncs from ERC-8004 or the public metadata URL.

### Why it matters
Aigora's job is agent-to-agent discovery. Hireable payment agents need a loadable profile, capability filters, and visible x402 hire hints — otherwise other agents hardcode endpoints and skip the marketplace. That weakens Aigora as the coordination layer for DeFAI payroll and splits on Celo.

### Suggestion
- Show a clearer profile error state with retry and a fallback link to raw agent.json / 8004scan metadata.
- Add catalog filters for capability tags (payments, payroll, x402) and display x402 hire hints on agent profiles.
- Document or provide a one-click refresh from ERC-8004 / published agent.json so marketplace copy stays aligned with the live agent.

### Anything else
Live app: https://remifi.up.railway.app
8004scan: https://8004scan.io/agents/celo/9745
Agent card: https://remifi.up.railway.app/.well-known/agent-card.json
Agent.json: https://remifi.up.railway.app/agent.json
