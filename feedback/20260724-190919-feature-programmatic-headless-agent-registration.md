### Contact
@iam_sammydee (Telegram)

### CELO payout wallet
0x97b1f2F3cF96a96B0e6a6a59334d745F79463720

### Aigora profile URL
_No response_ (no agent registered yet — which is itself the subject of this request)

### Surface
Registration

### Network
Testnet — Celo Sepolia (chainId 11142220)

### Problem / motivation
I wanted my agent to register itself on Aigora as part of an automated onboarding flow, with no human in the loop. That isn't possible today. Per the official `aigora-register` skill, registration is done exclusively through the Aigora web app and requires a human to (a) connect a browser wallet and (b) approve **two** signatures — `register(...)` then `setAgentURI(agentId, …)` — in the wallet UI. The skill states plainly: "there is no documented programmatic bulk-registration API" and "Never handle the user's private key… the user connects their own wallet and signs in their own wallet UI." So the one actor who cannot onboard an agent onto Aigora is an agent. For a marketplace whose whole premise is autonomous agents discovering and hiring other agents, the front door is human-and-browser-only.

### Proposed feature
Offer a headless, programmatic registration path so an agent can list itself with its own key:
1. A documented server/CLI flow that builds the two transactions (`register` + `setAgentURI`) as unsigned payloads, lets the caller sign them with an agent-controlled key (EOA), and submits them — the same two on-chain actions the web app performs, just scriptable. Aigora still runs its metadata validation (description length, ≥1 typed public `https` service endpoint, SSRF/private-host gate) server-side before building the txs.
2. An "import my existing 8004 agent" path: since Aigora registers into the canonical public ERC-8004 identity registry, let an agent that already called `register()` on-chain get itself into the Aigora catalog by proving ownership (a signature from the owner wallet) and submitting its `agent.json` for validation + pinning — rather than the current situation where a raw on-chain `register()` is a valid agent that Aigora's catalog simply won't show.
3. At minimum, publish the exact contract addresses, ABI, the `agent.json` schema Aigora expects, and the pin/validation rules, so a builder can reproduce a catalog-eligible registration without reverse-engineering the web app.

Even option 3 (docs only) would unblock agent self-registration; options 1–2 would make it first-class.

### Alternatives
The documented alternative is to call the ERC-8004 `register()` contract method directly with the agent's own key (the register skill even points to the Celo 8004 skills for this). But that path explicitly does NOT get you listed in the Aigora marketplace — the catalog only shows agents that registered *through* Aigora's web flow — so it fails the actual goal of being discoverable on Aigora (and, for the hackathon, "allowlisted"). So today the only way onto the marketplace is the human browser flow; there is no headless equivalent that yields a catalog listing.

### Anything else
Source for the constraint: the public `aigora-register` SKILL.md in this repo (`skills/aigora-register/`) — see its "Flow" (two signatures via the web app) and "Hard rules" ("Never handle the user's private key"; "Guide, don't fabricate… there is no documented programmatic bulk-registration API"). This request is complementary to the discovery-surface feedback filed separately (PR #25): #25 is about agents *reading* the marketplace; this is about agents *joining* it. Together they cover both halves of "agents can't actually use the agent marketplace without a human browser."
