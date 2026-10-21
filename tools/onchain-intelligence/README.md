# AriB Research On-Chain Intelligence — Public API

What is happening on-chain for Bitcoin and Ethereum, what changed, and how fresh is the data?

Free keyed API with small limits for personal and research use. Research only, not financial advice.

- **Docs page:** https://onchain.aribresearch.com/api-docs
- **Base URL:** `https://onchain.aribresearch.com/public-api/v1`
- **Methodology:** `onchain-v1`

## Key

Send your key in the `X-API-Key` header. Keys in the URL are refused.
Free, issued by hand on request through https://onchain.aribresearch.com/feedback (type **"Other"**). Keys expire after 365 days.

```bash
curl -H "X-API-Key: YOUR_KEY" "https://onchain.aribresearch.com/public-api/v1/score?asset=btc"
```

## Endpoints

| Method | Path | Returns |
|--------|------|---------|
| GET | `/public-api/v1/` | API index: endpoints, limits, attribution |
| GET | `/public-api/v1/score` | Latest On-Chain Score, band, signal agreement and data coverage for one asset (`?asset=btc\|eth`) |
| GET | `/public-api/v1/metrics` | Latest value, observation date and status of each metric for one asset (`?asset=btc\|eth`) |
| GET | `/public-api/v1/history` | Daily score and band, last N days (`?asset=btc\|eth&days=1..90`, default 30) |
| GET | `/public-api/v1/forward-record` | Scores as recorded at publication time (`?asset=btc\|eth`) |
| GET | `/public-api/v1/openapi.json` | This API as OpenAPI 3 (no key needed) |

`asset` is required on `score`, `metrics`, `history` and `forward-record`.

## Limits

| Limit | Value |
|-------|-------|
| Requests per minute per key | 30 |
| Requests per day per key | 1000 |
| Requests per day, all keys together | 20000 |
| Rows per response | 500 |
| History | last 90 days |
| Key lifetime | 365 days |

Over a limit: HTTP `429` with `Retry-After` and `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset` (and `-Day` variants).
No bulk export.

## Response

Every response is JSON with `attribution`, `license`, `notice` ("Research only, not financial advice"), `as_of` and `data`.

Errors: `{"error": {"code", "message"}}`

| Status | Meaning |
|--------|---------|
| 401 | Missing or invalid key |
| 429 | Rate limited (`Retry-After`, `X-RateLimit-*` headers) |
| 503 | Data temporarily unavailable |

## Datasets

| Id | Description | Unit | Frequency |
|----|-------------|------|-----------|
| `onchain_score` | Daily On-Chain Score (0–100) and band for BTC and ETH, methodology onchain-v1 | score 0–100 | daily |
| `latest_metrics` | Latest value, observation date, status and data class of each displayed metric | mixed (see unit field) | daily |
| `score_history` | Score and band for the last 90 days | score 0–100 | daily |
| `forward_record` | Published scores recorded at publication time, never revised (last 90 days) | score 0–100 | daily |

## Licence and attribution

Personal and internal use. Attribution required. No redistribution, resale, scraping or bulk download.

**Attribution:** AriB Research On-Chain Intelligence (onchain.aribresearch.com). Contains Coin Metrics Community network data (CC BY-NC 4.0).

## Files

- [manifest.json](manifest.json): machine-readable summary
- [openapi.json](openapi.json): OpenAPI 3 spec
