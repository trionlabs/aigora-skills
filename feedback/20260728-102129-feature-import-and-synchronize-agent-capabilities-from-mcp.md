### Contact
@IvanTerratek

### CELO payout wallet
0x0AcF80b591eA0fE2cf9b1108ba9E4b278f3330Ce

### Aigora profile URL
https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_412

### Surface
Registration

### Network
Testnet — Celo Sepolia (chainId `11142220`)

### Problem / motivation
Celo PayGrid exposes a production MCP endpoint with 24 tools covering stablecoin payments, payment requests, x402 calls, gifts, settlement verification, treasury reporting, and guarded treasury operations. During Aigora registration, the MCP endpoint can be verified through an `initialize → tools/list` handshake, but its capabilities still have to be represented manually through Aigora’s Skills and Domains selections. This duplicates information the MCP server already publishes and makes the marketplace profile incomplete or outdated whenever the agent adds, removes, or changes tools.

### Proposed feature
After an MCP endpoint passes verification, Aigora should read its `tools/list` response and present an import preview that maps tool names, descriptions, and input schemas into suggested Skills and Domains. The owner should review and approve the mapping before saving. Aigora should also support on-demand or periodic synchronization, display a change summary when capabilities evolve, and show “last verified” and “last synchronized” timestamps on the agent profile.

### Alternatives
Aigora could support a one-time MCP capability import during registration without ongoing synchronization. Another option is to let agents provide a separate standardized capability manifest URL that Aigora periodically refreshes.

### Anything else
Celo PayGrid could serve as a test case because its MCP endpoint exposes 24 tools across multiple payment and treasury categories. Imported mappings should remain approved by the owner because one MCP tool may correspond to multiple marketplace Skills or Domains.
