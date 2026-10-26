### Contact
iwbinb@gmail.com

### CELO payout wallet
0xa64c821ca9633802135aa81098aa1d46cda83595

### Aigora profile URL
_No response_

### Surface
Documentation

### Network
Testnet — Celo Sepolia (chainId `11142220`)

### What happened?
Aigora's official feedback template describes the profile field as an `…/services/<id>` URL. The live agent profile also displays a numeric Agent ID.

I opened the listed testnet profile for HalalFlow Zakat Agent, which displays Agent ID `407`. Following the documented URL format, `https://aigora.org/services/407` returns `Agent not found`.

The same agent is available through a longer composite URL containing the chain ID, registry address, and agent ID:

`https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_407`

This creates an easy failure path for hackathon participants: a builder can follow the official `<id>` guidance, submit a numeric profile URL, and give judges a dead link even though the agent is correctly registered and visible in the catalog.

### Steps to reproduce
1. Open `https://aigora.org/` and select the Celo Sepolia testnet catalog.
2. Open the HalalFlow Zakat Agent profile.
3. Confirm that its profile displays Agent ID `407`.
4. Visit `https://aigora.org/services/407`, following the documented `…/services/<id>` format.
5. Observe `Agent not found`.
6. Visit `https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_407`.
7. Observe that the same Agent ID `407` loads successfully.

Expected behavior: either the numeric route should resolve or redirect to the canonical profile, or the documentation and feedback templates should clearly specify the composite profile identifier and show builders how to copy the canonical URL.

### Logs / console output
_No response_

### Transaction / agent ID
Testnet Agent ID: `407`

Working canonical profile:
`https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_407`

Broken numeric profile:
`https://aigora.org/services/407`

### Anything else
This affects both profile sharing and the hackathon submission path, where builders are asked to provide an Aigora Profile URL.

A backward-compatible redirect from numeric agent URLs would protect already-submitted links. The registration confirmation screen, profile page, and feedback template should also expose the exact canonical URL with a Copy button.
