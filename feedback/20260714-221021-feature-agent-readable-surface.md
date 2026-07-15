### Contact
@Liffero

### CELO payout wallet
0x91facaa348a8a6847443d9e9338f3b1044977052

### Aigora profile URL
_No response_

### Surface
Documentation

### Network
Testnet — Celo Sepolia (chainId 11142220)

### Problem / motivation
Aigora's own pitch is being an agent marketplace — but a plain HTTP GET
to aigora.org (and its /docs route) returns only the SPA shell with no
rendered content until JavaScript executes. This means an autonomous
agent, a crawler, or any LLM tool with a fetch capability cannot discover
what Aigora is, how to register, or how the task lifecycle works without
a human first reading the rendered page and relaying it. For a platform
whose customers are agents, this is a structural discoverability gap
that sits underneath every other onboarding issue reported so far.

### Proposed feature
1. Add SSR or prerendering for the landing and docs routes, so a plain
   GET returns real content.
2. Publish a `/skill.md` and/or `llms.txt` at the root, describing the
   API base URL, the auth/registration flow, and the task lifecycle in
   one fetchable document.

This isn't hypothetical — two different kinds of platforms already ship
exactly this:
- Askbots lets an agent self-onboard from a single `/skill.md` fetch.
- Mercado Pago's own developer docs serve a clean, JS-free Markdown
  version of every page by appending `.md` to the URL (e.g.
  `checkout-bricks/overview.md` → `text/plain`, fully readable, no
  render step) alongside "Copy page for LLMs" / "Show as Markdown"
  buttons in the UI. A major payments company already treats
  agent/LLM-readable docs as a first-class surface, not an afterthought.

### Alternatives
None that scale: today the only way for an agent to "learn" Aigora is
for a human operator to read the rendered site and hand-write a
description or skill file on the agent's behalf, which doesn't hold up
as the marketplace grows.

### Anything else
Verified 2026-07-14 (live, at submission time): https://aigora.org and
https://aigora.org/docs both return only the SPA shell — 3663 bytes,
`text/html`, `<title>Aigora — Agentic Conversation & Agent Marketplace</title>`,
empty `<body>` beyond the SvelteKit app mount point, no content without
JS execution. Also checked https://aigora.org/skill.md and
https://aigora.org/llms.txt: both return HTTP 200 but are byte-for-byte
identical to the homepage shell (SPA catch-all fallback, `text/html`),
confirming neither actually exists yet — as a control, Mercado Pago's
equivalent `.md` route returns `Content-Type: text/plain` with genuinely
distinct content, which is what a real implementation looks like.
Submitted as part of the Celo Agentic Payments & DeFAI Hackathon
(Track 4 — Best Feedback for Aigora).
