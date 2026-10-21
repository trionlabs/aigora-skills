# [Feedback:feature] Agent-native registration so agents can register themselves

## Header

- **Contact:** @Spagerobaseeth (Telegram)
- **CELO payout wallet:** 0xF70A02D74970FAFF6b0bE6D0dD558965E1B4d855
- **Aigora profile URL:** not yet registered (this feature request is about why)
- **Surface:** Registration
- **Network:** Testnet (Celo Sepolia)

## Problem / motivation

I run an autonomous agent (Bureau, ERC-8004 agent #9675 on Celo mainnet). It holds its own wallet, signs its own transactions, trades on Mento, and sells data over x402 without a human in the loop. When I asked it to register itself on Aigora, it could not: registration is browser-only through the aigora.org web app with a Thirdweb wallet connection, and the aigora-register skill itself states there is no programmatic registration API.

So the one actor an agent marketplace is built for, the agent, is the one actor that cannot register. I had to stop the agent's workflow and plan a manual browser session, and the only ways to keep the agent's own identity are bad options: import the agent's hot key into a browser extension (poor key hygiene) or register with a different human wallet so the marketplace identity does not match the wallet the agent actually transacts with.

## Proposed feature

A registration path an agent can complete with nothing but its private key:

1. An HTTP endpoint that accepts the agent.json metadata plus an EIP-712 or personal_sign signature from the agent wallet, and performs the mint and metadata pin server-side. The x402.celo.org facilitator already does exactly this pattern for API keys (fetch nonce, sign message, POST) and it works beautifully headless.
2. Alternatively (or additionally): accept an existing ERC-8004 identity. If a wallet already owns an agentId on the identity registry with a reachable agentURI, let it claim a profile by signature instead of re-minting.

Either unlocks the "agents onboarding agents" loop that makes a marketplace like this compound.

## Alternatives considered

- Importing the agent's key into MetaMask for one session: works but normalizes terrible key handling for exactly the audience that should model good practice.
- Registering with a personal wallet: breaks the link between the profile and the wallet that pays and gets paid onchain, which weakens the reputation story ERC-8004 is supposed to provide.

## Anything else

The register skill is well written and honest about the limitation. This is the single change that would have turned my registration from a planned human session into a 20-second agent action.
