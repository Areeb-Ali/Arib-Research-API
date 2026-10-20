# Derivatives Intelligence Data API (free beta)

The same numbers as the site, for your own scripts and dashboards.
Research only, not financial advice.

- **Docs page:** https://derivatives.aribresearch.com/api
- **Base URL:** `https://api-derivatives.areebalikhan.com/v1`
- **Question:** What are crypto derivatives traders positioned for right now?
- **Methodology:** `composite-1.2`

## Key

Sign in, open **Account**, create a key. One key per account. It is shown once.

## Auth

Send the key in a header. Never in the URL.

```
X-API-Key: di_live_...
```

or

```
Authorization: Bearer di_live_...
```

## Limits (beta)

| Limit | Value |
|-------|-------|
| Per minute | 2 |
| Per day / week / month / quarter / year | 50 / 200 / 600 / 1,500 / 5,000 |
| History | last 24 hours |
| Rows per response | at most 500 |
| Global cap | one daily cap for all keys together |

Quotas belong to the account, so a new key does not reset them.
Every response has `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset` and `X-Quota-*` headers.
A `429` has `Retry-After`.

## Endpoints (GET)

Assets: `BTC`, `ETH`, `SOL`.

| Path | What |
|------|------|
| `/status` | service status and your plan |
| `/markets` | headline numbers for BTC, ETH, SOL |
| `/assets/{asset}` | full current view of one asset |
| `/assets/{asset}/regime` | current regime and zone |
| `/assets/{asset}/history?range=24h` | score and price history inside your plan |
| `/assets/{asset}/liquidations` | liquidation totals |
| `/assets/{asset}/liquidation-levels` | estimated liquidation levels (model) |
| `/assets/{asset}/options/expiries` | options expiry calendar and max pain |
| `/regimes?asset=BTC&limit=100` | recent regime changes |

## Responses

Success:

```json
{ "data": ..., "meta": { "generatedAt", "dataAgeMinutes", "attribution", "license", "notice" } }
```

Error:

```json
{ "error": { "code", "message", "docs" } }
```

Error codes: `missing_key`, `invalid_key`, `revoked_key`, `plan_limit`, `rate_limited`, `quota_exceeded`, `global_limit`, `unknown_asset`, `bad_range`, `warming_up`, `not_found`.

## Example

```bash
curl -H "X-API-Key: $DI_KEY" https://api-derivatives.areebalikhan.com/v1/markets
```

## Attribution and terms

Show **"Derivatives Intelligence by AriB Research"** with the data.
Personal and internal use only. Reselling, bulk redistribution or showing the data inside another product needs written permission.
See the Terms on the site.

## Files

- [manifest.json](manifest.json): machine-readable summary
- [openapi.json](openapi.json): OpenAPI 3 spec
