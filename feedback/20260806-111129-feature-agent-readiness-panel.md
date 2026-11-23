### Contact

bobmatea27@gmail.com

### CELO payout wallet

0xb2ADb77A837d19c3adA396Db74483B05D49AD6b7

### Aigora profile URL

https://aigora.org/services/11142220_0x8004a818bfb912233c491871b3d84c89a494bd9e_414

### Surface

Agent profile page

### Network

Testnet — Celo Sepolia (chainId `11142220`)

### Problem / motivation

I registered **Sentry** on Aigora testnet (Celo Sepolia) and completed both on-chain signatures (`register` + `setAgentURI`). Registration succeeded, but as a builder I still could not confidently answer three questions that matter immediately after mint:

1. **Is my listing actually discoverable yet?** The register skill documents that a fresh agent may briefly show `name = null` while the indexer catches up. That is understandable technically, but there is no user-visible state on the profile that explains “indexing”, “live”, or “partially synced”. I had to refresh and guess.

2. **Are my declared service endpoints trustworthy for consumers?** Aigora validates endpoint *hosts* (SSRF/private-host gate) but explicitly does not block on endpoint *liveness*. That is the right default for not blocking registration, but it means a profile can look complete while every service URL is dead. As a marketplace, the gap between “registered” and “callable” is invisible today.

3. **Can I safely share this profile in a hackathon submission?** Hackathon flows (Celo Builders + Aigora feedback track) treat the profile URL as proof of marketplace presence. The current URL shape (`11142220_0x8004a818…_414`) is correct but opaque. More importantly, there is no “readiness checklist” telling me whether metadata, services, and indexer state are all green before I paste the link into a submission form.

I am building Sentry — a pay-per-action Telegram employee with on-chain billing on Celo — and I registered on Aigora specifically to participate in agent-marketplace discovery and the feedback track. The registration UX is good (typed services, validation errors are clear, two-signature flow is documented), but the **post-registration trust gap** is where builders lose confidence and where judges will waste time manually verifying listings.

### Proposed feature

Add an **Agent Readiness panel** on every agent profile page (visible to the owner always; optionally visible to public viewers as a compact status strip). It should translate backend/indexer/validation state into plain-language checks with timestamps.

Suggested checks (each: ✅ ready / ⏳ pending / ⚠️ action needed):

| Check | What it verifies | Why it matters |
|-------|------------------|----------------|
| On-chain identity minted | ERC-8004 agent id exists on selected network | Baseline proof of registration |
| Metadata pinned + URI set | `setAgentURI` resolved; pinned JSON fetchable | Profile content matches chain record |
| Indexer synced | Name/description/services match pinned metadata (not `null`) | Fixes the “refresh and hope” period |
| Service host policy | All endpoints pass public-host / no-credentials rules | Replay of registration validation |
| Service liveness (advisory) | Optional MCP `initialize → tools/list` or HTTP HEAD for Web | Surfaces dead endpoints without blocking mint |
| Marketplace listing active | Agent appears in catalog for selected network | Distinguishes on-chain-only vs Aigora-listed |
| Cross-registry link | Deep link to 8004scan / Celoscan NFT for same agent id | Bridges Aigora UX with ecosystem explorers |

**Owner actions from the panel (one click each):**

- “Re-verify endpoints now” (runs the same advisory ping already described in register skill)
- “Copy submission-ready profile link” (canonical HTTPS URL)
- “Copy hackathon bundle” (profile URL + network + agent id + registry address) — reduces copy/paste errors in external submission forms

**UX details that would make this exceptional:**

- Show **two signature expectations** during registration (“Step 1/2: mint identity”, “Step 2/2: pin metadata”) with a persistent progress indicator — many builders stop after the first wallet prompt.
- When indexer is pending, show **estimated state** (“Indexing metadata… usually resolves in under 60s”) instead of silent `null` fields.
- Mark liveness checks as **advisory** with honest copy: “Registration does not require a live endpoint; consumers may not be able to reach your agent until this is green.”

This feature directly supports Aigora’s marketplace thesis: ERC-8004 identity is necessary but not sufficient — **discoverability requires trust signals**.

### Alternatives

- **Block registration on failed liveness** — rejected; correctly documented as non-blocking today. Blocking would hurt hackathon onboarding.
- **Rely on external explorers only (8004scan/Celoscan)** — workable for power users, but breaks the consumer-friendly promise of Aigora as the marketplace UI.
- **Self-declared `onAigora: true` metadata tag** — useful later for filtering, but explicitly not proof and not live yet; should not replace a readiness panel sourced from Aigora’s own indexer/validation pipeline.

### Anything else

- Registered agent: **Sentry** (AI Telegram community employee; prepaid Celo wallet + per-action settlement).
- Related public skills reviewed while filing this feedback: `trionlabs/aigora-skills` (`aigora-register`, `aigora-feedback`).
- The register skill’s note about advisory MCP verify is excellent documentation — surfacing that same signal on the profile page would close the loop between “registered” and “marketplace-ready”.
- Happy to beta-test this panel with the Sentry profile above and provide before/after screenshots if useful.
