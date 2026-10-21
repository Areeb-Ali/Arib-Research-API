# Opportunity Intelligence Public API

Which crypto futures contracts are seeing unusual volatility expansion right now, and can they be traded cleanly?

A small, free, read-only API by AriB Research. It returns the same research views the website shows:
crypto futures contracts with unusual volatility expansion on MEXC, Binance and Bybit, each exchange on its own data.
Research only, not financial advice.

- **Docs page:** https://radar.aribresearch.com/api-docs
- **Base URL:** `https://radar.aribresearch.com/public-api/v1`
- **Methodology:** `1.1.0`

## Key

A free personal key in the `X-API-Key` header. Keys in the URL are refused.

Ask through the feedback page on the site and say what you will use it for. Keys are valid 90 days.

```bash
curl -H "X-API-Key: YOUR_KEY" "https://radar.aribresearch.com/public-api/v1/opportunities?exchange=mexc&status=live"
```

## Endpoints

| Method | Path | Returns |
|--------|------|---------|
| GET | `/status` | Health of each exchange connector and data freshness |
| GET | `/exchanges` | Supported exchanges and their current availability |
| GET | `/opportunities?exchange=mexc&status=live&hours=24` | Recent and live opportunities of one exchange |
| GET | `/opportunities/{uid}` | One opportunity: detection evidence, lifecycle, follow-up observations |
| GET | `/performance?exchange=mexc&horizon=60` | Historical forward performance summary of one exchange |

### Parameters

| Parameter | Endpoint | Values |
|-----------|----------|--------|
| `exchange` | `/opportunities`, `/performance` | Exchange id, default `mexc` |
| `status` | `/opportunities` | `all`, `live`, `detected`, `active`, `expired`, `invalidated` |
| `hours` | `/opportunities` | 1 to 72 |
| `horizon` | `/performance` | 15, 30, 60, 120 or 240 (minutes) |

## Limits

| Limit | Value |
|-------|-------|
| Requests per minute per key | 30 |
| Requests per day per key | 1000 |
| Requests per day, all keys together | 20000 |
| Rows per response | at most 500 |
| History | 3 days |
| Key validity | 90 days |

A reached limit answers `429` with `Retry-After`, `X-RateLimit-Limit`, `X-RateLimit-Remaining` and `X-RateLimit-Reset`.
No bulk export.

## Response

Every answer is `{schema_version, as_of, source, status, attribution, license, notice, data}`.
Values that are not available are `null`, never estimated.

Errors: `{"error": {"code", "message", "ref"}}`

| Status | Meaning |
|--------|---------|
| 401 | Missing or invalid key |
| 429 | Limit reached, see `Retry-After` and `X-RateLimit` headers |

## Datasets

| Id | Description | Unit | Frequency |
|----|-------------|------|-----------|
| `status` | Connector state and data freshness per exchange | state, ms timestamps | every 5 minutes |
| `exchanges` | Exchange catalog with availability | n/a | every 5 minutes |
| `opportunities` | Detected volatility expansion opportunities with redacted evidence, lifecycle and forward observations | scores 0 to 100, returns as fractions | every 5 minutes |
| `performance` | Forward performance and base rate comparison, insufficient evidence shown as such | rates as fractions with intervals | as observations complete |

## License

Personal and internal use. No resale, redistribution, scraping, crawling or bulk download.

Credit **"AriB Research Opportunity Intelligence"** when you show the data.

## Files

- [manifest.json](manifest.json): machine-readable summary
- [openapi.json](openapi.json): OpenAPI 3 spec
