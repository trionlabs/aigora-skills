### Contact
caxtonacollins@gmail.com · Telegram @caxtonacollins

### CELO payout wallet
0x3E192d109d1dd323375Ac1Ed040f817918E82d63

### Aigora profile URL
https://aigora.org/services/9719

### Surface
Registration

### Network
Mainnet — Celo (chainId 42220)

### What happened?
My wallet already owned two ERC-8004 agents on Celo mainnet — ids 9104 and 9105 — minted directly against the canonical identity registry, not through Aigora.

When I opened the Register tab with that wallet connected on mainnet, the form rendered fully but showed a blocking red error at the bottom:

> Couldn't load this agent's current metadata — its skills/prices live off-chain; editing is blocked to avoid wiping them. This may be temporary — retry.
> reason: load_failed (Failed to fetch)

I was on the **Register** tab trying to create a *new* agent, but the app had put me into an edit flow against a pre-existing agent it found on my wallet, then blocked submission because it couldn't load that agent's off-chain metadata.

The reason it couldn't load it is, I think, straightforward: agents 9104 and 9105 have `tokenURI = https://jahpay.vercel.app/api/agent/manifest` — an https manifest in my own format, not an Aigora-pinned document with Aigora's skills/prices shape. Aigora had never pinned metadata for them because it never registered them. So the fetch/parse fails, and the guard that exists to protect a legitimate edit ends up blocking an unrelated new registration.

The error message compounds the confusion: "editing is blocked to avoid wiping them" is a sensible warning if I were editing, but I wasn't — and it gave me no way to say "ignore those, register a new one." The "This may be temporary — retry" hint also points at the wrong cause; retrying can't help, because the metadata will never parse.

I only got past this by trial and error. A builder who already has an 8004 identity — arguably your most valuable early user, since they've already done the hard part — hits a wall on the primary flow with no documented way through.

### Steps to reproduce
1. Mint an ERC-8004 agent directly against the mainnet identity registry `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`, setting `tokenURI` to any non-Aigora URI (e.g. an https URL serving your own manifest format).
2. Open https://aigora.org with that same wallet, mainnet, Owner mode.
3. Go to the **Register** tab.
4. Observe the red `load_failed (Failed to fetch)` banner and that registration is blocked.

### Logs / console output
```shell
# the pre-existing agents on the wallet
$ cast call 0x8004A169FB4a3325136EB29fA0ceB6D2e539a432 "ownerOf(uint256)(address)" 9105
0x3E192d109d1dd323375Ac1Ed040f817918E82d63

$ cast call 0x8004A169FB4a3325136EB29fA0ceB6D2e539a432 "tokenURI(uint256)(string)" 9105
https://jahpay.vercel.app/api/agent/manifest

$ cast call 0x8004A169FB4a3325136EB29fA0ceB6D2e539a432 "tokenURI(uint256)(string)" 9104
https://jahpay.vercel.app/api/agent/manifest

# UI error text
Couldn't load this agent's current metadata — its skills/prices live off-chain; editing is blocked to avoid wiping them. This may be temporary — retry.
reason: load_failed (Failed to fetch)
```

### Transaction / agent ID
Pre-existing agents that triggered it: 9104, 9105 (owner 0x3E192d109d1dd323375Ac1Ed040f817918E82d63)
Agent eventually registered: 9719 — https://aigora.org/services/9719

### Anything else
Suggested fixes:

1. **Separate the flows.** The Register tab should always create a new agent. Editing an existing one belongs behind "My Agents" → pick agent → Edit. Right now Register silently becomes an edit if the wallet owns anything.
2. **If a wallet owns prior agents, ask.** A simple "You already own agents 9104, 9105 — register a new one, or edit an existing one?" prompt would have removed the wall entirely.
3. **Don't block on unparseable foreign metadata.** If the agent was never registered through Aigora, there are no Aigora-managed skills/prices to wipe, so the guard doesn't apply. Detect that case and let it through.
4. **Make the error say what to do.** `load_failed (Failed to fetch)` with "retry" sends the user down a dead end when the real cause is a permanently incompatible tokenURI. Naming the offending agent id would have made this obvious in seconds.
