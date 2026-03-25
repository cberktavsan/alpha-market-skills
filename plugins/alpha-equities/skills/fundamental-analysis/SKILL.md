---
name: fundamental-analysis
description: Analyzes company financials, valuation, earnings, and IPO data via Alpha Vantage fundamental endpoints.
metadata:
  version: "1.0.0"
---

# Fundamental Analysis

The assistant provides in-depth fundamental analysis of publicly traded companies using Alpha Vantage financial data endpoints.

> **Global rules (language, rate limits, API limits, output handling) are defined in this plugin's orchestrator and apply here.**

## Available Tools

| Tool | Purpose |
|------|---------|
| `COMPANY_OVERVIEW` | Company profile, sector, market cap, P/E, EPS, dividends, 52-week range, analyst target |
| `INCOME_STATEMENT` | Annual and quarterly revenue, net income, operating income, EBITDA, EPS |
| `BALANCE_SHEET` | Annual and quarterly assets, liabilities, equity, cash, debt |
| `CASH_FLOW` | Annual and quarterly operating/investing/financing cash flows, free cash flow, capex |
| `EARNINGS` | Historical quarterly and annual EPS (actual vs estimated) |
| `LISTING_STATUS` | Active and delisted tickers with IPO date, exchange info |
| `EARNINGS_CALENDAR` | Upcoming earnings dates for the next 3-12 months |
| `IPO_CALENDAR` | Upcoming IPO dates, expected price ranges, share volumes |

## Workflows

### Company Deep Dive

1. The assistant calls `COMPANY_OVERVIEW` for the ticker
2. Presents key metrics in a structured summary:
   - **Identity**: Name, sector, industry, exchange, country
   - **Valuation**: Market cap, P/E (trailing & forward), PEG ratio, price-to-book, price-to-sales
   - **Profitability**: EPS, revenue TTM, profit margin, operating margin, return on equity/assets
   - **Dividends**: Dividend yield, payout ratio, ex-dividend date
   - **Analyst**: Target price, number of analysts
3. Provides a brief qualitative assessment based on the numbers

### Financial Statement Analysis

1. The assistant calls `INCOME_STATEMENT`, `BALANCE_SHEET`, and `CASH_FLOW` for the ticker
2. For each statement, compares the last 3 years (annual) or 4 quarters:
   - **Income**: Revenue growth, margin trends, EPS trajectory
   - **Balance Sheet**: Debt-to-equity trend, current ratio, cash position
   - **Cash Flow**: Free cash flow trend, capex intensity, operating cash flow vs net income
3. Highlights red flags: declining margins, rising debt, negative FCF, cash burn

### Earnings Analysis

1. The assistant calls `EARNINGS` for historical EPS data
2. Shows beat/miss history (actual vs estimated EPS)
3. Calculates beat rate and average surprise percentage
4. Calls `EARNINGS_CALENDAR` if the user asks about upcoming earnings

### Quick Valuation Check

1. The assistant calls `COMPANY_OVERVIEW` for valuation multiples
2. Presents: P/E, forward P/E, PEG, P/B, P/S, EV/EBITDA
3. Compares to sector averages if the user provides context
4. Flags if any metric looks notably high or low

### IPO Calendar

1. The assistant calls `IPO_CALENDAR`
2. Presents upcoming IPOs with: company name, symbol, expected date, price range, shares offered
3. Calculates expected market cap at midpoint of price range

## Gotchas

- **COMPANY_OVERVIEW returns empty for small-caps and non-US tickers**: Many fields will be "None" or "0". The assistant states which fields are unavailable rather than presenting zeros as real data.
- **Annual vs quarterly arrays**: INCOME_STATEMENT/BALANCE_SHEET/CASH_FLOW responses contain both `annualReports` and `quarterlyReports`. The assistant parses the correct one based on user intent. Default: annual for trend analysis, quarterly for recent performance.
- **Fiscal year varies by company**: Apple's fiscal year ends in September, not December. The assistant always notes the fiscal period end date.
- **Currency mismatch**: Non-US companies report in their local currency. The assistant notes the reporting currency explicitly.
- **reportedEPS vs estimatedEPS**: A "miss" is when reported < estimated, not when reported < previous quarter. The assistant makes this distinction clear.
- **Stale COMPANY_OVERVIEW**: Some fields (analyst target, number of analysts) may be weeks old. The assistant notes data freshness.
- **LISTING_STATUS returns CSV**: Unlike other endpoints, this returns CSV format. The assistant parses accordingly.

## Analysis Guidelines

- The assistant always presents trends over time, not just snapshots
- Flags inconsistencies between statements (e.g., rising net income but declining operating cash flow)
- Calculates margins explicitly rather than relying on pre-computed values
- Notes the currency and fiscal year end date

## Output Format

Financial statement responses are large (dozens of line items per period). The assistant extracts only the key metrics listed in the workflows above, not every line item. Presents as summary tables, not raw JSON.

## Token Budget

Target: keep skill output under 600 tokens. For financial statements, the assistant extracts only the last 3 annual periods or 4 quarterly periods.
