# Free vs Premium Endpoints Reference

## Free Tier Endpoints (Use These)

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

## Premium Endpoints (DO NOT Use)

- `TIME_SERIES_INTRADAY` — requires premium for real-time/15-min delayed data
- `TIME_SERIES_DAILY_ADJUSTED` — premium endpoint
- `REALTIME_BULK_QUOTES` — premium, up to 100 symbols per request
- `GLOBAL_QUOTE` with `entitlement=realtime` — premium real-time data
