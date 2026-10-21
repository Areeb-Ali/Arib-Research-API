# Technical Intelligence: free API

What is the chart of this coin doing right now: trend, momentum, volume and key levels, in one score?

Free technical readings for crypto coins from AriB Research: the tracked coin list, the latest technical score (0 to 100)
with its state, trend, RSI, volume and setup tags, and a short history of stored scores.

Research only, not financial advice. Scores describe what price and volume have done.
They are not forecasts, trade signals or recommendations.

- **Docs page:** https://technical.aribresearch.com/api-docs
- **Base URL:** `https://technical.aribresearch.com/public-api/v1`
- **OpenAPI 3:** https://technical.aribresearch.com/public-api/v1/openapi.json
- **Methodology:** `pre-v1`

## Get a key

Sign in at https://technical.aribresearch.com/account with Google and press **Create API key**.
The key is shown once. One key per account. Keys expire after 365 days and can be revoked or replaced on the same page.

Send it in the `X-API-Key` header. A key in the URL is refused.

```bash
curl -H "X-API-Key: YOUR_KEY" "https://technical.aribresearch.com/public-api/v1/score/BTC?tf=1d"
```

## Endpoints (GET)

| Path | Returns |
|------|---------|
| `/coins` | Tracked coins (symbol, base, name) |
| `/scores?tf=1d` | Latest score for every tracked coin on one timeframe (`4h`, `1d`, `1w`) |
| `/score/{symbol}?tf=1d` | Latest score for one coin, for example `BTC` |
| `/history/{symbol}?tf=1d&days=30` | Stored score snapshots for one coin, at most 30 days |
| `/openapi.json` | This API as OpenAPI 3 (no key needed) |

`tf` defaults to `1d`. On `/history` it also accepts `1h` and `1M`.

## Response shape

Example values, not real data:

```json
{
  "schema_version": 1,
  "as_of": "2026-10-21T09:00:00Z",
  "source": "technical-intelligence",
  "status": "ok",
  "data": {"symbol": "BTCUSDT", "tf": "1d", "score": 61, "state": "Weak Bullish", "trend": "bullish",
           "rsi": 55.2, "rel_volume": 1.1, "setups": ["bullish_trend"], "price": 83194.65, "name": "Bitcoin"},
  "attribution": "Technical Intelligence by AriB Research (https://technical.aribresearch.com). Market data: Binance public market data.",
  "license": "Personal and internal use with attribution. No resale, no redistribution of bulk data, no use to build a competing data product.",
  "notice": "Research only, not financial advice"
}
```

`status` is `ok`, `partial`, `empty` or `unavailable`. When market data can't be loaded the values are `null`.
Nothing is estimated or filled in.

Errors: `{"error": {"code", "message", "ref"}}`

| Status | Meaning |
|--------|---------|
| 401 | Missing, invalid, expired or revoked key |
| 429 | Rate limit reached (see `Retry-After` and `X-RateLimit-*` headers) |

## Limits

| Limit | Value |
|-------|-------|
| Requests per minute, per key | 30 |
| Requests per day, per key | 1000 |
| Requests per day, all free keys together | 20000 |
| Rows per response | 500 |
| History | 30 days |
| Key validity | 365 days |

Over a limit you get HTTP `429` with `Retry-After`.
Every response carries `X-RateLimit-Limit`, `X-RateLimit-Remaining` and `X-RateLimit-Reset`.
There is no bulk export and no raw exchange data.

## Datasets

| Id | Description | Unit | Frequency |
|----|-------------|------|-----------|
| `coins` | Tracked coins (symbol, base, name) | none | static |
| `technical_scores` | Technical score 0 to 100, state, trend, RSI, volume vs usual and setup tags, computed by AriB Research from Binance candles | score 0-100 | recomputed at most every 2 minutes |
| `score_history` | Stored score snapshots for one coin and timeframe, short history only | score 0-100 | at most one snapshot per 15 minutes per coin and timeframe |

## Rules

Personal and internal use with attribution: show **"Technical Intelligence by AriB Research"** with a link wherever you display the data.
No resale, no redistribution of bulk data, no scraping, crawling or bulk download, and no use to build a competing data product.
Market data comes from Binance public market data.

Full terms: https://technical.aribresearch.com/terms

## Files

- [manifest.json](manifest.json): machine-readable summary
- [openapi.json](openapi.json): OpenAPI 3 spec
