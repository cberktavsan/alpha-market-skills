---
name: news-sentiment
description: Retrieves market news with AI sentiment scores, earnings transcripts, insider transactions, and institutional holdings.
metadata:
  version: "1.0.0"
---

# News & Sentiment Analysis

The assistant accesses market news, sentiment data, earnings transcripts, and institutional activity from Alpha Vantage.

> **Global rules (language, rate limits, free-tier constraints, output handling) are defined in this plugin's orchestrator and apply here.**

## Available Tools

| Tool | Purpose |
|------|---------|
| `NEWS_SENTIMENT` | Live and historical news articles with AI-generated sentiment scores |
| `EARNINGS_CALL_TRANSCRIPT` | Full earnings call transcripts with AI sentiment signals |
| `INSIDER_TRANSACTIONS` | Recent insider buys/sells for a company |
| `INSTITUTIONAL_HOLDINGS` | Major institutional holders and their positions |
| `ANALYTICS_FIXED_WINDOW` | Advanced analytics over a fixed time window |
| `ANALYTICS_SLIDING_WINDOW` | Advanced analytics over a sliding time window |

## NEWS_SENTIMENT Parameters

- `tickers`: Comma-separated ticker symbols (e.g., `AAPL,MSFT`)
- `topics`: Filter by topic — `technology`, `finance`, `economy`, `manufacturing`, `energy_transportation`, `real_estate`, `blockchain`, `ipo`, `mergers_and_acquisitions`, `earnings`, `retail_wholesale`, `life_sciences`, `financial_markets`
- `time_from` / `time_to`: Date range in `YYYYMMDDTHHMM` format
- `sort`: `LATEST`, `EARLIEST`, `RELEVANCE`
- `limit`: Number of results (default 50)

## Workflows

### Stock News & Sentiment

1. The assistant calls `NEWS_SENTIMENT` with the ticker symbol
2. For each article, extracts: title, source, date, sentiment score, sentiment label
3. Calculates overall sentiment: averages the ticker-specific sentiment scores
4. Summarizes: how many bullish vs bearish vs neutral articles
5. Highlights the most impactful headline

### Topic-Based News Scan

1. The assistant calls `NEWS_SENTIMENT` with a `topics` filter (no specific ticker)
2. Presents top 5-7 most relevant articles
3. Groups by sentiment: positive, negative, neutral
4. Identifies emerging themes

### Earnings Call Analysis

1. The assistant calls `EARNINGS_CALL_TRANSCRIPT` with `symbol` and `quarter` (e.g., `2024Q3`)
2. Summarizes key points: CEO/CFO main messages, revenue/earnings highlights, forward guidance, analyst concerns
3. Notes any AI-flagged sentiment signals

### Insider Activity Check

1. The assistant calls `INSIDER_TRANSACTIONS` for the ticker
2. Presents recent transactions: insider name, title, transaction type, shares, price, date
3. Calculates net insider buying/selling over recent period
4. Interprets: cluster of insider buys = potential bullish signal; sells are less informative (may be routine)

### Institutional Holdings Review

1. The assistant calls `INSTITUTIONAL_HOLDINGS` for the ticker
2. Shows top holders: institution name, shares held, percentage of outstanding, change
3. Identifies if major institutions are increasing or decreasing positions

## Sentiment Score Interpretation

- **Bullish**: Score > 0.35
- **Somewhat Bullish**: 0.15 to 0.35
- **Neutral**: -0.15 to 0.15
- **Somewhat Bearish**: -0.35 to -0.15
- **Bearish**: Score < -0.35

## Gotchas

- **NEWS_SENTIMENT returns up to 200 articles**: The assistant presents only the top 5-7 most relevant. Summarizes the rest as aggregate statistics (N bullish, N bearish, N neutral).
- **Ticker-specific vs overall sentiment**: Each article has an `overall_sentiment_score` AND per-ticker `ticker_sentiment`. The assistant always uses the ticker-specific score when analyzing a specific stock.
- **EARNINGS_CALL_TRANSCRIPT requires exact quarter format**: Use `YYYYQN` format (e.g., `2024Q3`). The assistant calculates the correct quarter string from user context.
- **INSIDER_TRANSACTIONS may be empty**: Not all companies have recent insider activity. The assistant states "no recent insider activity reported" rather than showing an empty table.
- **INSTITUTIONAL_HOLDINGS data lag**: Holdings are reported quarterly with a 45-day delay (SEC 13F filing). The data may be 1-4 months old. The assistant always states the reporting date.
- **ANALYTICS tools are advanced**: `ANALYTICS_FIXED_WINDOW` and `ANALYTICS_SLIDING_WINDOW` are used only when the user specifically requests windowed analytics, not for basic news queries.
- **Sentiment score precision**: Scores are floats between -1.0 and 1.0. Small differences (e.g., 0.18 vs 0.22) are both "somewhat bullish" — the assistant does not over-interpret.

## Output Format

- The assistant leads with the overall sentiment assessment
- Lists key headlines with their sentiment labels
- Uses clean tables for insider/institutional data
- Notes the time period covered
- Distinguishes between news sentiment (opinion) and insider/institutional activity (action)

## Token Budget

Target: keep skill output under 500 tokens. For NEWS_SENTIMENT, a 2-3 sentence sentiment summary followed by a table of 5 key headlines. For EARNINGS_CALL_TRANSCRIPT, a summary of 200 words maximum.
