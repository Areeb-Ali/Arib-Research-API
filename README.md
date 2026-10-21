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
