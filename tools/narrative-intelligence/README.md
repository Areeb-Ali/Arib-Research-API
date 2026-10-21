# AriB Narrative Intelligence: Public API

Point-in-time crypto news sentiment and narrative data from AriB Research Narrative Intelligence.

Research only, not financial advice.

- **Docs page:** https://narratives.aribresearch.com/research/api
- **Base URL:** `https://narratives.aribresearch.com/api/v1`
- **Methodology:** `1.0.0-rc2`

## What it answers

What is crypto news covering, how is its tone moving, and how strong is the evidence?
The data comes from about 20 crypto news outlets plus Google News, scored with CryptoBERT and VADER, with each story counted once.

## Access

A key is optional for the free tier. Send it in the `X-API-Key` header.
Keys are issued on request from https://narratives.aribresearch.com/pricing and expire after 365 days unless stated otherwise.

| Free tier | Limit |
|-----------|-------|
| Requests per minute | 10 |
| Requests per hour | 60 |
| Requests per day | 500 |
| All free callers together | 20,000 per day |
| Rows per response | 500 |
| History | last 30 days |

Responses carry `X-RateLimit-Limit`, `X-RateLimit-Remaining` and `X-RateLimit-Reset`.
A `429` carries `Retry-After` in seconds. There is no bulk export.
Browser cross-origin requests are allowed only from AriB Research sites, so call the API from your server.

## Endpoints

| Method | Path | Returns |
|--------|------|---------|
| GET | `/api/v1/sentiment` | Current news tone, evidence counts and leading narratives |
| GET | `/api/v1/history/{series}` | History for `index`, `news-tone`, `coin`, `price` or `calls`. Query: `start`, `end` (ISO-8601), `coin`, `format=json` or `csv` |
| GET | `/api/v1/coins` | Current per-coin Sentiment Index |
| GET | `/api/v1/alerts` | Latest alerts (`limit` up to 500) |
| GET | `/api/v1/lab/leadlag` | Lead/lag statistics of an index against price. Query: `series`, `days` |
| GET | `/api/v1/lab/backtest` | Rule backtest on stored history (Track Record rules by default) |

Every row carries the time it was produced. Points rebuilt after an outage are flagged `backfilled: true`.
Unavailable values are `null`, never estimated.

| Status | Meaning |
|--------|---------|
| 401 | Invalid, expired or revoked key |
| 429 | Rate limit reached; see `Retry-After` |

## Example

```bash
curl -H "X-API-Key: YOUR_KEY" "https://narratives.aribresearch.com/api/v1/history/index?format=csv" -o index.csv
```

## Datasets

| Id | Description | Unit | Frequency |
|----|-------------|------|-----------|
| `sentiment_index` | Original AriB Sentiment Index (0-100, relative to its own baseline), one point per cycle | index 0-100 | about every 10 minutes |
| `news_tone` | Weighted tone of crypto news (-1 to +1) with two standard errors and item counts | tone -1..+1 | about every 10 minutes |
| `coin_index` | Sentiment Index per coin from headlines naming the coin | index 0-100 | about every 10 minutes |
| `prices` | Coin prices recorded at collection time (CoinGecko) | USD | about every 10 minutes |
| `track_record_calls` | Graded index calls and crossovers with the BTC move after each | percent | on each call |
| `alerts` | Alerts produced by the system: index calls, crossovers, confirmed stories, coin regimes | event | on each event |

## Attribution and licence

Every JSON response includes `attribution`, `license` and `notice` fields.
CSV responses carry them as `X-Attribution`, `X-License` and `X-Notice` headers.

- **Attribution:** AriB Research Narrative Intelligence (aribresearch.com)
- **Licence:** Personal and internal use. Redistribution requires written permission. Attribution required.
- No scraping, crawling or bulk download of the site.
- Research only, not financial advice.

## Files

- [manifest.json](manifest.json): machine-readable summary
- [openapi.json](openapi.json): OpenAPI 3 spec
