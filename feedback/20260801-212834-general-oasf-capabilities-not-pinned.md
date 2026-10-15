### Contact

@myynamehhh (Telegram)

### CELO payout wallet

0x006cBA3012139C299Aa4A522697B4A0c49F38895

### Aigora profile URL

https://aigora.org/services/42220_0x8004a169fb4a3325136eb29fa0ceb6d2e539a432_9752

### Surface

Agent discovery / catalog

### Network

Mainnet — Celo (chainId `42220`)

### Your feedback

I came to Aigora as the builder of a Celo mainnet agent (CoinOp) looking to get
listed, registered through the app (now agent id **9752**, profile linked above), and
went through the marketplace as a buyer would both before and after. Everything below
is reproducible on `https://aigora.org` as of 2026-08-01, and the first item is the
one I'd fix before any of the others.

**1. The OASF capabilities I selected are not in the metadata Aigora pins — they
exist only inside Aigora.** The register form labels that section "Capabilities —
standardized OASF skills & domains **(indexed by 8004scan)**", so I picked 7 skills
and 2 domains believing they'd travel with the agent. My profile page renders all
nine correctly. But the `agent.json` that `tokenURI` actually points at
(`ipfs://QmRPDUhfVLpQDzDbPoMFrBcLVhwgSpsVju6DTB4itrssPT`) has **no `skills` key and
no `domains` key at all** — its keys are `type, name, description, services,
x402Support, active, registrations, supportedTrust, categories, external_links,
onAigora`. This is not specific to my registration: the top-ranked testnet agent
(HalalFlow, id 407, `ipfs://QmeC2UuDg5QkBTDLHgrJXV4DWoKAP8hGez9AzSNLmkVsxa`) is
missing them too, while its profile page also displays them. So the capability
taxonomy — the single most machine-readable thing on the form, and the thing an
agent-to-agent marketplace most needs to be portable — is Aigora-local state that no
8004scan, no other reader, and no agent doing discovery on-chain can ever see.

That matters more than a missing field, because "we write you into the **canonical**
ERC-8004 contracts, your own wallet owns it, any 8004 tool can read it" is the exact
argument that convinced me to register. It's true of the identity and false of the
capabilities, and the form's own "(indexed by 8004scan)" label says the opposite.

Related, smaller: the published `aigora-register` skill describes Skills as "up to
16, each with a unique name ≤32 chars and an optional markdown description ≤1000".
The real form is a fixed OASF dropdown, max 12, with no per-skill description, plus
a separate Domains dropdown (max 8) the skill never mentions. I had prepared six
hand-written skill names from the docs and had to throw all of them away at the form.
If the skill file is the intended way for agents to prepare a registration, that
drift costs every builder the same wasted step.

**2. Nothing in the catalog is a link, so nothing about an agent is discoverable off
Aigora.** Every "Profile" control is a `<button>` with a client-side route change —
the whole home page contains exactly one `<a href>` (to `early.aigora.org`). A
profile URL like `/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_407`
does load when typed directly, but `curl` on it returns zero rendered content, the
generic site `<title>`, and no `og:` or `twitter:` meta at all. The catalog is read
client-side from Firestore, so a crawler, a Telegram unfurl or an X card sees an
empty shell. For a marketplace whose product *is* discovery, this means an agent
listed on Aigora is invisible to every discovery channel that isn't Aigora itself —
and a builder can't share a listing that looks like anything.

**3. The network toggle defaults to Testnet, which is the smaller catalog.** A
first-time visitor lands on 13 testnet agents; switching to Mainnet loads "24+". So
the default view of "the agent marketplace, native to Celo" is the test one, and the
real one is behind a control most visitors won't notice. The two views also label
their counts differently ("13 shown" vs "24+ loaded"), which makes it harder to tell
you've switched.

**4. `Best` is the default sort, and the reputation it sorts on looks trivially
self-serviceable.** The #1 agent in the default testnet view shows `100/100` with
"280 feedback · weighted avg" — while every entry in its own Recent Feedback panel
is from one address (`0xf89b…a208`), all dated `2026-07-19`, the same day the agent
was registered. I'm not accusing anyone of gaming it, and testnet feedback is free
by definition — but as a buyer I can't distinguish "280 reviews" from "one wallet,
280 times", and the ranking doesn't either. That number is the single strongest
trust signal on the page.

**5. Three of the 13 testnet listings are titled "Aigora Agent".** These are entries
whose metadata never resolved — the `aigora-register` skill says a fresh agent "may
briefly show name = null … it resolves on its own", but here the fallback renders as
a plausible-looking agent named after the platform itself. So the catalog shows
what appear to be first-party Aigora agents that are actually unresolved third-party
registrations. A neutral "Unresolved metadata" state would be both more honest and
more useful to the builder whose listing is broken.

**6. Two config-level things I noticed while looking.** The published bundle logs its
own fault to every visitor's console: `[appcheck] PUBLIC_RECAPTCHA_SITE_KEY is empty
in this build, so no App Check token is attached to any request. With server-side App
Check enforcement on, every authenticated Cloud Function call will fail with 401.
This is a build/config fault in the published bundle, NOT a backend outage.` And the
production site reads from the Firestore project `aigora-dev`. My registration went
through fine, so enforcement is evidently off on the server today — which is the
point: the attestation you built isn't protecting anything, and the day someone turns
enforcement on, every authenticated call from this bundle starts failing. Worth
resolving in the calm rather than in the incident. (Happy to move this one to a
private channel if you'd rather it not sit in a public PR.)

Smaller thing: on a profile page the description renders `&amp;` literally
("Agentic Payments &amp; DeFAI") — either the metadata carries the escaped entity or
the renderer double-escapes it; normalising on display would cover both.

And one genuine positive: registering into the **canonical** ERC-8004 contracts
rather than a proprietary registry, and saying so plainly in the skill (with the
addresses, and the note that Celo has Identity + Reputation but no Validation
registry), is the reason I was willing to list at all. I already own agent 9751 on
mainnet; knowing Aigora writes into the same registry my own tooling reads means
listing costs me nothing in lock-in. That's an unusually honest design choice and it
should be louder in the marketing, not buried in a skill file.

### Why it matters

(1) and (2) are the same wound in two places: an agent registered on Aigora is
legible only inside Aigora. (1) means no machine outside can read what the agent
*does* — you're asking builders to fill in a standardised taxonomy and then not
publishing it where the standard says to look. (2) means no human outside can find
the listing either: no crawler, no link preview, nothing to index. Between them, the
profile URL I just earned with two signatures is a submission artifact rather than a
distribution channel — and for a marketplace, distribution is the whole product.

(1) is also the cheapest of all of these to fix. The metadata is already being pinned
at registration; adding `skills` and `domains` to that JSON is a change at one
write-site, and every past agent can be backfilled on their next edit.

(3) and (5) shape what a first-time visitor believes the marketplace *is*: today
that's a mostly-testnet catalog containing several entries named after the platform.
(4) decides who they trust once they're there — an unweighted feedback count as the
default ranking key is the thing sybils optimise first, and it's cheap to fix before
real money moves (de-duplicate by client address, or weight by distinct payers /
settled x402 payments rather than raw feedback rows).

(6) is potentially the difference between "sign-in is flaky" and "sign-in is
broken", and the fault is announced by your own bundle, so any user with a console
open sees it before you do.

### Suggestion

- Pin `skills` and `domains` into the `agent.json` at registration, so the OASF
  taxonomy is readable by 8004scan and any other ERC-8004 consumer — or, if that is
  deliberate for now, drop "(indexed by 8004scan)" from the form label and say
  plainly that capabilities are Aigora-side until then.
- Regenerate the `aigora-register` skill from the real form (12 OASF skills from a
  fixed list, no per-skill description, plus Domains), and publish the field list and
  validation rules at a public URL so builders can prepare before connecting a wallet.
- Server-render `/services/<id>` with a per-agent `<title>`, description and `og:`
  image; make catalog cards `<a href>` so links can be middle-clicked, copied,
  crawled and unfurled.
- Default the network toggle to Mainnet, and use one consistent count label.
- Rank on distinct feedback authors (or settled payments), not raw feedback count;
  show "N reviews from M wallets" on the card so the ratio is visible.
- Render unresolved metadata as an explicit "Unresolved metadata" state, never as an
  agent named "Aigora Agent".

### Anything else

All observations from `https://aigora.org` on 2026-08-01, Chromium. Evidence:

- (1) `curl -s https://ipfs.io/ipfs/QmRPDUhfVLpQDzDbPoMFrBcLVhwgSpsVju6DTB4itrssPT | jq keys`
  — my own agent, registered through the form with 7 skills and 2 domains selected;
  compare `QmeC2UuDg5QkBTDLHgrJXV4DWoKAP8hGez9AzSNLmkVsxa` (HalalFlow, id 407) for
  the same absence. Both profile pages render the capabilities regardless.
- (2) `curl -s https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_407 | grep -i '<title>\|og:'`
- (3)(4)(5) the default home view.
- (6) the browser console on first load, and the Firestore project id in the network
  panel.

I build CoinOp (<https://github.com/hms1499/shippost>), a coin-operated AI thread
writer on Celo mainnet that pays for its own AI calls over x402 — so the
ERC-8004-plus-x402 stack Aigora is betting on is the one I already run in
production.
