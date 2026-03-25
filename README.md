# Alpha Market Skills

A collection of 3 financial market analysis plugins for Claude, powered by [Alpha Vantage](https://www.alphavantage.co/). Each plugin is independently installable from the Claude marketplace.

## Plugins

| Plugin | Skills | Coverage |
|--------|--------|----------|
| **alpha-equities** | stock-analysis, fundamental-analysis, technical-analysis, news-sentiment | Stock prices, company financials, 40+ technical indicators, market news and insider activity |
| **alpha-macro** | economic-dashboard, commodity-tracker, market-overview | US economic indicators (GDP, CPI, unemployment, Fed rate), energy/metals/agriculture prices, top gainers/losers |
| **alpha-fx-crypto** | forex-analysis, crypto-analysis | Currency exchange rates, historical FX data, cryptocurrency prices and trends |

## Installation

### From Claude Marketplace

```
/plugin marketplace add cberktavsan/alpha-market-skills
```

Then enable the plugin(s) you need.

### API Key Setup

1. Get a free API key at [alphavantage.co/support/#api-key](https://www.alphavantage.co/support/#api-key)
2. Set the environment variable:
   ```bash
   export ALPHA_VANTAGE_API_KEY=your_api_key_here
   ```

## Updating

To get the latest version of installed plugins:

```
/plugin update alpha-equities@alpha-market-skills
/plugin update alpha-macro@alpha-market-skills
/plugin update alpha-fx-crypto@alpha-market-skills
```

If updates are not detected, restart your Claude Code session and try again.

## API Limits

| Tier | Daily Limit | Per-Minute Limit |
|------|-------------|------------------|
| Free | ~25 calls/day | — |
| Premium | No daily cap | 75 - 1200 calls/min (by plan) |

Each plugin's orchestrator warns before executing multi-step analyses that consume multiple calls.

| Analysis Type | Approximate Calls |
|---------------|-------------------|
| Single price check | 1-2 |
| Standard analysis | 3-5 |
| Deep analysis | 6-10 |
| Comprehensive report | 10-15 |

## Free vs Premium Endpoints

All plugins work with both free and premium API keys. Some endpoints require premium:

| Endpoint | Tier | Description |
|----------|------|-------------|
| `TIME_SERIES_INTRADAY` | Premium | Real-time intraday data |
| `TIME_SERIES_DAILY_ADJUSTED` | Premium | Adjusted daily data |
| `REALTIME_BULK_QUOTES` | Premium | Batch real-time quotes |
| `REALTIME_OPTIONS` / `HISTORICAL_OPTIONS` | Premium | Options data |
| `VWAP` | Premium | Volume-weighted average price |
| `FX_INTRADAY` | Premium | Intraday forex |
| `DIGITAL_CURRENCY_INTRADAY` | Premium | Intraday crypto |

If a premium endpoint fails, the plugin continues with available data and notes which calls require an upgrade. See [Alpha Vantage Premium](https://www.alphavantage.co/premium/) for plans.

## Language Support

All plugins respond in the user's language automatically.

## License

MIT
