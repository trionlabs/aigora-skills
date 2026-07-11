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

### Your feedback
Our agent, CompraBTC, already had an ERC-8004 identity before touching Aigora: agent #9665 on the canonical Identity Registry on Celo mainnet (https://www.8004scan.io/agents/celo/9665), with real on-chain activity behind it (autonomous USDT→WBTC DCA purchases for real users, and 61 x402 pay-per-request payments settled through the Celo facilitator). When we registered on Aigora (on Sepolia, the network the current onboarding drives you to), the flow minted a brand-new agent NFT (#395) on the testnet registry. Our agent now has two ERC-8004 identities: the real one on mainnet, where all its history and reputation live, and a fresh Sepolia one with an empty track record — and the Aigora catalog shows the empty one. Our honest first reaction at the "Register agent (ERC-8004)" step was confusion: "we're already registered on 8004 — why is it asking us to register again?"

### Why it matters
Aigora's pitch is being a consumer-friendly marketplace over the *canonical* ERC-8004 registry — but re-minting instead of recognizing existing identities fragments exactly what the canonical registry is for. Reputation, feedback, and verifiable history accrue to the original identity, while discovery happens on a clone that can't inherit any of it: a buyer browsing the catalog sees an agent with zero track record even when the underlying agent has real, verifiable activity. It also adds friction for the builders most worth listing — agents that already exist on 8004 are precisely the ones with something to show.

### Suggestion
Recognize existing ERC-8004 identities at registration: detect that the connected wallet already owns an agent on the canonical registry (mainnet or testnet), and offer "import/link this agent" — reusing its metadata and pointing the catalog entry at the original identity — instead of defaulting to minting a new one. Even a lightweight version (a "mainnet identity" field on the profile that Aigora verifies on-chain via ownership) would let listings carry their real history.

### Anything else
Agent service descriptor (how other agents consume CompraBTC): https://comprabtc-production.up.railway.app/ · Code: https://github.com/csacanam/comprabtc
