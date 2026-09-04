# DVOL-Adaptive: A Volatility-Sentiment Driven Bitcoin Trading & Signal System

Independent research project analyzing Bitcoin price behavior through the lens of the **Deribit Volatility Index (DVOL)**, combining a sentiment-based trading signal with dynamic risk management to backtest and optimize a systematic BTC trading strategy plus a **live daily prediction tracker** that generates and verifies signals in real time.

---

## Project Overview

This project explores whether volatility (via DVOL) can be used as a leading sentiment indicator for Bitcoin price direction, and whether a rules-based trading strategy built around it can be made consistently profitable through careful risk management.

The work is split into two components:

1. **Backtesting & Optimization** : a systematic simulation of a BTC trading strategy over 1,000+ days, refined through parameter optimization and intraday (hourly) resolution.
2. **Live Signal Tracker** : a daily-updating Google Sheet that generates a Bullish/Bearish/Neutral signal from live DVOL movement and automatically verifies it against next-day price action.

---

## Methodology

### Data Sources
- **Deribit Volatility Index (DVOL)** : daily BTC volatility index, used as the core sentiment driver
- **Yahoo Finance (BTC-USD)** : daily and hourly OHLC price data

### Signal Logic
- If today's DVOL is higher than yesterday's → **Bearish**
- If today's DVOL is lower than yesterday's → **Bullish**
- Signal is checked against next day Open-to-Close price movement to generate a RIGHT/WRONG verdict

### Strategy Development Process
1. Built an initial fixed-parameter simulation (5x leverage, 10% stop-loss, 15% profit cap) as a baseline
2. Introduced a **DVOL-based dynamic leverage advisor** leverage is reduced during extreme volatility periods (90th percentile DVOL, ~61.23)
3. Ran single-parameter optimization sweeps on leverage, stop-loss, and profit cap independently
4. Integrated **hourly intraday data** to accurately resolve whether a stop-loss or profit cap was hit first within a trading day
5. Performed an extensive **grid search** across base leverage (1x–15x), stop-loss (1–15%), and profit cap (1–20%) combined with dynamic leverage, to identify the optimal configuration
6. Tested an advanced trailing stop-loss variant (found to underperform documented as a negative result)

---

## Key Results

| Stage | Total Net Profit (simulated) |
|---|---|
| Initial fixed-parameter strategy | ~$6,737 |
| Profit cap increased to 20% | ~$9,100 |
| Optimal leverage (7x) identified | ~$9,632 |
| **Final: Dynamic leverage + grid-searched SL/TP + hourly resolution** | **~$11,817** |

**Optimal configuration found:**
- Base leverage: 10x (dynamically halved to 5x during extreme volatility)
- Stop-loss: 14%
- Profit cap: 20%

**Dynamic vs. fixed leverage comparison** (same SL/TP parameters):
- Fixed 10x leverage → ~$9,065 net profit
- Dynamic leverage (10x base, halved on high-DVOL days) → ~$11,817 net profit (+~30%)
- Win rate held constant at 50.80% in both cases — the improvement came entirely from **risk sizing**, not signal accuracy

**Sentiment signal accuracy:**
- Overall historical win rate: 53.2% (a modest edge over random chance)
- Most recent 100-day rolling win rate: 64.0%

*All profit figures are from historical backtesting/simulation and do not reflect live trading, transaction costs, or slippage.*

---

## Visualizations

**Leverage Optimization** : total net profit peaks around 7x leverage before declining sharply as amplified volatility triggers more stoplosses
![Leverage Optimization](charts/leverage_optimization.png)

**Stop-Loss Optimization** : profit stabilizes once stoploss exceeds roughly 14–20%, showing tight stoplosses were often detrimental in this volatile market
![Stop-Loss Optimization](charts/stoploss_optimization.png)

**Grid Search Surface (13x Leverage)** : profitability across combinations of stop-loss and profit booking percentages
![Grid Search Surface](charts/grid_search_surface.png)

**Rolling Win Rate** : 100 day rolling win rate trending above the 50% random-chance baseline, averaging 55.5% over the tracked period
![Rolling Win Rate](charts/rolling_winrate.png)

**Cumulative PnL** : cumulative profit/loss of the final strategy over 1,000 simulated days
![Cumulative PnL](charts/cumulative_pnl.png)

---

## Live Prediction Tracker

A companion Google Sheet tracks the DVOL-based signal on a live, ongoing basis:
- Daily DVOL, sentiment signal, and next day verdict (RIGHT/WRONG), logged continuously
- A live BTC price panel with % change across 1-minute to 1 month timeframes
- **[View the live tracker →](https://docs.google.com/spreadsheets/d/1fhh5p2BYuWqmFZRDN-BzKgBdybvOanuSEt65wqDGUls/edit?gid=0)**

---

## Tools & Stack

- **Python** (Google Colab) : data processing, simulation, and backtesting logic
- **Excel / Google Sheets** : data tracking, live signal verification, and output storage
- **Data sources:** Deribit (DVOL), Yahoo Finance (BTC-USD OHLC)

---

## Repository Structure

```
dvol-adaptive-btc-trading-strategy/
├── README.md
├── requirements.txt
├── DVOL_BTC_Strategy.ipynb          # Full backtesting & optimization notebook
├── data/
│   ├── BTC_Tracker_1000d.xlsx
│   ├── BTC_DVOL_Leverage_Advisor.xlsx
│   └── BTC_Hourly_Volatility_Matrix.xlsx
├── charts/
│   ├── leverage_optimization.png
│   ├── stoploss_optimization.png
│   ├── grid_search_surface.png
│   ├── rolling_winrate.png
│   └── cumulative_pnl.png
└── report/
    └── research_report.pdf          # Full write-up of methodology & findings
```

---

## Future Work

- Refine trailing stoploss logic (current implementation underperformed)
- Explore additional volatility/sentiment indicators, including ML based signal generation
- Incorporate realistic transaction costs and slippage
- Extend to a multi asset crypto portfolio
- Robustness testing across different market regimes and out of sample periods

---

