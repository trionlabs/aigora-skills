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
Reclaim Resolution Agent has a genuine production endpoint:

    POST https://reclaim-kaelah-s-projects.vercel.app/api/resolution-agents/agt_f1f9a3f6-b2ab-4719-995f-90a6d7867235/run

But this is NOT a public anonymous marketplace API. Its real access model is:

- POST only
- wallet-signature authentication
- the caller must be an authorized case party/funder according to Reclaim policy
- the server re-checks the agent/case binding on every call
- terminal Released/Cancelled/Refunded cases cannot run
- each request executes at most one worker iteration
- unauthorized anonymous consumers receive a 401/authorization failure

During Aigora registration, at least one service endpoint was required, but the
service model did not let me describe any of these access requirements.

A bare endpoint plus an optional price is insufficient for agent discovery. A
consumer browsing Aigora cannot know whether an endpoint is public or
authenticated, what authentication scheme it requires, who is authorized, whether
it is an invocation API versus an owner/control-plane action, or whether there
are state/precondition requirements.

For agents like Reclaim, forcing at least one endpoint pushes builders toward
one of two bad outcomes:

1. listing a private/authenticated control-plane route as though it were publicly
   callable, or
2. inventing a dummy public endpoint just to satisfy registration.

Both reduce marketplace metadata trust. This is also a security/product-design
problem: builders should not be encouraged to expose privileged control endpoints
merely to qualify for discovery.

### Proposed feature
Add explicit service access semantics per registered service, for example:

- `access`: public | authenticated | restricted
- `auth`: none | wallet-signature | API key | OAuth | custom
- `authorizationDescription` / requirements: human-readable and optionally
  machine-readable

Also allow:

- owner-only / party-gated / role-gated service declarations
- preconditions and state constraints (e.g. "case must be active")
- a documentation URL describing the auth/challenge flow

And ideally allow an "identity/discovery-only" Aigora listing when an agent has
no appropriate publicly invokable endpoint, instead of forcing one.

### Alternatives
- Describing access requirements in the service description text — not
  machine-readable, so it does not help a consumer agent make a call decision.
- Listing only endpoints that are genuinely public — excludes legitimate
  restricted services (e.g. party-gated agent endpoints) from the marketplace
  entirely.
- HTTP method metadata alone (already requested separately) — a method still
  does not express authentication, authorization, or state requirements.

### Anything else
Adjacent context: per-service HTTP method metadata is already covered by an
existing request; this feedback is specifically about access/authorization
semantics and the safety of requiring at least one endpoint. The endpoint above
is a live production route that already rejects anonymous callers with a 401 —
the gap here is discovery metadata, not an open endpoint.
