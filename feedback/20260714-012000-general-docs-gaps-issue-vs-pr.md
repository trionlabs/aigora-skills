# [Feedback:general] Docs gaps that cost real time: issue vs PR confusion, missing faucet link, contract addresses not inline

## Header

- **Contact:** @Spagerobaseeth (Telegram)
- **CELO payout wallet:** 0xF70A02D74970FAFF6b0bE6D0dD558965E1B4d855
- **Aigora profile URL:** not yet registered
- **Surface:** Documentation
- **Network:** Testnet (Celo Sepolia)

## Your feedback

Three places where the docs sent me in circles this week:

1. **Issue or PR?** The hackathon material says the feedback skill "creates a GitHub issue in trionlabs/aigora-skills" and the submission form asks for a "Feedback Issue URL". The aigora-feedback skill, however, creates a pull request adding a file under feedback/. The repo confirms it: the Issues tab is empty and all existing submissions are PRs. Every submitter has to notice this contradiction and guess which side is authoritative.

2. **No faucet link.** The register skill says to point the user at a Celo Sepolia faucet if their balance is zero, but never names one. A first-time Celo builder now has to go search, mid-flow, with a wallet connect screen open.

3. **Contract addresses are one hop away.** SKILL.md defers the registry addresses to references/registration-fields.md instead of stating them inline. Agents consuming the skill (which is the whole point of skills) do one more fetch and one more chance to fail.

## Why it matters

Skills are executed literally by coding agents. Ambiguity that a human shrugs off (issue vs PR) becomes a hard fork in an agent's plan, and during a timed hackathon each of these costs minutes to hours across every participant. The feedback bounty itself is judged on submissions arriving in the right format, so the format contradiction directly threatens the thing it gates.

## Suggestion

- Pick one canonical mechanism, state it identically in the hackathon page, the submission form label, and the skill. If PRs stay canonical, rename the form field to "Feedback PR URL".
- Add one faucet URL (e.g. the official Celo faucet) directly in the register skill text.
- Inline the Celo Sepolia and Celo mainnet registry addresses in SKILL.md, and keep the references file as the long-form appendix.

## Anything else

Filed as a PR (not an issue) because the repo's own history shows PRs are what actually exist. That choice being necessary is the feedback.
