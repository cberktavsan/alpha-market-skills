---
name: alpha-market-skills
description: >
  Collection of 3 financial analysis plugins powered by Alpha Vantage free-tier API.
  Install individual plugins for focused analysis or the full collection for comprehensive market coverage.
metadata:
  version: "1.0.0"
  collection: true
---

# Alpha Market Skills

A collection of 3 independently installable plugins for financial market analysis using the Alpha Vantage MCP server.

## Plugins

| Plugin | Skills | What It Covers |
|--------|--------|----------------|
| **alpha-equities** | stock-analysis, fundamental-analysis, technical-analysis, news-sentiment | Stock prices, company financials, 40+ technical indicators, market news and sentiment |
| **alpha-macro** | economic-dashboard, commodity-tracker, market-overview | US economic indicators (GDP, CPI, unemployment), energy/metals/agriculture prices, market movers |
| **alpha-fx-crypto** | forex-analysis, crypto-analysis | Currency exchange rates, crypto prices and historical trends |

## Setup

All plugins require a free Alpha Vantage API key set as the `ALPHA_VANTAGE_API_KEY` environment variable.

Get a key at: https://www.alphavantage.co/support/#api-key
