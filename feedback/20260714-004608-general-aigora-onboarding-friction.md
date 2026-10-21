### Contact
@ogazboiz

### CELO payout wallet
0xa479b8c6030cBB01f8E9F6AcB2Ad2C757C81894d

### Aigora profile URL
_No response_

### Surface
Registration

### Network
Testnet — Celo Sepolia (chainId 11142220)

### Your feedback
Registering MARKOV (our ERC-8004 agent, an autonomous on-chain opponent in GameArena) through Aigora was smooth overall, but a few onboarding rough edges cost real time:

1. "Register agent" and "Sign in" are two separate modals and easy to confuse. They look similar and are not clearly differentiated, so I opened the wrong one first.
2. Registration is two signatures (register then setAgentURI), but the wallet gives no context on why there are two. It reads like a double-prompt bug until you already know it is mint-then-pin.
3. After registering, the profile shows name = null for a while as the indexer catches up, with no loading or pending state. It reads as "my registration failed."
4. The onAigora self-declared tag is documented as "not proof of registration," which is an odd trust signal to ship. It invites confusion between self-declared agents and agents truly catalogued through Aigora.

### Why it matters
Onboarding is where you lose builders. Each of these made me pause and second-guess whether registration actually worked. For a hackathon funnel bringing in many first-time agent builders, these small ambiguities compound into support questions and drop-off before an agent ever reaches the catalog.

### Suggestion
- Add a 2-step progress indicator during signing: "1/2 mint identity, 2/2 pin metadata".
- Show an optimistic "indexing..." state on a freshly registered profile instead of name = null.
- Visually separate the Register and Sign-in modals (distinct titles, icons, or a single combined flow).
- Either surface catalog provenance distinctly from the onAigora tag, or drop the self-declared tag, to remove the trust ambiguity.

### Anything else
Agent registered from GameArena (gamearenahq.xyz). MARKOV live A2A endpoint: https://gamearenahq.xyz/api/a2a
