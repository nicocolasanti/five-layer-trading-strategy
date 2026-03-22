[README (1).md](https://github.com/user-attachments/files/26167932/README.1.md)
# five-layer-trading-strategy

An algorithmic trading bot I built on QuantConnect using Python. It runs five coordinated layers — regime detection, sector rotation, an ML signal, covered calls, and a crash hedge — all working together as one system.

Backtested January 2010 through January 2024. Currently live paper trading as of March 21, 2026.

---

## Backtest Results (14 Years)

| Metric | Result |
|---|---|
| Total Return | 351.90% |
| Annualized Return | 11.4% |
| Sharpe Ratio | 0.572 |
| Max Drawdown | 26.2% |
| Starting Capital | $100,000 |
| Final Value | $451,900 |
| Overfitting Parameters | 0 |

The 14-year window covers multiple full market cycles — the 2010s bull run, the 2018 correction, the 2020 COVID crash, and the 2022 bear market. The bot had to survive all of them.

---

## How It Works

**Layer 1 — Regime Detection**  
Checks whether SPY and QQQ are above or below their 200-day EMAs. If we're in a bear regime, the whole strategy scales back. This runs weekly and acts as the master filter for everything else.

**Layer 2 — Sector Rotation**  
Every month, it ranks all 11 SPDR sector ETFs by 6-month momentum and rotates into the top 3. Simple idea, but it makes a real difference in which part of the market you're actually in.

**Layer 3 — ML Signal**  
An XGBoost classifier trained on 12 features, retrained every month on rolling data. It doesn't make buy/sell decisions on its own — it scales position sizes between 0.65x and 1.0x depending on its confidence. Keeps the human logic in charge, lets the model fine-tune sizing.

**Layer 4 — Covered Calls**  
Sells weekly out-of-the-money calls on SPY and QQQ to collect premium on top of whatever the equity positions are doing. Skips this if VIX is too high — no point selling cheap options into a volatile market.

**Layer 5 — Black Swan Protection**  
Sits dormant most of the time. Only fires when the composite signal drops below a certain threshold, which historically lines up with major crash events. Didn't want to over-engineer this one — it just needs to work when things get bad.

---

## Stack

- Python 3
- QuantConnect LEAN Engine
- XGBoost
- QuantConnect equity + options data

---

## What's In This Repo

This repo contains a structural walkthrough of the system (`algorithm_overview.py`) and this README. The full implementation is proprietary and lives on QuantConnect.

---

## About

Built by N Colasanti. I build algorithmic trading systems and quantitative finance tools in Python. Available for freelance work.

---

*© 2026 N Colasanti. All rights reserved.*
