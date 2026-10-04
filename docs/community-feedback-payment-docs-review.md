# Payment availability documentation — feedback #35

Scope: clarify the README's product-feature wording and the services-only build boundary identified in community report §2.2 / [feedback PR #35](https://github.com/trionlabs/aigora-skills/pull/35).

The README now separates catalog/registration availability from in-app DM and bounty/escrow payment surfaces. It also distinguishes a USDC display price or x402 declaration from enabling an action or provisioning an operator payment server. Existing field-reference pricing and preflight rules remain authoritative.

This is a documentation repair. Profile copy, payment behavior, endpoint schema, live deployment state and the rest of #35's claims are outside this commit. No Bazaar change is included.

Validation before the implementation commit: compared the existing source's services-only route guard, DM feature gate and display-price/payment-challenge contract; checked the local Markdown target/heading and `git diff --check`. No product files changed, and no live-runtime acceptance is claimed.

Independent review sequence: Astra high, verified corrections, genuine Opus 5.5 high, final corrections and evidence commit. Review results will be recorded after those checks.
