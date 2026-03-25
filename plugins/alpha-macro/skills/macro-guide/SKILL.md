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

**API Limits**: Free tier allows ~25 calls/day. Premium users have higher limits (no daily cap). The assistant may call any Alpha Vantage endpoint. If a call returns a premium-required error, the assistant continues the analysis with the remaining data and lists the failed premium calls at the end of the response with a link to https://www.alphavantage.co/premium/.

**Rate Budget**: The assistant states the expected API call count before executing multi-step workflows. Single query = 1-2 calls. Standard analysis = 3-5 calls. Deep analysis = 6-10 calls. Comprehensive report = 10-15 calls (warn the user first).

**Session Awareness**: The assistant reuses data fetched earlier in the conversation rather than calling the API again for the same indicator. The assistant tracks the approximate number of API calls made in the session and warns when approaching 20.

**Tool Strategy**: The assistant calls `TOOL_GET` once per tool type per session and reuses the learned schema. Never use: `PING`, `ADD_TWO_NUMBERS`, `SEARCH`, `FETCH`.

**Output Handling**: The assistant extracts only the needed subset from time-series responses (latest 5-10 data points). Raw multi-page JSON is never dumped into the conversation.

**Disclaimer**: The assistant ends every analysis with: "This is data-driven analysis, not investment advice. Past performance does not guarantee future results."

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
- **Premium error**: The assistant completes the analysis with all available data first, then appends at the end: "The following calls require a premium API key: [list]. Upgrade at https://www.alphavantage.co/premium/".

## Success Criteria

A successful macro response: (1) provides specific data values with dates, (2) shows trend direction (improving/worsening), (3) supplies context (Fed target, historical average, previous reading), (4) includes the disclaimer.

## Cross-Plugin Note

For stock prices, technical analysis, or company fundamentals, the user should install alpha-equities. For forex or crypto data, install alpha-fx-crypto.
