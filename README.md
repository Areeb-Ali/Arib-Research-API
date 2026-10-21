# AriB Research API

Free public data APIs for the AriB Research crypto intelligence tools.
Each tool has its own API, key and limits. All APIs are in **free beta**.

> Research only, not financial advice.

## Tools

| Tool | Question it answers | Docs |
|------|---------------------|------|
| Derivatives Intelligence | What are crypto derivatives traders positioned for right now? | [docs](tools/derivatives-intelligence/README.md) |
| Liquidity Intelligence | Is liquidity entering or leaving crypto, and is institutional capital entering or leaving through spot ETFs and corporate treasuries? | [docs](tools/liquidity-intelligence/README.md) |
| Narrative Intelligence | What is crypto news covering, how is its tone moving, and how strong is the evidence? | [docs](tools/narrative-intelligence/README.md) |
| Macro Intelligence | What macro regime are markets in, and what is driving it? | [docs](tools/macro-intelligence/README.md) |
| Event Calendar | What scheduled events in the coming days and weeks could matter for crypto markets, and what just happened? | [docs](tools/event-calendar/README.md) |
| On-Chain Intelligence | What is happening on-chain for Bitcoin and Ethereum, what changed, and how fresh is the data? | [docs](tools/onchain-intelligence/README.md) |
| Opportunity Intelligence | Which crypto futures contracts are seeing unusual volatility expansion right now, and can they be traded cleanly? | [docs](tools/opportunity-intelligence/README.md) |

More tools will be added here one by one.

## How it works

- Every tool has its own base URL and its own key.
- Get a key by signing in on the tool's site, opening **Account** and creating a key.
- Send the key in a header. Never put it in the URL.
- Every API has rate limits and quotas. They are listed on each tool's page.

## Repository layout

```
tools/<tool-slug>/
  README.md      human docs: limits, endpoints, errors, terms
  manifest.json  machine-readable summary of the API
  openapi.json   OpenAPI 3 spec
```

## Terms

Data is for personal and internal use. Credit the tool when you show its data.
Reselling, bulk redistribution or showing the data inside another product needs written permission.
