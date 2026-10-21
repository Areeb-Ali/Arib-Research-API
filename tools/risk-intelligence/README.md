# Risk Intelligence · Free API

Are there verified risk events or stress conditions in crypto right now, and how has that changed?

Part of AriB Research. Reports what happened or exists; never predicts.
Research only, not financial advice.

- **Docs page:** https://risk.aribresearch.com/api-docs
- **Base URL:** `https://risk.aribresearch.com/public-api/v1`
- **Methodology:** `v1.0`

## Access

Send your key in the `X-API-Key` header. Keys in the URL are refused.

Free keys on request via the feedback form at https://risk.aribresearch.com/feedback.
Keys expire after 365 days and can be renewed.

```bash
curl -H "X-API-Key: YOUR_KEY" https://risk.aribresearch.com/public-api/v1/state
```

## Limits

| Limit | Value |
|-------|-------|
| Requests per minute per key | 30 |
| Requests per day per key | 1000 |
| Requests per day, all keys together | 20000 |
| Rows per response | 500 (page with cursor) |
| History | last 30 days |
| Key validity | 365 days, renewable |

Over a limit: HTTP `429` with `Retry-After`.
Every response carries `X-RateLimit-Limit`, `X-RateLimit-Remaining` and `X-RateLimit-Reset` (seconds until 00:00 UTC).
Read-only: `GET` and `HEAD`. No bulk download.

## Endpoints

| Method | Path | Returns |
|--------|------|---------|
| GET | `/state` | Current label, score, coverage and module status |
| GET | `/modules` | Per-module detail: severities, contributing items, peg deviations, exchange flows |
| GET | `/events` | Recorded events of the last 30 days |
| GET | `/history` | Forward-record snapshots of the last 30 days |
| GET | `/sources` | Data sources and their current health |

### Parameters

| Parameter | Endpoint | Values |
|-----------|----------|--------|
| `module` | `/events` | `security`, `pegs`, `regulatory`, `cex` |
| `since` | `/events` | Epoch seconds or ISO 8601 (within the last 30 days) |
| `min_points` | `/events` | Minimum severity points |
| `days` | `/history` | 1 to 30, default 30 |
| `limit` | `/events`, `/history` | 1 to 500, default 100 |
| `cursor` | `/events`, `/history` | `next_cursor` from the previous page |

## Response

```json
{
  "schema_version": 1,
  "as_of": "2026-10-21T08:00:00Z",
  "source": "aribresearch/risk-intelligence",
  "status": "ok",
  "data": {},
  "attribution": "AriB Research · Risk Intelligence. Underlying data: DefiLlama, SEC, CFTC, OFAC Sanctions List Service, UK FCA (see each item's source link).",
  "license": "Personal and internal use. Do not resell or redistribute bulk data. Credit AriB Research (Risk Intelligence) and the original sources.",
  "notice": "Research only, not financial advice"
}
```

Missing values are `null` with a reason; nothing is estimated.
The score is `null` below 50% coverage (status `insufficient_coverage`).

Errors: `{"error": {"code": "...", "message": "...", "ref": "..."}}`

| Status | Meaning |
|--------|---------|
| 400 | Invalid parameter |
| 401 | Missing, invalid, expired or revoked key |
| 429 | Rate limit or quota reached; see `Retry-After` and `X-RateLimit-*` headers |

## Datasets

| Id | Description | Unit | Frequency |
|----|-------------|------|-----------|
| `state` | Current state label, 0-100 risk score (null below 50% coverage), coverage, per-module status and severity | score 0-100; coverage 0-1 | every 15 minutes |
| `modules` | Module details: decayed severity points of contributing hacks and regulatory actions, stablecoin price deviation from $1, price-neutral exchange net flows | points 0-100; USD; fraction | 15 minutes to 12 hours by module |
| `events` | Hacks, peg deviations, official regulatory actions and exchange outflow events with date, severity points, source link and matched rules | points; USD | as recorded |
| `history` | Forward record of the score, coverage and per-module severity, one row per collection cycle | score 0-100; coverage 0-1 | every 15 minutes |
| `sources` | Source registry with last successful fetch time | n/a | every 15 minutes |

Redistribution: not permitted in bulk; quote individual items with credit.

## License and attribution

- **License:** Personal and internal use. Do not resell or redistribute bulk data. Credit AriB Research (Risk Intelligence) and the original sources.
- **Attribution:** AriB Research · Risk Intelligence. Underlying data: DefiLlama, SEC, CFTC, OFAC Sanctions List Service, UK FCA (see each item's source link).
- No scraping, crawling or bulk download.
- Research only, not financial advice.

## Files

- [manifest.json](manifest.json): machine-readable summary
- [openapi.json](openapi.json): OpenAPI 3 spec
