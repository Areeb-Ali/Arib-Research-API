# Macro Intelligence public API

What macro regime are markets in, and what is driving it?

Read-only JSON with the current macro regime, the Macro Score and its drivers, liquidity, futures positioning, global policy rates, FX and the release calendar.

Research only, not financial advice.

- **Docs page:** https://macro.aribresearch.com/developers
- **Base URL:** `https://macro.aribresearch.com/v1`
- **Methodology:** `mi-1.1`

## Get a key

Sign in with Google at https://macro.aribresearch.com/account and create up to 3 keys.
Keys are shown once and cannot be recovered later. They do not expire; revoke them on the same page.

Send the key in the `X-API-Key` header:

```bash
curl -H "X-API-Key: YOUR_KEY" https://macro.aribresearch.com/v1/snapshot
```

Keep the key on your server, never in browser code or a public repository.

## Limits

| Limit | Value |
|-------|-------|
| Per minute | 5 |
| Per day (per account) | 100 |
| All users together, per day | 3000 |
| Rows per list in a response | 500 |
| History window | last 90 days |

Hourly, weekly, quarterly and yearly quotas also apply; current values are on the docs page.
Over a limit you get `429` with `Retry-After`. Every response carries `X-RateLimit-*` headers
(`X-RateLimit-Minute-Limit`, `X-RateLimit-Minute-Remaining`, `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Daily-Limit`, `X-RateLimit-Daily-Remaining`).
No bulk download.

## Endpoints

| Endpoint | Returns |
|----------|---------|
| `GET /v1/snapshot` | Current macro regime, Macro Score (0-100), coverage, agreement, drivers, key changes |
| `GET /v1/history` | Live Macro Score and regime since launch (last 90 days) |
| `GET /v1/snapshots` | Stored live snapshots with the displayed inputs (last 90 days) |
| `GET /v1/track-record` | Forward record since the methodology freeze with matured outcomes |
| `GET /v1/liquidity` | Net liquidity, central bank balance sheet, financial conditions, stress, funding |
| `GET /v1/positioning` | Speculator futures positioning for rates, FX, equity index, gold, crude and Bitcoin |
| `GET /v1/central-banks` | Policy rates and 6-month direction for 15 central banks |
| `GET /v1/fx` | Major FX pairs: 1D, 1W, 1M, 3M change, z-score, 1-year percentile |
| `GET /v1/calendar` | Recent and upcoming CPI, PPI, payrolls and GDP release dates |
| `GET /v1/expectations` | Published nowcasts and inflation expectations |

## Response

```json
{"api_version": "v1", "methodology_version": "mi-1.1", "generated_at": "...",
 "attribution": "...", "license": "...", "notice": "Research only, not financial advice.", "data": {}}
```

Every value has its own date and status. Missing data is reported as missing (`null` with a status), never filled in.

| Status | Meaning |
|--------|---------|
| 401 | Missing or invalid key |
| 403 | Plan without API access |
| 429 | Rate limit or quota reached; see `Retry-After` |

## Datasets

| Id | Description | Unit | Frequency | Redistribution |
|----|-------------|------|-----------|----------------|
| `macro_snapshot` | Current macro regime, Macro Score (0-100), coverage, agreement, drivers, key changes | score 0-100 | hourly | Derived by AriB Research; do not redistribute |
| `macro_score_history` | Live Macro Score and regime since launch (last 90 days) | score 0-100 | every 6 hours | Derived by AriB Research; do not redistribute |
| `snapshots` | Stored live snapshots with the displayed inputs (last 90 days) | mixed | every 6 hours | Derived by AriB Research; do not redistribute |
| `track_record` | Forward record since the methodology freeze with matured outcomes | percent | daily | Derived by AriB Research; do not redistribute |
| `liquidity` | Net liquidity, central bank balance sheet, financial conditions, stress, funding | USD billions / index / percent | weekly and daily | Official statistics; free to use with attribution |
| `positioning` | Speculator futures positioning for rates, FX, equity index, gold, crude and Bitcoin | contracts / percent of open interest / 0-100 index | weekly (Friday) | Public domain (CFTC) |
| `central_banks` | Policy rates and 6-month direction for 15 central banks | percent | daily | BIS data, attribution required |
| `fx` | Major FX pairs: 1D, 1W, 1M, 3M change, z-score, 1-year percentile | percent / z | hourly | Derived statistics only; no raw price feed |
| `calendar` | Recent and upcoming CPI, PPI, payrolls and GDP release dates | date | daily | Official statistics; free to use with attribution |
| `expectations` | Published nowcasts and inflation expectations | percent | daily | Official statistics; free to use with attribution |

## License

Personal and internal use. No resale, redistribution or republication of the data.

**Attribution:** AriB Research Macro Intelligence, built from official statistics (Federal Reserve, BLS, BEA, BIS, CFTC) and delayed market data.

Research only, not financial advice.

## Files

- [manifest.json](manifest.json): machine-readable summary
- [openapi.json](openapi.json): OpenAPI 3 spec
