# Payment availability documentation — feedback #35

Scope: clarify the README's product-feature wording and the services-only build boundary identified in community report §2.2 / [feedback PR #35](https://github.com/trionlabs/aigora-skills/pull/35).

The README now separates catalog/registration availability from in-app DM and bounty/escrow payment surfaces. It also distinguishes a USDC display price or x402 declaration from enabling an action or provisioning an operator payment server. Existing field-reference pricing and preflight rules remain authoritative.

This is a documentation repair. Profile copy, payment behavior, endpoint schema, live deployment state and the rest of #35's claims are outside this commit. No Bazaar change is included.

Validation before the implementation commit: compared product source revision `361f8e107ec412ff0ed0b45ff2258510ee173243` for the services-only route guard, DM feature gate and display-price/payment-challenge contract; checked the local Markdown target/heading and `git diff --check`. No product files changed, and no live-runtime acceptance is claimed.

Implementation commit: `d835be1a04a330e72112fc138802207733121cb1`, reviewed against `11df25426dc74aaebdff057cbb000ba468b75628`.

Independent Astra high: no findings. Genuine Opus 5.5 high: no actionable findings; actual response metadata confirmed `claude-opus-5-5` only. The implementer verified reviewed file hashes before applying Opus's optional wording, link, terminology and source-revision suggestions. Those final documentation edits were checked by the implementer; a final independent re-review is not claimed.

Final validation: both local anchors exist, `git diff --check` passes, and product source remains unchanged. GitHub PR #35's title and submitted diff were read directly after the model reviews and support this narrow README scope. The reviewers used the supplied report; neither claimed raw-PR, live-origin or payment acceptance. UI price copy, verification of live payment availability and the remaining product requests stay separate.
