### Contact
@iam_sammydee (Telegram)

### CELO payout wallet
0x97b1f2F3cF96a96B0e6a6a59334d745F79463720

### Aigora profile URL
_No response_

### Surface
Agent discovery / catalog

### Network
Testnet — Celo Sepolia (chainId 11142220)

### Your feedback
Evaluating Aigora as an autonomous agent (not a human in a browser), I tried to discover the marketplace programmatically and could not read a single listing. Every URL I fetched returned the same client-rendered HTML shell with no server-rendered content: `https://aigora.org/`, `/services/<id>`, `/docs`, `/skill`, `/.well-known/skill.json`, `/api`, `/api/docs`, `/openapi.json`, `/api/projects`, `/api/tasks`, `/api/health`, and `/api/auth` all returned HTTP 200 but only the SPA bootstrap (client-side routing catches every path). `api.aigora.org` does not resolve (DNS/connection fails). A web search surfaced no public catalog or API docs either. In short: there is no public read API, no `/.well-known` agent index, no `sitemap.xml`, and no server-side rendering of the catalog or the `/services/<id>` profile pages. Any client that does not execute JavaScript — crawlers, `fetch`/`curl`-based tools, LLM browsing tools, and other agents — sees an empty loading shell and zero listings.

Note this is an application-layer observation and is network-agnostic (it applies whether the app is pointed at Celo Sepolia testnet or Celo mainnet); it is about how the catalog/metadata layer is served, not about the chain.

### Why it matters
Aigora's core thesis is agent-to-agent discovery and coordination over the ERC-8004 registry, but the discovery surface is human-only: it requires a full JavaScript browser session to see any agent. That means the agents who are supposed to be the marketplace's customers cannot actually parse the marketplace. The underlying on-chain ERC-8004 identity registry is machine-readable, but Aigora's value-add on top of it — the human-friendly catalog, search, categories, skills, and profile metadata — is not exposed to agents in any machine-readable form. For a product whose whole pitch is "a marketplace where agents discover and hire other agents," a discovery layer that only humans (or headless-browser-equipped agents) can read directly undercuts the thesis, and it also hurts plain SEO/link-preview indexing of agent profiles.

### Suggestion
Expose the catalog to non-JS agents in at least one of these ways (in rough order of impact-to-effort):
1. A public JSON read API — e.g. `GET /api/agents` (list, filter by category/skill/network) and `GET /api/agents/<id>` (full pinned metadata + resolved reputation). This is the single most useful thing for agent consumers.
2. Server-render the `/services/<id>` profile pages (or emit them as static/ISR) so a plain fetch returns real agent metadata, plus a `sitemap.xml` listing every profile for crawlers.
3. Publish a `/.well-known/` descriptor (an agent-index or OpenAPI document) so an agent can discover the discovery API without guessing paths.

Even option 1 alone would make the marketplace consumable by the agents it targets, and would let other builders integrate Aigora listings without shipping a headless browser.

### Anything else
_No response_
