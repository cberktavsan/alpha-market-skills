# CLAUDE.md

## What This Repository Is

Alpha Market Skills — a mono-repo containing 3 marketplace plugins for financial market analysis, powered by the Alpha Vantage MCP server. Each plugin is independently installable.

## Repository Structure

```
alpha-market-skills/
  .claude-plugin/marketplace.json   — Marketplace manifest (3 plugins)
  .plugin/plugin.json               — Open Plugins format metadata
  SKILL.md                          — Collection-level metadata
  plugins/
    alpha-equities/                 — Stocks, fundamentals, technicals, news
    alpha-macro/                    — Economic indicators, commodities, market overview
    alpha-fx-crypto/                — Forex, cryptocurrency
```

Each plugin contains: `.mcp.json` + `skills/` with an orchestrator guide and specialized skills.

## MCP Server

All plugins connect to the same Alpha Vantage MCP server.
- **URL**: `https://mcp.alphavantage.co/mcp`
- **Auth**: `ALPHA_VANTAGE_API_KEY` environment variable
- **Tools**: 106 available, ~55 used by this collection

## Free Tier Constraints

- ~25 API calls/day, ~5 calls/minute
- Premium endpoints NOT used: `TIME_SERIES_INTRADAY`, `TIME_SERIES_DAILY_ADJUSTED`, `REALTIME_BULK_QUOTES`, `REALTIME_OPTIONS`, `HISTORICAL_OPTIONS`, `VWAP`, `FX_INTRADAY`, `DIGITAL_CURRENCY_INTRADAY`
- Unused utility tools: `PING`, `ADD_TWO_NUMBERS`, `SEARCH`, `FETCH`

## Plugin Boundaries

| Plugin | Skills | Domain |
|--------|--------|--------|
| alpha-equities | equities-guide, stock-analysis, fundamental-analysis, technical-analysis, news-sentiment | Stocks, company financials, 40+ indicators, market news |
| alpha-macro | macro-guide, economic-dashboard, commodity-tracker, market-overview | GDP, CPI, Fed rate, oil, gold, top movers |
| alpha-fx-crypto | fx-crypto-guide, forex-analysis, crypto-analysis | Currency pairs, crypto prices and trends |

## Skill Authoring Rules

1. SKILL.md must have YAML frontmatter with `name` and `description`
2. Keep SKILL.md under 500 lines; detailed content goes to `references/`
3. Write in third person
4. Every skill must have a **Gotchas** section
5. Do not duplicate Global Rules — they live in each plugin's orchestrator only
6. Technical indicators use tiered structure: Primary (7) → Secondary (7) → Specialized
7. Include a **Token Budget** section with target output size

## Context Engineering Principles Applied

- **Progressive disclosure**: Workflows in `references/`, indicators tiered by frequency
- **Strategic positioning**: Global rules at orchestrator top, disclaimers at end
- **Observation masking**: Extract only needed data points from API responses
- **Session awareness**: Reuse fetched data, track API call count
- **Tool consolidation**: TOOL_GET once per tool type, reuse schema
