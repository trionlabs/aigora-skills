### Contact

mericcintosunn@gmail.com

### CELO payout wallet

0xF43F43D8aee114a71B164e1f6214BC7625a5742D

### Aigora profile URL

_No response_

### Surface

Agent discovery / catalog

### Network

Both, and the split matters for reading the rest: every observation below was made on
**Mainnet - Celo (chainId 42220)**, because that is where our agent and the registry we
swept live. The Aigora registration path this report criticises is the hackathon one on
**Testnet - Celo Sepolia (chainId 11142220)**. Anything labelled with an agent id or the
registry address `0x8004A169FB4a3325136EB29fA0ceB6D2e539a432` is mainnet.

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
ids off that contract (token ids 9256-9759, read 2026-08-09 ~14:10Z, Celo mainnet head
block 74387565 at 15:12Z) and paid our own oracle to verify a slice of them one by one,
and from that data there is no way to compute "which of these are on Aigora". The one signal
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

Evidence, so this can be told apart from a transient deploy state. All seven URLs
observed at **2026-08-09T15:12:00Z**, following the same result at ~13:46Z:

| URL | status | content-type |
|---|---|---|
| `https://aigora.org/api/agents` | 200 | `text/html; charset=utf-8` |
| `https://aigora.org/api/services` | 200 | `text/html; charset=utf-8` |
| `https://aigora.org/llms.txt` | 200 | `text/html; charset=utf-8` |
| `https://aigora.org/skill.md` | 200 | `text/html; charset=utf-8` |
| `https://aigora-indexer-vfl5n4ujkq-ey.a.run.app/` | 200 | `application/json` |
| `https://aigora-indexer-vfl5n4ujkq-ey.a.run.app/health` | 200 | `application/json` |
| `https://aigora-indexer-vfl5n4ujkq-ey.a.run.app/agents` | 404 | `application/json` |

The indexer reported `network: celo-sepolia`, `chainId: 11142220`, `indexer_epoch: 11`,
`max_lag_seconds: 164` at that moment.

**Smaller, same theme.** The registry holds at least three tokenURI conventions in one
contract: an `https://` agent card (#9759), an `ipfs://` CID (#9752), and a gzipped
base64 `data:application/json` URI (#1). All three read live from
`0x8004A169FB4a3325136EB29fA0ceB6D2e539a432` on Celo mainnet on 2026-08-09 between
13:45Z and 15:12Z. We had to handle all three tonight. The skill
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

1. **Publish the provenance set, with evidence rather than assertion.** One unauthenticated
   `GET /api/agents` returning, per agent: `agentId`, `chainId`, `registryAddress`,
   `owner`, `tokenURI`, and the registration evidence itself - `registrationTxHash`,
   `blockNumber`, `logIndex`. Say explicitly whether `owner` and `tokenURI` are current
   or as-of-registration, because they are mutable and the two answers differ. A caller
   can then replay the log entry against Celo and confirm the listing without trusting
   the response, which is the whole point: a bare JSON list is our `onAigora` problem
   again, just served from your domain instead of a spoofable key. Signing the payload
   (or publishing a Merkle root over it) is a reasonable second step, but the tx hash and
   log index alone already make it independently checkable. Document it in the repo
   README and serve a real `/llms.txt` and `/skill.md` from the app instead of the SPA
   shell.
2. **Add a prepared-transaction mode to registration.** Let a caller submit the metadata
   and get back the pinned CID plus a complete, broadcastable pair: for each of `register`
   and `setAgentURI`, the `to` (registry address), `chainId`, `data` (encoded calldata),
   and `value`. Say how long the prepared pair is valid and what happens on retry - a
   stable idempotency key over the metadata, so retrying returns the same CID and calldata
   instead of a second pin. And define how you match the externally broadcast transaction
   back to your pin before you mark the agent provenance-eligible: watching the registry
   for a `setAgentURI` whose URI equals the CID you issued is enough, and it keeps the
   provenance claim yours rather than the caller's. Provenance survives, and
   `aigora-register` becomes a skill an agent can actually finish.
3. **Add a claim flow for existing on-chain agents, bound so it cannot be replayed.** Our
   first instinct was "a signature from `ownerOf(agentId)` is enough", and that is too
   loose: a bare signature is replayable across agents, chains and time. Verify a
   domain-separated message carrying `chainId`, the registry address, `agentId`, the
   claiming account, a nonce and an expiry, reject reused nonces and expired messages, and
   accept ERC-1271 when `ownerOf` is a contract - a good share of serious agents are owned
   by a multisig, and EOA-only signature recovery locks exactly those out. That avoids
   pushing established agents into minting a second identity.
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

A note on this entry's own history, since it is public: suggestions 1, 2 and 3 above are
tighter than the version first pushed to this PR. Your repo's automated reviewer flagged
them as under-specified, and on three of the four points it was right - most usefully
that a bare `ownerOf` signature is replayable. Those are folded in above. The one thing
it caught that was a genuine error rather than a missing detail was ours: the header
originally said Celo Sepolia while every observation in the body is Celo mainnet. Fixed.
