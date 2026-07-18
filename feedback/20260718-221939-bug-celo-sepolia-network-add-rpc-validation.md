# [Feedback:bug] Celo Sepolia network-add fails wallet RPC validation despite a live endpoint

### Contact
jadonsunshine@gmail.com

### CELO payout wallet
0x3a3a9fD9dF6B4Eb1DdB6bF8a4e4D29756920Cfe0

### Aigora profile URL
https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_406

### Surface
Wallet / connection

### Network
Testnet — Celo Sepolia (chainId 11142220)

### What happened?
Registering on testnet with Rabby, the very first step — adding Celo Sepolia as a custom network — dead-ended. The add-network dialog came pre-filled with chainId `11142220` and RPC `https://forno.celo-sepolia.celo-testnet.org/`, but Rabby's health check flagged it: **"RPC invalid or currently unavailable"**, and the save button stayed blocked. The endpoint was actually fine — `eth_chainId` answered `0xaa044c` from curl at the same minute. So a registration that should take ten minutes stalled at minute one on a false negative, before Aigora itself was ever reached.

What eventually unblocked it: retrying the check (it is intermittent), dropping the trailing slash from the RPC URL, or swapping in an alternate RPC (`https://celo-sepolia.drpc.org` or `https://rpc.ankr.com/celo_sepolia` — both serve the same chainId). Adding the chain via chainlist.org also works and skips the manual form entirely.

### Steps to reproduce
1. Use a wallet that does not yet have Celo Sepolia configured (Rabby here; its custom-network form runs an RPC health check before allowing save).
2. Go through the Aigora testnet registration flow until the app prompts the network switch / add, or add the network manually with the suggested values.
3. RPC URL as suggested, with the trailing slash: `https://forno.celo-sepolia.celo-testnet.org/`.
4. Rabby intermittently reports "RPC invalid or currently unavailable" and blocks saving — while the same endpoint answers JSON-RPC normally outside the wallet.

### Logs / console output
```shell
$ curl -s -X POST https://forno.celo-sepolia.celo-testnet.org/ \
    -H "content-type: application/json" \
    -d '{"jsonrpc":"2.0","id":1,"method":"eth_chainId","params":[]}'
{"jsonrpc":"2.0","result":"0xaa044c","id":1}   # 0xaa044c = 11142220 — the RPC is up
```

### Transaction / agent ID
11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_406

### Anything else
Two cheap fixes on Aigora's side would remove this class of failure for every wallet, not just Rabby:

1. When prompting the network add (`wallet_addEthereumChain`), pass **multiple `rpcUrls`** — forno plus one or two public alternates — and **without the trailing slash**. Wallets that health-check pick the first one that answers, so one flaky or strictly-parsed URL stops being a hard blocker.
2. On the registration page, list the alternate RPCs (or link the chainlist.org entry for 11142220) next to the "switch to testnet" step. The first thing a new registrant does is fight their wallet's network form; a one-line escape hatch there saves the funnel's first step.

Worth stressing: everything after this point was smooth — both signatures confirmed quickly and the profile URL worked on the first load. The only real friction in the whole flow was this pre-Aigora network hurdle, which is exactly why it deserves handling by the app.
