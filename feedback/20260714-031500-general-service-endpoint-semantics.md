# [Feedback:general] Service endpoints are underspecified: buyers cannot tell how to actually call (or pay) an agent

## Header

- **Contact:** @Spagerobaseeth (Telegram)
- **CELO payout wallet:** 0xF70A02D74970FAFF6b0bE6D0dD558965E1B4d855
- **Aigora profile URL:** https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_398
- **Surface:** Registration
- **Network:** Testnet (Celo Sepolia)

## Your feedback

I registered my agent Bureau today (agent #398). The flow itself was smooth: two signatures, IPFS pin, done. But the Services section only takes a URL, a version, and a price. The row's placeholder text implies a fixed contract: "HTTPS · POST /invoke (SSE) · GET /health".

My agent does not work that way, and I could not tell Aigora that. Its paid endpoint is a plain `GET /v1/fx/rates` priced per-call through x402: the server answers HTTP 402 with payment requirements, the buyer signs a USDC authorization and retries. There was no field for the HTTP method, no field for the payment protocol, and no way to link a schema. I entered the URL and the price and hoped.

So the metadata now tells a buyer "this service costs 0.01 USDC" without telling them how to pay it or even which verb to use. A buyer who follows the placeholder's implied contract would POST /invoke at my GET endpoint and get a 404, then probably assume the agent is broken.

## Why it matters

The catalog's whole promise is that a buyer (human or agent) can discover a service and use it without talking to the builder first. Underspecified endpoints break exactly that promise, and they break it silently: everything looks registered and priced, but the first real call fails. As more x402-priced agents register (this hackathon is producing many), the gap between "price shown" and "no payment protocol declared" will generate support noise for you and dead first impressions for builders.

## Suggestion

Per service row, add:
- HTTP method + path (or a small set: GET / POST / SSE)
- Payment protocol dropdown (none, x402, other) so the price field has an actionable meaning
- Optional link to an OpenAPI/schema URL

Alternatively, lean on x402 itself: if a service is marked x402, Aigora could probe the URL once, read the 402 payment-requirements challenge, and display the verified price, token, and network straight from the source. That would make listed prices trustworthy instead of self-reported.

## Anything else

Live example of the mismatch, callable today: `GET https://bureau-fnw3.onrender.com/v1/fx/rates` returns a 402 x402 challenge; the catalog entry for agent #398 cannot express any of that.

Tiny adjacent note: the docs describe profile URLs as `aigora.org/services/<id>`, but the real URL is the composite `services/11142220_0x8004a818..._398`. Guessing `services/398` renders the app shell instead of a not-found page, which had me briefly convinced my registration had failed.
