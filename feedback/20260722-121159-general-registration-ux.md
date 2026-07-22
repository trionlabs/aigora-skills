### Contact
@benibauer3 / benigbauer37@gmail.com

### CELO payout wallet
0x99D9D4705720D05400dA1a06E33C8870Cd1B33dC

### Aigora profile URL
https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_410

### Surface
Registration

### Network
Testnet — Celo Sepolia (chainId `11142220`)

### Your feedback
I registered the Scout agent for the Celo Agentic Payments & DeFAI Hackathon (Track 4) through Aigora on testnet. Overall the flow worked: connect wallet → pick testnet → Register agent → two signatures → public profile URL.

The main friction was in the Services section. I had a Web / REST endpoint for my GitHub repo (validated with a green check), then tried to add a second public URL (`https://askbots.ai`) into what turned out to be the **Version** field. The UI showed: "Version is invalid or too long (letters/digits/dots/dashes, max 32)." It wasn't obvious that the second text box was version (not another endpoint), so I spent time thinking the Askbots URL itself was rejected.

Price in USDC next to the service was clear. Once Version was set to something like `0.1.0`, registration proceeded and the profile published at `/services/...`.

### Why it matters
Hackathon builders move fast and often paste multiple URLs (repo, product, docs). Mislabeling or placing Version next to the endpoint without a strong label causes failed validation and abandoned registrations — especially for first-time Aigora users who need a profile URL to qualify for the feedback track.

### Suggestion
1. Label the Version field explicitly (e.g. "Version (e.g. 0.1.0) — not a URL") and show a short example placeholder.
2. If the user pastes a URL into Version, show a friendlier error: "This looks like a URL. Add another Service instead, and put a short version like 0.1.0 here."
3. Make "Add another service" more prominent so people don't reuse the Version box for a second endpoint.

### Anything else
Agent: Scout — https://github.com/benibauer3/scout  
Mainnet ERC-8004 (separate from Aigora testnet listing): https://8004scan.io/agents/celo/9678  
Hackathon: Agentic Payments & DeFAI — Track 4 Best Feedback for Aigora
