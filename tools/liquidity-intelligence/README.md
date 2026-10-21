# Liquidity Intelligence free API

Is liquidity entering or leaving crypto, and is institutional capital entering or leaving through spot ETFs and corporate treasuries?

Research only, not financial advice.

- **Docs page:** https://liquidity.aribresearch.com/developers
- **Base URL:** `https://liquidity.aribresearch.com/public-api/v1`
- **Methodology:** `liquidity-v1 / inst-1.0`

## Key

Free key on request: use the **Give Feedback** button on the site and choose **Other**.
Keys are issued by hand and expire after 365 days.

Send the key in the `X-API-Key` header. Keys in the URL are refused.

```bash
curl -H "X-API-Key: YOUR_KEY" https://liquidity.aribresearch.com/public-api/v1/liquidity/latest
```

## Limits

| Limit | Value |
|-------|-------|
| Requests per minute per key | 30 |
| Requests per day per key | 1000 |
| Requests per day, all keys together | 20000 |
| Rows per response | 500 |
| History | 90 days |
| Key validity | 365 days |

Over a limit: HTTP `429` with `Retry-After`.
Every response has `X-RateLimit-Limit`, `X-RateLimit-Remaining` and `X-RateLimit-Reset`.

## Endpoints

| Method | Path | Returns |
|--------|------|---------|
| GET | `/public-api/v1/meta` | Limits, endpoints and datasets of this API |
| GET | `/public-api/v1/liquidity/latest` | Latest Liquidity Score, state, zone and inputs |
| GET | `/public-api/v1/liquidity/history` (`days`) | Daily Liquidity Score and state for recent days |
| GET | `/public-api/v1/institutional/latest` | Institutional flow state, coverage and evidence |
| GET | `/public-api/v1/institutional/etf-flows` (`asset`, `days`) | Daily 7 and 30 day spot ETF net flow sums for recent days |

Parameters: `days` is 1 to 90 (default 30). `asset` is `BTC` (default) or `ETH`.

## Response

Every response: `schema_version`, `as_of`, `source`, `status`, `attribution`, `license`, `notice`, `data`.
Missing data is `null` with a reason, never estimated.

Errors: `{ "error": { "code", "message" } }`

| Status | Meaning |
|--------|---------|
| 400 | Bad parameter, or a key sent in the URL |
| 401 | Missing or invalid `X-API-Key` |
| 429 | Limit reached. See `Retry-After` and `X-RateLimit` headers |
| 503 | Data temporarily unavailable |

## Datasets

| Id | Description | Unit | Frequency | Attribution |
|----|-------------|------|-----------|-------------|
| `liquidity_score` | Liquidity Score (0-100), state (Expanding, Stable, Draining) and zone | score 0-100 | daily | AriB Research Liquidity Intelligence |
| `liquidity_inputs` | Current value, reading and data date of each Liquidity Score input | mixed (USD, %, transfers/day) | daily | AriB Research Liquidity Intelligence |
| `institutional_state` | Institutional flow state (Inflows, Outflows, Mixed), coverage and evidence per input | label | daily | AriB Research Liquidity Intelligence |
| `etf_flow_sums` | US spot ETF 7 and 30 day net flow sums | USD | daily | AriB Research Liquidity Intelligence. Spot ETF flow figures derived from The Commoner Bitcoin ETF Tracker by TFTC (CC BY 4.0) |

## License

Personal and internal use. Attribution required: **AriB Research Liquidity Intelligence**.
No redistribution as a data feed, no scraping, crawling or bulk download.

## Files

- [manifest.json](manifest.json): machine-readable summary
- [openapi.json](openapi.json): OpenAPI 3 spec
