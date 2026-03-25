---
name: macro-guide
description: Routes macroeconomic, commodity, and market overview requests to the correct skill within the alpha-macro plugin.
metadata:
  version: "1.0.0"
---

# Macro Plugin — Orchestration Guide

This skill routes macroeconomic, commodity, and general market requests. The assistant reads this skill first for any economy, commodity, or market-overview query.

## Global Rules

**Language**: The assistant responds in the user's language. Instructions below are English; all user-facing output matches the user's language.

**Free Tier Only**: ~25 calls/day, ~5/min. NEVER call premium endpoints: `TIME_SERIES_INTRADAY`, `TIME_SERIES_DAILY_ADJUSTED`, `REALTIME_BULK_QUOTES`, `REALTIME_OPTIONS`, `HISTORICAL_OPTIONS`, `VWAP`, `FX_INTRADAY`, `DIGITAL_CURRENCY_INTRADAY`.

**Rate Budget**: The assistant states the expected API call count before executing multi-step workflows. Single query = 1-2 calls. Standard analysis = 3-5 calls. Deep analysis = 6-10 calls. Comprehensive report = 10-15 calls (warn the user first).

**Session Awareness**: The assistant reuses data fetched earlier in the conversation rather than calling the API again for the same indicator. The assistant tracks the approximate number of API calls made in the session and warns when approaching 20.

**Tool Strategy**: The assistant calls `TOOL_GET` once per tool type per session and reuses the learned schema. Never use: `PING`, `ADD_TWO_NUMBERS`, `SEARCH`, `FETCH`.

**Output Handling**: The assistant extracts only the needed subset from time-series responses (latest 5-10 data points). Raw multi-page JSON is never dumped into the conversation.

**Disclaimer**: The assistant ends every analysis with: "Data may be delayed (free tier). This is data-driven analysis, not investment advice. Past performance does not guarantee future results."

## Skill Routing Table

| User Intent | Primary Skill | Secondary Skill(s) |
|---|---|---|
| GDP, inflation, unemployment, Fed rate | economic-dashboard | — |
| Gold, oil, commodities | commodity-tracker | — |
| Top gainers/losers, market snapshot | market-overview | — |
| "How is the market?" | market-overview | economic-dashboard |
| "Economic outlook" | economic-dashboard | commodity-tracker |
| "Gold and oil status" | commodity-tracker | — |
| "Yield curve" | economic-dashboard | — |
| "Commodity dashboard" | commodity-tracker | — |

For multi-skill workflow details, the assistant refers to `references/workflows.md`.

## Disambiguation Rules

- "Market" without specifics → market-overview (General Market Overview workflow)
- "Economy" / "macro" / "GDP" / "inflation" / "interest rate" → economic-dashboard
- "Gold" / "oil" / commodity name → commodity-tracker
- "Market overview" + "economy" → General Market Overview workflow
- "Yield curve" / "treasury" → economic-dashboard

## Error Handling

- **Rate limit hit**: The assistant informs the user. Macro data changes slowly; suggests resuming next session.
- **Empty response**: Some economic indicators update monthly/quarterly. The assistant notes the reporting lag.
- **Premium error**: The assistant skips and continues with available data.

## Success Criteria

A successful macro response: (1) provides specific data values with dates, (2) shows trend direction (improving/worsening), (3) supplies context (Fed target, historical average, previous reading), (4) includes the disclaimer.

## Cross-Plugin Note

For stock prices, technical analysis, or company fundamentals, the user should install alpha-equities. For forex or crypto data, install alpha-fx-crypto.
