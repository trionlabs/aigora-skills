### Contact
@emiridbest

### CELO payout wallet
0x4d4cC2E0c5cBC9737A0dEc28d7C2510E2BEF5A09

### Aigora profile URL
https://aigora.org/services/42220_0x8004a169fb4a3325136eb29fa0ceb6d2e539a432_9691

### Surface
Agent discovery / catalog

### Network
Mainnet — Celo (42220)

### Your feedback
aigora.org defaults to testnet on load. My agent (Vomia, EIP-8004 agent ID 9691) is registered on Celo mainnet (42220), but because the site opens in testnet mode, looking it up returned "agent not found" — as if the registration had failed. It took me a while to realize nothing was wrong with my agent; the site was just querying the wrong network. Switching to mainnet showed it correctly.

### Steps to reproduce
- Browser: Chrome (desktop)
- Session: fresh session, no prior aigora.org visit — so the testnet mode was the site's own default, not a remembered preference
- Initial URL: https://aigora.org
- Network selector on load: Testnet
- Lookup input: mainnet agent ID 9691 (EIP-8004 registry 0x8004a169fb4a3325136eb29fa0ceb6d2e539a432)
- Exact result shown: "agent not found"
- Fix: switching the network selector to Mainnet showed the agent correctly

### Why it matters
This is a false-negative on the very first interaction. A new user who just registered a mainnet agent is told it doesn't exist, which reads as "my registration broke" and erodes trust in the marketplace before they've done anything. For a discovery surface, silently showing an empty/wrong-network view is worse than an explicit "you're viewing testnet" state. Because this was a fresh session, it is the default a brand-new visitor sees.

### Suggestion
Primary fix — make the current network explicit and remembered, independent of any default choice:
1. Persist and restore the user's last-used network across sessions, so a returning user isn't silently reset.
2. Show the current network prominently — a clearly labelled toggle at the top of the page — so a first-time visitor always knows which network they're viewing.
3. When an agent/service isn't found on the selected network, check the other network and surface a hint like "This agent exists on Mainnet — switch network?" instead of a bare "agent not found." This alone would have turned my dead end into a one-click fix.

Whether the out-of-the-box default should be mainnet or testnet is a separate product decision — and worth noting the aigora-register skill (skills/aigora-register/SKILL.md) currently points hackathon users at testnet, so a plain mainnet default could produce the same false-negative for testnet agents. The persistence, explicit-state, and cross-network-hint fixes above avoid the problem regardless of which default is chosen.

### Anything else
_No response_
