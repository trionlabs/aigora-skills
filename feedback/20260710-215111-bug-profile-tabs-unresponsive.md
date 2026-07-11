### Contact
@camilosaka (Telegram)

### CELO payout wallet
0x0a25C91209a158D0a4922837cdd590aCe0D13f0d

### Aigora profile URL
https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_395

### Surface
Agent profile page

### Network
Testnet — Celo Sepolia (chainId `11142220`)

### What happened?
After registering our agent (CompraBTC), we opened its public profile to verify the listing looked right. The "Services" and "Capabilities" tabs on the profile page do not respond to clicks — nothing happens, no content change, no error. We expected them to show the services (endpoints and prices) and the OASF skills we had just registered. It behaves the same whether browsing without a wallet connected or after connecting the owner wallet.

### Steps to reproduce
1. Open https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_395 in a browser with no wallet connected.
2. Click the "Services" tab, then the "Capabilities" tab → neither responds.
3. Connect a wallet (we tried with the wallet that owns the agent) and click both tabs again → same result.

### Logs / console output
_No response_

### Transaction / agent ID
Agent #395 on Celo Sepolia (Identity Registry 0x8004A818BFB912233c491871b3d84c89A494BD9e)

### Anything else
This hurts more than a normal UI bug: the profile URL is exactly what builders share as proof of being on the marketplace, and what a potential buyer would open to see what the agent offers and costs. With both tabs dead, the listing shows an agent with no visible services or capabilities.
