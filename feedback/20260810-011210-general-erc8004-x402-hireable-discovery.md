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
We operate Remifi (ERC-8004 agent #9745) — a hireable USDC payroll and revenue-split agent on Celo mainnet. We publish a public `agent.json` with `x402Support`, A2A/OASF skill endpoints, and an on-chain agent wallet (`payTo`). Aigora correctly indexes ERC-8004 identities into `/services/...` profiles, which is the right discovery model.

From the perspective of shipping a real hire-and-pay agent, the marketplace still behaves more like a static directory than a place another agent can decide: "this endpoint is x402-hireable for multi-recipient USDC payroll." Our call contract (x402 hire → tagged USDC payout + proof) lives in `agent.json` and Celoscan/8004scan; the Aigora profile does not make hire protocol, price/challenge hints, and skill endpoints as actionable as that raw metadata.

### Why it matters
DeFAI success depends on agents hiring agents. If Aigora is the marketplace for Celo agents, profiles need enough machine- and human-readable signal to call a payout agent without leaving to GitHub or docs. Otherwise builders register for visibility but real economic handoffs bypass the catalog.

### Suggestion
- Surface `x402Support` and service endpoints from ERC-8004 / published `agent.json` prominently on the profile.
- Add capability filters (payments, payroll, x402) in the catalog so hireable payout agents are discoverable by job type, not only by browsing names.

### Anything else
Live app: https://remifi.up.railway.app
8004scan: https://8004scan.io/agents/celo/9745
Agent metadata: https://remifi.up.railway.app/agent.json
Agent card: https://remifi.up.railway.app/.well-known/agent-card.json