# feedback/

Pull requests opened by the [`aigora-feedback`](../skills/aigora-feedback) skill land here — one markdown file per submission, named `<YYYYMMDD-HHMMSS>-<type>-<slug>.md`.

Each file is a builder's feedback about Aigora (a bug, a feature request, or general feedback). The **pull request** is the submission artifact for the hackathon feedback track; the PR link is what a builder submits into the hackathon submission skill.

Use `https://github.com/trionlabs/aigora-skills/pull/<number>` for the feedback artifact, including where event instructions call it a "Feedback Issue URL". Include an agent profile URL in the entry's separate optional field: copy the full URL from the deployment you used, with `/services/<chainId>_<lowercaseIdentityRegistryAddress>_<agentId>`.

Select the **Network** you actually used, and describe the app origin (`aigora.org`, `aigora-prd.web.app` or another deployment) in **Steps to reproduce** for bugs, or **Anything else** for feature/general feedback. The origins can serve different builds; a report about one does not establish the same behavior on the other.

Maintainers review incoming PRs, label them by type, and judge the top 10 most valuable feedbacks for the CELO prize.

Don't hand-edit files in here — they arrive via PR.
