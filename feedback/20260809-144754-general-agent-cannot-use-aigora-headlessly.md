### Contact

mericcintosunn@gmail.com

### CELO payout wallet

0xF43F43D8aee114a71B164e1f6214BC7625a5742D

### Aigora profile URL

_No response_

### Surface

Agent discovery / catalog

### Network

Testnet - Celo Sepolia (chainId 11142220)

### Your feedback

We build A-Identity, an ERC-8004 trust oracle: before one agent pays another, it asks us
to verify the counterparty. So we came to Aigora twice over: as an agent that wanted to be listed, and as a
consumer of Aigora as a directory. Three things, one theme: Aigora is an agent
marketplace that an agent cannot currently use without a human at a browser.

**1. Registration cannot be completed headlessly, in a skill written for an agent to
run.** `aigora-register` addresses an agent directly, and then Step 6 requires two
interactive wallet signatures in the web app, so the agent it is addressed to has to stop
and hand the task back to a human. There is no documented way to obtain the two calls as
calldata (`register(...)`, then `setAgentURI(agentId, ...)` against the CID Aigora
pinned) and broadcast them from a signer we already control. Our own rule for on-chain
writes is prepared-or-executed: without a signer we return the exact call we would have
made. Aigora has no equivalent, so the flow stalls at exactly the point an agent is most
capable.

**2. The catalog is not a view of the registry, and nothing on-chain distinguishes the
two.** Aigora writes into the canonical Celo IdentityRegistry, which is exactly where our
agent already lives: `celo/9759` in `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432`, owner
`0xF43F43D8aee114a71B164e1f6214BC7625a5742D`, tokenURI
`https://a-identity.xyz/.well-known/agent-card.json`. The skill is explicit and correct
that a direct `register()` will not appear in your catalog, because listing comes from
provenance. But provenance is not readable from chain state. Today we read the 500 most recently minted
ids off that contract and paid our own oracle to verify a slice of them one by one, and
from that data there is no way to compute "which of these are on Aigora". The one signal
you offer, `onAigora: true`, your own skill documents as self-declared, spoofable, and
never to be gated on. That is the honest description of it, and it also means the signal
cannot answer the question. There is also no documented way to claim or import an agent
that already exists on-chain. On mainnet that means an established 8004 agent's only path
into your catalog is to mint a second identity in the same registry it already lives in,
which is precisely the duplicate-identity behaviour a trust layer exists to discourage.

**3. There is no machine-readable directory, although you already run the service that
would provide one.** `aigora.org` is a SvelteKit SPA and its catch-all route answers 200
with the HTML shell for everything: `/api/agents`, `/api/services`, `/llms.txt` and
`/skill.md` all return the app page with a 200. A crawling agent therefore gets a success
status and a document that says nothing, which is worse than a 404 because it reads as
"this endpoint exists and is empty". Meanwhile your indexer at
`https://aigora-indexer-vfl5n4ujkq-ey.a.run.app` serves a genuinely good machine document
at `/` and `/health` (`{ok, service, network, chainId, head_block, indexer_epoch,
max_lag_seconds, status, cursors{...lag_seconds}}`) that is more honest about its own
freshness than most indexers we integrate. It is referenced only in the site CSP, it is
documented nowhere, and `/agents` on it is a 404.

**Smaller, same theme.** The registry holds at least three tokenURI conventions in one
contract: an `https://` agent card (#9759), an `ipfs://` CID (#9752), and a gzipped
base64 `data:application/json` URI (#1). We had to handle all three tonight. The skill
notes that a fresh agent may show `name = null` while the indexer catches up, but the
owner has no way to tell "indexer is lagging" from "my metadata is in a form you do not
resolve" - the two failure modes look identical on the profile page.

### Why it matters

Aigora's stated market is agent-to-agent discovery, and the three items above are exactly
the surfaces an agent counterparty needs. As it stands, the marketplace is discoverable by
humans and opaque to the software it is for.

For us specifically it blocked real integration work, not hypothetical work. We wanted to
consume Aigora as a directory inside our trust oracle: an agent asks us about a
counterparty, and "listed on Aigora, registered through Aigora" would be a genuine
positive trust signal we would happily weigh. We cannot use it. We cannot fetch the
member set, and if we scraped it we could not verify it, because the only on-chain marker
is one your own documentation tells us not to trust. So the signal we would have paid
attention to is one we have to ignore, and Aigora's provenance advantage - the thing that
makes your catalog worth more than a raw registry read - stays locked inside your
database.

It also matters for sybil resistance, which is the same problem from the other side. Point
2 means the correct move for an existing 8004 agent that wants to be listed is to mint a
duplicate identity. A marketplace built on an identity registry should not have "register
twice" as its supported path.

### Suggestion

Concretely, in the order we would value them:

1. **Publish the provenance set.** One authenticated-free endpoint, `GET /api/agents`,
   returning the agents registered through Aigora with `{agentId, chainId, owner,
   tokenURI, registeredAt}`. That single endpoint fixes points 2 and 3 at once: it makes
   the catalog machine-readable and makes "on Aigora" a verifiable claim a third party can
   check, instead of a spoofable JSON key. Document it in the repo README and serve a real
   `/llms.txt` and `/skill.md` from the app instead of the SPA shell.
2. **Add a prepared-transaction mode to registration.** Let a caller submit the metadata,
   get back the pinned CID plus the exact `register` and `setAgentURI` calldata, and
   broadcast from their own signer. Provenance is preserved because the pin came from
   Aigora, so these agents can still be catalog-listed, and `aigora-register` becomes a
   skill an agent can actually finish.
3. **Add a claim flow for existing on-chain agents.** A signature from the current
   `ownerOf(agentId)` is sufficient proof to list an agent that already exists, and it
   avoids pushing established agents into minting a second identity.
4. **Distinguish the two `name = null` cases on the profile.** "Indexed, metadata
   unresolvable at `<uri>`" versus "not indexed yet, lag Ns" - you already have the lag
   number in the indexer health document.

### Anything else

Context for what we were doing when we hit this: we run four pay-per-request x402 trust
tools on Celo mainnet, settled in USDC through the first-party Celo facilitator, and
tonight we ran the live Celo ERC-8004 registry through them one agent at a time. Every
settlement is published with its transaction hash at
https://a-identity.xyz/celo-proof and https://a-identity-backend.onrender.com/api/celo/proof.
That sweep is where points 2 and 3 stopped being theoretical: we had 500 minted agents
in front of us and still could not answer "which of these are on Aigora".

Repo: https://github.com/getA-Identity/A-Identity
Agent: https://8004scan.io/agents/celo/9759
