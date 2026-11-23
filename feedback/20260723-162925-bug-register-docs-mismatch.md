### Contact
caxtonacollins@gmail.com · Telegram @caxtonacollins

### CELO payout wallet
0x3E192d109d1dd323375Ac1Ed040f817918E82d63

### Aigora profile URL
https://aigora.org/services/9719

### Surface
Documentation

### Network
Mainnet — Celo (chainId 42220)

### What happened?
I used the `aigora-register` skill from this repository to prepare my registration before opening the form, which is exactly what it's for — "help the user assemble each field **before** they open the form, so nothing fails validation." I prepared the wrong things, because the documented fields don't match the live app.

**1. Skills are documented as free text; the app has a fixed dropdown.**

`skills/aigora-register/SKILL.md` (Step 5, validation rules) states:

> **Skills** (optional): up to 16, each with a **unique name** of ≤32 characters (`skill_name_invalid` / `skill_name_dup` / `too_many_skills`); a skill's description is free markdown, capped at 1000 characters (`skill_desc_too_long`).

I wrote eight skill names to that spec — "Mento swap quotes", "Live market snapshot", "AI swap recommendation", and so on. None were usable. The live form has a **fixed dropdown of standardized OASF skills, capped at 12** (`Skills 0/12`), with no free-text entry and no description field anywhere.

The documented validation errors follow from this: `skill_name_invalid`, `skill_name_dup` and `skill_desc_too_long` cannot occur in the current UI, because the user never types a skill name or description.

**2. The Domains field isn't documented at all.**

The form has a second required-ish capability dropdown, `Domains 0/8` (Software Engineering, Data Science, Blockchain, Banking, Investment Services, Retail (Finance), Risk Management, Telemedicine, Contract Law, …). `SKILL.md` never mentions it. Neither does `references/registration-fields.md`.

**3. Per-service prices aren't documented.**

Each service row has a price field in USDC. The docs describe a service as "a typed endpoint (Web / MCP / A2A) with a public `https://` URL" — no mention that you also set a price, which is arguably the most consequential field on the form for a marketplace.

**4. The docs and the app disagree about which network hackathon entrants should use.**

`SKILL.md` says, twice and unambiguously:

> **Testnet (hackathon):** Celo Sepolia, chainId `11142220` — use this for the hackathon.
> ... for the hackathon, select **testnet (Celo Sepolia, `11142220`)**.

But the Register form has a **"Register for the Hackathon"** toggle that works on mainnet, and the Agentic Payments & DeFAI hackathon's own rules require Celo **mainnet** ("All activity must be on Celo mainnet"). I followed the skill, started on testnet, and had to switch. I registered on mainnet in the end (agent 9719) with the hackathon toggle on.

This one has real consequences — a builder who follows the documentation registers on the wrong network for the hackathon they're entering, and may not find out until judging.

### Steps to reproduce
1. Read `skills/aigora-register/SKILL.md` in this repo, Step 2 and Step 5, and prepare skills as free-text names ≤32 chars.
2. Open https://aigora.org, connect a wallet, Owner mode, Register tab.
3. Open the **Skills** dropdown — observe a fixed list of standardized OASF options, 12 slots, no free-text input, no description field.
4. Observe the **Domains** dropdown (8 slots) and the per-service USDC price fields, neither of which appears in the documentation.
5. Compare `SKILL.md`'s "use testnet for the hackathon" against the form's "Register for the Hackathon" toggle on mainnet, and against the hackathon's mainnet-only rule.

### Logs / console output
_No response_

### Transaction / agent ID
Agent 9719 — https://aigora.org/services/9719 (registered on mainnet, hackathon toggle on)

### Anything else
Suggested fixes:

1. Rewrite the Skills section of `SKILL.md` to describe the standardized OASF dropdown (12 max), and document the Domains dropdown (8 max) and the per-service price field.
2. Drop or correct the `skill_name_invalid` / `skill_name_dup` / `skill_desc_too_long` error codes in `references/registration-fields.md` if they're no longer reachable.
3. Reconcile the network guidance with the hackathon rules and the in-app hackathon toggle. If mainnet is correct for the Agentic Payments & DeFAI hackathon, the "use testnet for the hackathon" lines should go — they currently send entrants to the wrong chain.
4. Document the "Register for the Hackathon" toggle. It's how judges filter the catalog, it's off by default, and it's easy to submit without ever noticing it.

I'd be glad to open a PR against `SKILL.md` with these corrections if that's useful — I've just been through the whole flow with notes.
