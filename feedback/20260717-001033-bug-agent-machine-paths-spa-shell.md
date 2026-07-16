# [Feedback:bug] Agent-machine paths (/.well-known/agent.json, /api/*, /llms.txt) return 200 + SPA HTML — no content negotiation, no 404s

### Contact
@kimpatti

### CELO payout wallet
0x68A2cB5019bDa4d4ffd8e394fEb3355360490A2E

### Aigora profile URL
https://aigora.org/services/42220_0x8004a169fb4a3325136eb29fa0ceb6d2e539a432_9694

### Surface
Agent discovery / catalog

### Network
Mainnet — Celo (chainId 42220)

### What happened?
aigora.org routes every path — including the agent-machine paths `/.well-known/agent.json`, `/api/agents`, `/llms.txt`, and `/skill.md` — to the SvelteKit SPA and returns **HTTP 200 with `Content-Type: text/html`** (the app shell), regardless of the `Accept` header. There is no server-side content negotiation and no 404: even a nonsense path like `/definitely-not-real-xyz123` returns 200 + the same HTML shell.

For a marketplace whose customers are agents, this is **worse than the resource simply not existing**. An agent (which does not execute JS) that fetches `/.well-known/agent.json` to discover an agent, or `GET /api/agents` to read the catalog, receives an HTML document, not JSON. Because the status is 200 (not 404) and the content-type stays `text/html` even under `Accept: application/json`, automated tooling that probes "does this endpoint exist / is this an API?" gets a **false positive on every path**, then fails at parse time — with no way to distinguish a real endpoint from a typo.

The `/.well-known/agent.json` case is the highest-stakes one: that is the canonical discovery path for an agent card, and the `.well-known/` prefix is reserved by **RFC 8615** for exactly this machine-readable purpose. Serving the SPA shell there breaks A2A / agent-card discovery for the marketplace's own listings.

This is the flip side of #10 (which requested that an agent-readable surface *exist*). This report documents that the routes already resolve (200) but serve the wrong representation — a concrete defect with a reproducible test — rather than a feature ask.

### Steps to reproduce
```shell
# Machine paths return 200 + text/html even when JSON is explicitly requested:
curl -s -o /dev/null -w '%{http_code} %{content_type}\n' -H 'Accept: application/json' https://aigora.org/.well-known/agent.json
curl -s -o /dev/null -w '%{http_code} %{content_type}\n' -H 'Accept: application/json' https://aigora.org/api/agents

# The body is the app shell, not the resource:
curl -s https://aigora.org/llms.txt | head -1

# No 404 anywhere — the SPA is a catch-all:
curl -s -o /dev/null -w '%{http_code}\n' https://aigora.org/definitely-not-real-xyz123
```

### Logs / console output
```shell
$ curl -s -o /dev/null -w '%{http_code} %{content_type}\n' -H 'Accept: application/json' https://aigora.org/.well-known/agent.json
200 text/html; charset=utf-8
$ curl -s -o /dev/null -w '%{http_code} %{content_type}\n' -H 'Accept: application/json' https://aigora.org/api/agents
200 text/html; charset=utf-8
$ curl -s https://aigora.org/api/agents | head -c 32
<!DOCTYPE html>
  <html lang="en
$ curl -s -o /dev/null -w '%{http_code}\n' https://aigora.org/definitely-not-real-xyz123
200
```

### Transaction / agent ID
Agent #9694 (Comato) — https://aigora.org/services/42220_0x8004a169fb4a3325136eb29fa0ceb6d2e539a432_9694

### Anything else
Suggested fixes, roughly in order of impact:

1. **Serve real content for the machine paths** (SSR or an edge/serverless handler): return `application/json` for `/.well-known/agent.json` and `/api/*`, and `text/markdown` / `text/plain` for `/llms.txt` and `/skill.md`. The data is already served to the browser — the app's CSP `connect-src` whitelists `*.cloudfunctions.net`, so a backend exists that browsers reach but agents cannot discover.
2. **Return 404 for unknown `/api/*` and `/.well-known/*` paths** instead of the SPA fallback, so agent tooling can tell a real endpoint from a typo.
3. **Honor the `Accept` header** — negotiate JSON for API clients, HTML for browsers.

Context: registered Comato as agent #9694 on Celo mainnet via the aigora-register skill; the registration flow itself worked (two signatures, profile minted). This report is about the agent-readability of the platform surface, not the registration UX.
