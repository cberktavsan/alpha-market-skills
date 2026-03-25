# Equity Endpoints Reference

## Free Tier Endpoints

### GLOBAL_QUOTE
- **Parameters**: `symbol` (required)
- **Returns**: price, open, high, low, volume, latest trading day, previous close, change, change percent
- **Note**: Data may be delayed 15+ minutes on free tier

### TIME_SERIES_DAILY
- **Parameters**: `symbol` (required), `outputsize` (optional: compact=100 points, full=20+ years)
- **Returns**: Daily OHLCV (Open, High, Low, Close, Volume)
- **Note**: `compact` is default, `full` requires more quota

### TIME_SERIES_WEEKLY / TIME_SERIES_WEEKLY_ADJUSTED
- **Parameters**: `symbol` (required)
- **Returns**: Weekly OHLCV, adjusted version includes dividends and split coefficients

### TIME_SERIES_MONTHLY / TIME_SERIES_MONTHLY_ADJUSTED
- **Parameters**: `symbol` (required)
- **Returns**: Monthly OHLCV, adjusted version includes dividends and split coefficients

### SYMBOL_SEARCH
- **Parameters**: `keywords` (required)
- **Returns**: Best matching symbols with name, type, region, market open/close times, currency

### MARKET_STATUS
- **Parameters**: None
- **Returns**: Current trading status for all major global markets

## Premium Endpoints

These endpoints require a premium API key. The assistant may attempt them; if they fail, it notes the requirement and continues.

- `TIME_SERIES_INTRADAY` — real-time/15-min delayed intraday data
- `TIME_SERIES_DAILY_ADJUSTED` — daily data adjusted for splits and dividends
- `REALTIME_BULK_QUOTES` — batch quotes for up to 100 symbols
- `GLOBAL_QUOTE` with `entitlement=realtime` — real-time quote data
- `REALTIME_OPTIONS` / `HISTORICAL_OPTIONS` — options chain data
