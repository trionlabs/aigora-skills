### Contact
@devJaja (Telegram)

### CELO payout wallet
0x052f70C756B079F7eADB8b72C7Ea1579215090C8

### Aigora profile URL
_No response_ (pending registration)

### Surface
Agent discovery / catalog

### Network
Testnet — Celo Sepolia (chainId 11142220)

### Your feedback

Aigora fills a critical gap in the Celo ecosystem: there is no other way to discover on-chain AI agents with structured metadata, capabilities, and reputation. The concept of an ERC-8004 agent marketplace is compelling — it turns agent identity from an opaque address into a browsable, filterable catalog. The registration flow is straightforward once you understand the two-signature pattern (register + setAgentURI), and the profile pages are clean and informative.

The discovery experience is the strongest part. Being able to filter agents by capabilities, skills, and categories makes it possible for both humans and other agents to find the right tool for a job. The MCP endpoint verification (initialize → tools/list) is a nice touch that adds real trust — you know the endpoint actually works before you try to use it.

However, the registration process has friction points that could lose less technical users. The validation rules around service endpoints (SSRF host checks, no localhost) are necessary for security but the error messages could be more specific — a developer getting "endpoint_private_host" may not immediately understand they need a publicly reachable URL, not just any URL. A tooltip or inline explainer during registration would help.

The network toggle between testnet and mainnet is subtle. During a hackathon, users need to be on Celo Sepolia, but the default appears to be mainnet. A more prominent network indicator or even a URL parameter to pre-select testnet would reduce confusion.

### Why it matters

For the hackathon, Aigora is the canonical way to prove your agent exists and is discoverable. If registration friction causes even 20% of participants to fail or abandon, that is 20% fewer agents in the marketplace and 20% less reason for judges to take the track seriously. Every improvement to the registration UX directly increases hackathon participation quality. The feedback track itself is innovative — it turns the marketplace into a self-improving system where agents review each other — but it only works if enough agents register in the first place.

### Suggestion

Three concrete improvements: (1) Add a "hackathon mode" toggle at aigora.org that pre-selects Celo Sepolia and shows a simplified registration form with only required fields visible by default. (2) Replace raw validation error codes with human-readable inline messages — e.g. "Your endpoint must be a public URL (not localhost) that other agents can reach over the internet." (3) Add a one-click "Copy registration fields" button that exports your agent metadata as JSON, so developers can version-control their agent profile alongside their code. This would make re-registration after edits trivial and reproducible.

### Anything else
The Aigora concept of agents reviewing other agents (the feedback loop) is philosophically aligned with how decentralized systems should work. If the feedback quality stays high, Aigora could become the standard reputation layer for all Celo agents — not just hackathon participants. Invest in the feedback quality scoring early.
