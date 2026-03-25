---
name: fx-crypto-guide
description: Routes forex and cryptocurrency requests to the correct skill within the alpha-fx-crypto plugin.
metadata:
  version: "1.0.0"
---

# FX & Crypto Plugin — Orchestration Guide

This skill routes foreign exchange and cryptocurrency requests. The assistant reads this skill first for any forex or crypto query.

## Global Rules

**Language**: The assistant responds in the user's language. Instructions below are English; all user-facing output matches the user's language.

**API Limits**: Free tier allows ~25 calls/day. Premium users have higher limits (no daily cap). The assistant may call any Alpha Vantage endpoint. If a call returns a premium-required error, the assistant continues the analysis with the remaining data and lists the failed premium calls at the end of the response with a link to https://www.alphavantage.co/premium/.

**Rate Budget**: The assistant states the expected API call count before executing multi-step workflows. Single query = 1-2 calls. Standard analysis = 3-5 calls. Deep analysis = 4-6 calls (warn the user first).

**Session Awareness**: The assistant reuses data fetched earlier in the conversation rather than calling the API again for the same pair. The assistant tracks the approximate number of API calls made in the session and warns when approaching 20.

**Tool Strategy**: The assistant calls `TOOL_GET` once per tool type per session and reuses the learned schema. Never use: `PING`, `ADD_TWO_NUMBERS`, `SEARCH`, `FETCH`.

**Output Handling**: The assistant extracts only the needed subset from time-series responses (latest 5-10 data points). Raw multi-page JSON is never dumped into the conversation.

**Disclaimer**: The assistant ends every analysis with: "This is data-driven analysis, not investment advice. Past performance does not guarantee future results."

## Skill Routing Table

| User Intent | Primary Skill |
|---|---|
| Currency rates, forex pairs, exchange rates | forex-analysis |
| Bitcoin, Ethereum, crypto prices | crypto-analysis |
| "Where is the dollar heading?" | forex-analysis |
| "BTC analysis" / crypto with trend | crypto-analysis |
| Multi-currency comparison | forex-analysis |
| Multi-crypto comparison | crypto-analysis |

For multi-step workflow details, the assistant refers to `references/workflows.md`.

## Disambiguation Rules

- "Dollar" / "euro" / currency name → forex-analysis
- "Bitcoin" / "BTC" / crypto name → crypto-analysis
- "Dollar rate" (ambiguous) → The assistant infers the counter currency from the user's language (Turkish user → USD/TRY, etc.)
- "[pair] technical" → The skill calls technical indicator tools directly (RSI, MACD, BBANDS use the same Alpha Vantage MCP tools)

## Error Handling

- **Rate limit hit**: The assistant informs the user. FX/crypto markets are 24/7; suggests resuming later.
- **Symbol not found**: The assistant verifies the correct Alpha Vantage symbol format (e.g., `BTC` not `BTCUSD` for crypto).
- **Premium error**: The assistant completes the analysis with all available data first, then appends at the end: "The following calls require a premium API key: [list]. Upgrade at https://www.alphavantage.co/premium/".

## Success Criteria

A successful FX/crypto response: (1) shows the rate in both directions, (2) includes timestamp (critical for 24/7 markets), (3) provides context (daily change, trend), (4) uses the user's likely local currency where applicable, (5) includes the disclaimer.

## Cross-Plugin Note

For technical indicator overlays (RSI, MACD, BBANDS on FX/crypto pairs), the assistant can call indicator tools directly via the Alpha Vantage MCP server — these tools work with any symbol type. For economic context (Fed rate, GDP), the user should install alpha-macro.
