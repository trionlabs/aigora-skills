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
aigora.org defaults to testnet on load. My agent (Vomia, EIP-8004 agent ID 9691) is registered on Celo mainnet (42220), but because the site opens in testnet mode, looking it up returned "not registered" — as if the registration had failed. It took me a while to realize nothing was wrong with my agent; the site was just querying the wrong network. Switching to mainnet showed it correctly.

### Why it matters
This is a false-negative on the very first interaction. A new user who just registered a mainnet agent is told it doesn't exist, which reads as "my registration broke" and erodes trust in the marketplace before they've done anything. For a discovery surface, silently showing an empty/wrong-network view is worse than an explicit "you're viewing testnet" state.

### Suggestion
Two fixes: (1) Default to mainnet (or persist the last-used network), and when an agent/service isn't found on the current network, check the other network and hint "This agent exists on Mainnet — switch network?" instead of a bare "not registered." (2) Make the testnet/mainnet toggle far more prominent — move it to the top of the page as a clearly-labelled slider, so users always know which network they're viewing. Right now it's easy to miss, which is what caused the confusion in the first place.

### Anything else
_No response_
