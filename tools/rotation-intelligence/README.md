# Rotation Intelligence · Public API

Where is capital moving inside crypto: BTC, ETH, large caps, mid caps, sectors, or into stablecoins, and how broad is the move?

Part of AriB Research. A small, free, read-only API with the same figures the website shows.
Research only, not financial advice.

- **Docs page:** https://rotation.aribresearch.com/api-docs
- **Base URL:** `https://rotation.aribresearch.com/public-api/v1`
- **Methodology:** `1.0` (https://rotation.aribresearch.com/research/methodology)

## Access

Send your key in the `X-API-Key` header. Keys in the URL are refused.

```bash
curl -H "X-API-Key: YOUR_KEY" https://rotation.aribresearch.com/public-api/v1/state
```

Keys are free. Request one through the feedback form on the website
(https://rotation.aribresearch.com/feedback, type **"Other"**, say what you will build).
A key is shown once and is valid for 365 days.

## Limits

| Limit | Value |
|-------|-------|
| Requests per minute, per key | 30 |
| Requests per day, per key | 1,000 |
| Requests per day, all keys together | 20,000 |
| Rows per response, at most | 500 |
| History window | 90 days |

Every response carries `X-RateLimit-Limit`, `X-RateLimit-Remaining` and `X-RateLimit-Reset`.
Over a limit the API answers HTTP `429` with `Retry-After`.
There is no bulk export and no raw provider data. Browsers may call the API only from the Rotation Intelligence website (CORS).

## Endpoints

| Method | Path | Returns |
|--------|------|---------|
| GET | `/meta` | Tool, methodology version, limits, endpoints |
| GET | `/state` | Current state label, the rule that fired, coverage and every input |
| GET | `/dominance?days=30` | BTC, ETH, stablecoin and other shares of total market cap, 7/30/90-day changes, daily history |
| GET | `/ethbtc?days=30` | ETH/BTC last daily close, changes, 20/50/200-day averages, daily closes |
| GET | `/buckets` | Large, mid, small and all-alt buckets vs BTC over 7/30/90 days |
| GET | `/sectors` | Nine sector baskets vs BTC and vs the market, with breadth |
| GET | `/sectors/{key}` | One sector with its members (`ai`, `defi`, `l1`, `l2`, `memes`, `gaming`, `rwa`, `depin`, `exchange`) |
| GET | `/breadth?days=30` | Share of the top 50/100 coins beating BTC over 30/90 days, daily history |

`days` is optional, minimum 1, default 30.
`HEAD` is accepted on every path. Responses carry a weak `ETag`; send it back in `If-None-Match` to get `304 Not Modified`.

## Response shape

```json
{
  "schema_version": 1,
  "as_of": "2026-10-21T08:00:00Z",
  "source": "rotation-intelligence",
  "status": "ok",
  "attribution": "AriB Research · Rotation Intelligence (rotation.aribresearch.com). Underlying market data: CoinPaprika, CoinGecko and Binance.",
  "license": "Personal and internal use. No resale, no redistribution of bulk data, no scraping. Underlying market data remains subject to its providers' terms.",
  "notice": "Research only, not financial advice.",
  "data": {}
}
```

`status` is `ok`, `stale`, `unavailable` or `collecting`.
Missing values are `null` with a reason; nothing is estimated or filled in.

Errors look like `{"error": {"code": "...", "message": "...", "ref": "..."}}` and carry the same attribution, license and notice.
Invalid parameters return `422` with validation details.

## Datasets

| Id | Description | Unit | Frequency |
|----|-------------|------|-----------|
| `state` | Current state label, the rule that fired, coverage and every input | label / % / pp | hourly |
| `dominance` | BTC, ETH, stablecoin and other shares of total crypto market cap and their changes | % of total market cap; pp | hourly; daily history |
| `ethbtc` | ETH/BTC daily close, 7/30/90-day change, 20/50/200-day simple averages | ratio; % | daily (UTC close) |
| `buckets` | Equal- and cap-weight returns of market-cap buckets relative to BTC | % | daily (UTC close) |
| `sectors` | Equal-weight sector basket returns vs BTC and vs a top-100 market index, breadth | % | daily (UTC close) |
| `breadth` | Share of top 50/100 coins beating BTC over 30/90 days | % of coins | daily (UTC close); daily history |

Redistribution: not allowed in bulk; quote figures with attribution.

## Attribution and license

- **Attribution:** AriB Research · Rotation Intelligence (rotation.aribresearch.com). Underlying market data: CoinPaprika, CoinGecko and Binance.
- **License:** Personal and internal use. No resale, no redistribution of bulk data, no scraping. Underlying market data remains subject to its providers' terms.
- Research only, not financial advice.

## Files

- [manifest.json](manifest.json): machine-readable summary
- [openapi.json](openapi.json): OpenAPI 3.1 spec
