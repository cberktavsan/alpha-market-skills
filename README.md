# Alpha Vantage Toolkit

A collection of 3 financial market analysis plugins for Claude, powered by [Alpha Vantage](https://www.alphavantage.co/). Each plugin is independently installable from the Claude marketplace.

## Plugins

| Plugin | Skills | Coverage |
|--------|--------|----------|
| **alpha-vantage-equities** | stock-analysis, fundamental-analysis, technical-analysis, news-sentiment | Stock prices, company financials, 40+ technical indicators, market news and insider activity |
| **alpha-vantage-macro** | economic-dashboard, commodity-tracker, market-overview | US economic indicators (GDP, CPI, unemployment, Fed rate), energy/metals/agriculture prices, top gainers/losers |
| **alpha-vantage-fx-crypto** | forex-analysis, crypto-analysis | Currency exchange rates, historical FX data, cryptocurrency prices and trends |

## Installation

### From Claude Marketplace

```
/plugin marketplace add cberktavsan/alpha-vantage-toolkit
```

Then enable the plugin(s) you need.

### API Key Setup

1. Get a free API key at [alphavantage.co/support/#api-key](https://www.alphavantage.co/support/#api-key)
2. Set the environment variable:
   ```bash
   export ALPHA_VANTAGE_API_KEY=your_api_key_here
   ```

## Rate Limits

Free tier allows approximately **25 API calls per day** and **5 per minute**. Each plugin's orchestrator warns before executing multi-step analyses that consume multiple calls.

| Analysis Type | Approximate Calls |
|---------------|-------------------|
| Single price check | 1-2 |
| Standard analysis | 3-5 |
| Deep analysis | 6-10 |
| Comprehensive report | 10-15 |

## Free Tier Limitations

These premium-only endpoints are **not used** by any plugin:

- `TIME_SERIES_INTRADAY` (real-time intraday data)
- `TIME_SERIES_DAILY_ADJUSTED` (adjusted daily data)
- `REALTIME_BULK_QUOTES` (batch real-time quotes)
- `REALTIME_OPTIONS` / `HISTORICAL_OPTIONS` (options data)
- `VWAP` (volume-weighted average price)
- `FX_INTRADAY` (intraday forex)
- `DIGITAL_CURRENCY_INTRADAY` (intraday crypto)

## Language Support

All plugins respond in the user's language automatically.

## License

MIT
