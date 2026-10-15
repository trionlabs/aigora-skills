# [Feedback:bug] Connect modal doesn't detect installed wallets; WalletConnect spinner never resolves

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
Opening the connect-wallet flow, the modal showed my installed wallet (Rabby, Chrome) as **"not installed"** — no connectable option at all. I installed additional wallet extensions from the Chrome Web Store to work around it; the modal still didn't detect them. WalletConnect was worse: it just kept loading and loading, spinner forever, never a QR or an error.

What finally worked was clearing the browser cache and reloading several times — after that the modal suddenly listed the wallet as installed and connected fine. Then, on the first registration signature (the `register(...)` prompt), I got an error I unfortunately didn't capture before retrying; a retry went through and both signatures ultimately confirmed.

Total cost: most of my registration time was spent fighting the connect modal, not registering. A user less determined stops at "not installed" — they have a wallet, the page says they don't, and there is nothing left to click.

### Steps to reproduce
1. Chrome with Rabby installed (in my case alongside other wallet extensions later — multiple injected wallets is the realistic user setup).
2. Open aigora.org and start the connect-wallet flow.
3. Modal lists the installed wallet as "not installed"; no injected option is connectable.
4. Choose WalletConnect instead → indefinite loading, no QR, no timeout, no error.
5. Clear browser cache, hard-reload (possibly several times) → wallet is now detected and connects.
6. Proceed to Register agent → first signature attempt errored once (text not captured), succeeded on retry.

### Logs / console output
_No response_

### Transaction / agent ID
11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_406

### Anything else
Suggestions from the shape of the failure:

1. **Detect wallets via EIP-6963 provider announcements** rather than (only) probing `window.ethereum` at page load. With multiple wallet extensions installed, `window.ethereum` is a battleground — detection based on it goes stale and misreports "not installed" for wallets that are right there. EIP-6963 events also mean detection recovers without a cache clear.
2. Add a **"rescan wallets" affordance** on the connect modal. The working fix — clear cache and reload repeatedly — is invisible to users; a rescan button (or re-running discovery when the modal opens, not at page load) makes the recovery one click.
3. **WalletConnect needs a timeout state.** An infinite spinner with no QR and no error message gives the user nothing to act on or report. If the relay/session fails, say so.
4. On the register signature step, **surface the error text and keep the form state**. My first `register(...)` attempt failed with an error I couldn't capture before it was gone; whatever it said, a visible, copyable message (and a retry that doesn't feel like starting over) turns "mystery failure" into a reportable bug.

For balance: once connected, the rest of the flow held up — both signatures confirmed and the profile URL worked immediately. The friction is concentrated entirely at the front door.
