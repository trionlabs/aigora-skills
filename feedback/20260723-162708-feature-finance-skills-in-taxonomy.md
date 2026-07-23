### Contact
caxtonacollins@gmail.com · Telegram @caxtonacollins

### CELO payout wallet
0x3E192d109d1dd323375Ac1Ed040f817918E82d63

### Aigora profile URL
https://aigora.org/services/9719

### Surface
Agent discovery / catalog

### Network
Mainnet — Celo (chainId 42220)

### Problem / motivation
I registered an agent that sells financial data and execution: oracle-priced Mento swap quotes, a live market snapshot with volatility scoring, swap routing recommendations, and market analysis — six endpoints, paid per call in USDC over x402 on Celo mainnet.

When I reached the Capabilities section I could not describe any of that.

The **Skills** dropdown — the field the form labels *"standardized OASF skills & domains (indexed by 8004scan)"* — contains no financial capability at all. Scrolling the full list: Text Summarization, Text Completion, Paraphrasing, Story / Content Generation, Question Answering, Knowledge Synthesis, Fact Extraction, Translation, Problem Solving, Content Moderation, Named Entity Recognition, Code Refactoring & Optimization, Code Generation, Mathematical Reasoning, Document Retrieval (RAG), Document/DB Q&A (RAG), Augmented Generation (RAG), Image Generation, Object Detection, Text to Image, Visual Q&A, Speech Recognition, Feature Engineering, Data Quality Assessment, Schema Inference, Multi-Agent Planning, Task Decomposition, API / Tool Use, Workflow Automation, Threat Detection, Vulnerability Analysis, Compliance Assessment, CI/CD Configuration, Quality Evaluation, Chain-of-Thought Reasoning.

There is no "price quote", no "market data", no "swap routing", no "payment settlement", no "portfolio analysis". The taxonomy has a slot for CI/CD Configuration and Object Detection but none for quoting a price — on a marketplace whose host chain is built for payments.

What makes this sharper is that the *other* two classification fields on the same form do know finance exists:

| Field | Finance coverage |
|---|---|
| **Domains** | Banking, Investment Services, Retail (Finance), Risk Management |
| **Categories** | Trading |
| **Skills** | *nothing* |

So the form lets me declare the *industry* I operate in and the *category* I sit under, but not the *capability* I actually sell. And Skills is the field described as indexed by 8004scan — the searchable one, the one another agent would query to find a price feed.

The result is that my agent is now on-chain describing itself as:

    tool_interaction/api_schema_understanding
    natural_language_processing/analytical_reasoning/problem_solving
    tool_interaction/workflow_automation
    natural_language_processing/information_retrieval_synthesis/question_answering

Those are the four least-wrong options available. None of them tells a potential buyer that this agent quotes stablecoin swap prices on Celo. An agent searching the catalog for a market-data provider cannot find me, and I cannot be distinguished from a generic chatbot.

### Proposed feature
Add a finance/payments branch to the OASF skill set used by the Skills dropdown. Concretely, skills along the lines of:

- `finance/price_quote` — return a priced quote for an asset pair
- `finance/market_data` — live rates, depth, volatility, tradability
- `finance/swap_routing` — choose a venue/route for an exchange
- `finance/payment_settlement` — execute or settle a payment onchain
- `finance/portfolio_analysis` — positions, exposure, PnL
- `finance/risk_scoring` — slippage, liquidity, counterparty risk

That set would let a DeFi or payments agent state what it does in the one field the catalog actually indexes, and would let buyers filter for it.

If the OASF list is upstream and not yours to change, two smaller fixes would still help a lot:

1. Allow a handful of free-text skills alongside the standardized ones, so builders can describe capabilities the taxonomy hasn't caught up with. (Your own `aigora-register` docs currently describe skills as free text — see my separate documentation report.)
2. Index Domains and Categories for search too, so `finance_and_business/banking` + `trading` at least make a financial agent discoverable while the skill vocabulary catches up.

### Alternatives
- **Using Domains + Categories to signal finance.** This is what I ended up doing (`technology/blockchain`, `finance_and_business/retail`, `finance_and_business/banking`, category `trading`). It doesn't solve it — those describe industry and section, not capability, and by the form's own labelling they are not the indexed field.
- **Free-text skills.** The `aigora-register` documentation describes skills as free text with a ≤32 character name and a markdown description, which would have solved this cleanly. The live form doesn't offer it.
- **Describing the capability in the free-form description field.** Works for a human reading the profile, useless for programmatic discovery, which is the point of a machine-readable capability list.

### Anything else
For context on what the agent actually sells, the public catalog is at https://www.jahpay.xyz/api/agent/catalog — six endpoints, each priced, five of them returning HTTP 402 until paid. That document expresses the agent's capabilities far better than anything I was able to select on the Register form, which seems like the gap worth closing.
