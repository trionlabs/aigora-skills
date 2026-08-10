### Contact
@iamkaelah (Telegram)

### CELO payout wallet
0x76D7a718CcDc1c132c52D4C05eA0c2FA8e657486

### Aigora profile URL
_No response_

### Surface
Registration

### Network
Testnet — Celo Sepolia (chainId `11142220`)

### Problem / motivation
During Reclaim's Aigora registration, Aigora required at least one service endpoint. The form showed the validation message:

> Add an endpoint URL for this service.

There was no field to declare that the endpoint was authenticated or restricted, and no way to explain its authorization model. We therefore had to list Reclaim's real production Resolution Agent control endpoint:

    POST https://reclaim-kaelah-s-projects.vercel.app/api/resolution-agents/agt_f1f9a3f6-b2ab-4719-995f-90a6d7867235/run

This endpoint is not a public anonymous API. Its verified behavior is:

- an unauthenticated POST returns 401 (we confirmed this against production: the request is rejected before any execution — no state change occurs)
- it requires wallet-signature authentication (the caller signs a message bound to the agent and the run action; invalid or missing signatures are rejected with 401)
- it is party-restricted: only the case funder, the on-chain client, or the on-chain worker is authorized to trigger a run
- it is state-restricted: the escrow case must exist and must not be in a terminal state (released/cancelled/refunded)
- each request executes at most one worker iteration — it is a manual control-plane trigger, not a pay-per-request service

The registration form could not express any of this. A consumer agent that reads only the bare service URL cannot distinguish between:

- a callable public API
- an authenticated API
- an owner/control-plane action
- a party-restricted workflow

That ambiguity is a real marketplace problem, not a hypothetical one. A consumer that tries to call our control endpoint directly gets a 401 and no explanation of what the endpoint is; and the builder side is worse — because an endpoint is mandatory, builders with private control-plane routes are pushed toward either exposing a privileged endpoint as if it were public, or inventing a dummy public endpoint just to pass validation. Both outcomes degrade the metadata trust the marketplace is supposed to provide. Builders should never be incentivized to expose privileged control endpoints merely to qualify for discovery.

### Proposed feature
Add explicit service access semantics per registered service, kept concise and implementable:

- `access`: `public` | `authenticated` | `restricted`
- `auth`: `none` | `wallet-signature` | `api-key` | `oauth` | `custom`
- `authorizationDescription`: who is allowed to call (e.g. "case parties only"), human-readable and optionally machine-readable
- `preconditions`: state or context requirements (e.g. "escrow case must be active")
- `docsUrl`: a link describing the auth/challenge flow

And allow an "identity/discovery-only" listing when an agent has no appropriate publicly invokable endpoint, instead of forcing one. The point is not to make private endpoints callable — it is to let builders declare them accurately so consumers never mistake a control-plane route for a public service.

### Alternatives
- Describing access requirements in the service description text — not machine-readable, so a consumer agent still cannot make a call decision programmatically.
- Listing only endpoints that are genuinely public — excludes legitimate restricted services (e.g. party-gated agent endpoints) from the marketplace entirely.
- HTTP method metadata alone — already requested separately, and a method still does not express authentication, authorization, or state requirements.

### Anything else
The 401 behavior above is directly reproducible against the production endpoint with an unauthenticated POST; no signatures, headers, keys, or secrets are disclosed here. The gap is discovery metadata, not an open endpoint — the endpoint already rejects anonymous callers safely.
