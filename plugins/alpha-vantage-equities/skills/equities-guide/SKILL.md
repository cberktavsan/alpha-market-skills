---
name: equities-guide
description: Routes equity market requests to the correct skill within the alpha-vantage-equities plugin.
metadata:
  version: "1.0.0"
---

# Equities Plugin — Orchestration Guide

This skill routes equity-related requests to the correct specialized skill. The assistant reads this skill first for any stock, fundamental, technical, or news-sentiment query.

## Global Rules

**Language**: The assistant responds in the user's language. Instructions below are English; all user-facing output matches the user's language.

**Free Tier Only**: ~25 calls/day, ~5/min. NEVER call premium endpoints: `TIME_SERIES_INTRADAY`, `TIME_SERIES_DAILY_ADJUSTED`, `REALTIME_BULK_QUOTES`, `REALTIME_OPTIONS`, `HISTORICAL_OPTIONS`, `VWAP`, `FX_INTRADAY`, `DIGITAL_CURRENCY_INTRADAY`.

**Rate Budget**: The assistant states the expected API call count before executing multi-step workflows. Single query = 1-2 calls. Standard analysis = 3-5 calls. Deep analysis = 6-10 calls. Comprehensive report = 10-15 calls (warn the user first).

**Session Awareness**: The assistant reuses data fetched earlier in the conversation rather than calling the API again for the same symbol. The assistant tracks the approximate number of API calls made in the session and warns when approaching 20.

**Tool Strategy**: The assistant calls `TOOL_GET` once per tool type per session and reuses the learned schema for subsequent `TOOL_CALL` invocations. Never use: `PING`, `ADD_TWO_NUMBERS`, `SEARCH`, `FETCH`.

**Output Handling**: The assistant extracts only the needed subset from time-series responses (latest 5-10 data points). Raw multi-page JSON is never dumped into the conversation. Financial statement responses are summarized to key metrics only.

**Disclaimer**: The assistant ends every analysis with: "Data may be delayed (free tier). This is data-driven analysis, not investment advice. Past performance does not guarantee future results."

## Skill Routing Table

| User Intent | Primary Skill | Secondary Skill(s) |
|---|---|---|
| Stock price / quote / comparison | stock-analysis | — |
| Company financials / valuation / earnings | fundamental-analysis | — |
| RSI, MACD, moving averages, indicators | technical-analysis | — |
| News, sentiment, insider trades | news-sentiment | — |
| "Full analysis of [TICKER]" | stock-analysis | fundamental-analysis + technical-analysis + news-sentiment |
| "[TICKER] technical" | technical-analysis | stock-analysis (for price context) |
| "[TICKER] fundamentals" | fundamental-analysis | — |
| "[TICKER] news" | news-sentiment | — |
| Earnings calendar / IPO calendar | fundamental-analysis | — |

For multi-skill workflow details, the assistant refers to `references/workflows.md`.

## Disambiguation Rules

- "[TICKER]" alone → stock-analysis (just get quote)
- "[TICKER] analysis" / "analyze [TICKER]" → Full Stock Report workflow
- "[TICKER] technical" → technical-analysis only
- "[TICKER] fundamentals" → fundamental-analysis only
- "[TICKER] news" → news-sentiment only
- "Earnings calendar" / "IPO calendar" → fundamental-analysis

## Error Handling

- **Rate limit hit**: The assistant informs the user and suggests waiting 1 minute or continuing next session.
- **Symbol not found**: The assistant uses `SYMBOL_SEARCH` via stock-analysis to find the correct ticker.
- **Empty response**: The assistant explains the endpoint may lack data for that symbol/date and suggests alternatives.
- **Premium error**: The assistant skips that endpoint, notes it to the user, and continues with remaining data.

## Success Criteria

A successful equities response: (1) answers the user's question with specific numbers, (2) provides context for those numbers (trend, comparison, threshold), (3) uses no more API calls than necessary, (4) includes the disclaimer.

## Cross-Plugin Note

For commodity prices, forex rates, or economic indicators, the user should install the corresponding Alpha Vantage plugin (alpha-vantage-macro or alpha-vantage-fx-crypto).
