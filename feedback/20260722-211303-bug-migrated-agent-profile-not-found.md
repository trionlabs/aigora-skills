### Contact
hamatthan@gmail.com

### CELO payout wallet
0xF9aa21D3921C7F292738D4E5864EaE3543081E98

### Aigora profile URL
https://aigora.org/services/9232

### Surface
Agent profile page

### Network
Mainnet — Celo (chainId `42220`)

### What happened?
I had an agent already registered directly on-chain in the ERC-8004 Identity
Registry (agentId 9232), predating any use of Aigora. In "My Agents" it showed
up correctly with a "Migrate to Aigora" action. I clicked it, filled in the
required fields (description, a Web service endpoint, OASF skills/domain,
categories, enabled the hackathon toggle), and submitted. The app showed a
"Agent migrated to Aigora 🎉" success modal after a single wallet signature,
consistent with what the flow promised ("one signature repoints its on-chain
metadata, no funds transferred").

The `setAgentURI` transaction confirmed at `2026-07-22T14:54:54Z`. As of
`2026-07-22T22:18:43Z` (~7.5 hours later), `https://aigora.org/services/9232`
still shows "agent not found", and "My Agents" still shows the "Migrate to
Aigora" button on that listing instead of reflecting the migrated state.

I verified independently that this isn't a chain or pinning problem:
- The wallet's `setAgentURI` transaction to the Identity Registry
  (`0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`) is confirmed with status `ok`.
- `tokenURI(9232)` on that contract currently returns
  `ipfs://QmdEVc6KY57RhxuqqNuV8taDqLV9CSjFYvRj4kXWEXtPMK`.
- That CID is live and fetches in full from a public IPFS gateway
  (`ipfs.io`), returning exactly the metadata submitted in the form (name,
  description, services, OASF skills/domains, categories, external links,
  `onAigora: true`).

So the on-chain state and the pinned metadata are both correct and publicly
retrievable. I can't see Aigora's backend, so I can't confirm the root cause —
but this rules out a chain or IPFS-pinning problem, leaving something in
Aigora's own read path (indexer, cache, or query layer) as the most likely
explanation for why the UI hasn't picked up the update.

### Steps to reproduce
1. Have an ERC-8004 agent already registered directly on-chain (not
   originally through Aigora), with a `data:` URI rather than an IPFS one.
2. Connect that agent's wallet on aigora.org, on Mainnet.
3. In "My Agents", click "Migrate to Aigora" on the existing listing.
4. Fill in the required fields and submit; sign the single transaction.
5. Get the "Agent migrated to Aigora 🎉" success modal.
6. Visit `…/services/<agentId>` — expect the profile; get "agent not found".
7. Return to "My Agents" — expect the listing to reflect the migrated
   state; it still offers "Migrate to Aigora" as if nothing happened.

### Logs / console output
No browser console/network capture from the original session. I tried to
find a public REST API to pull request-level diagnostics independently —
`/api/agents/9232`, `/api/services/9232`, `api.aigora.org/agents/9232`,
`api.aigora.org/services/9232`, `/_next/data/agents/9232.json` — all of
these resolve to the SPA's HTML shell rather than a distinct API response,
so I couldn't capture a failing request/response from outside a browser.
Can provide DevTools Network/Console output on request if that helps
triage.

### Transaction / agent ID
Tx: `0x38f2c222148190910ec9183d6d4b2ff111be2fc3f9aa620cc7f5c6ca53997efb`
Agent ID: `9232`

### Anything else
Happy to re-check anytime — this agent is live and this state was still
reproducing as of `2026-07-22T22:18:43Z`.
