# 📈 SPY Quant Momentum Trading & Risk Parity Backtest Framework

[![Python](https://img.shields.io/badge/Python-3.9+-blue.svg)](https://www.python.org/)
[![Libraries](https://img.shields.io/badge/yfinance%20|%20pandas%20|%20numpy-brightgreen.svg)]()
[![Quant](https://img.shields.io/badge/Quant%20Finance-Risk%20Parity-orange.svg)]()

This project is dedicated to building a progressive, institutional-grade quantitative trading and backtesting framework for the S&P 500 ETF (SPY). Starting from a simple momentum factor, the framework evolves into a complex dynamic position-sizing model. By decoupling "signal generation," "trend filtering," and "volatility management," we successfully tackled common algorithmic trading traps such as look-ahead bias and whipsaw friction.

---

## 💡 Market Background & Core Pain Points

When designing mid-to-low frequency algorithmic trading strategies, quantitative researchers face several critical challenges:
- **Look-Ahead Bias (Future Leakage):** A fatal flaw in amateur backtests. Calculating today's momentum using today's closing price and executing the trade *today* is physically impossible in live markets. 
- **Market Noise & Whipsaws:** Simple binary momentum signals trigger massive false positives during sideways, ranging markets, leading to high transaction costs and "death by a thousand cuts."
- **Extreme Value Distortions:** Sudden market anomalies (e.g., flash crashes) severely distort mathematical indicators. Standard moving averages and momentum factors are highly vulnerable to these outliers.
- **Fixed Position Risks:** Binary "All-in / All-out" strategies suffer massive drawdowns during sudden regime shifts. Ignoring real-time asset volatility leads to out-of-control portfolio variance.

---

## 🏗️ Algorithm Architecture: Three-Tier Evolution Approach

To combat these market challenges, the project discards a monolithic script approach in favor of a **progressive three-phase architecture**, upgrading from a baseline signal to a hedge-fund-level risk control model:

### 📍 Phase 1 (V0): Factor Generation & Baseline Backtest (The Foundation)
We built the foundational data pipeline and basic momentum logic (`01_data_and_factor.ipynb`).
- Extracted real-time daily OHLCV data using `yfinance`, with an automated fix for multi-level header inconsistencies.
- **MAD Winsorization:** Innovatively applied the Median Absolute Deviation (MAD) algorithm to robustly filter out extreme price outliers before factor calculation.
- **Strict T+1 Execution:** Implemented a mandatory `shift(1)` on all generated signals, ensuring tomorrow's position is strictly based on today's closing data, structurally eliminating look-ahead bias.

### 📍 Phase 2 (V1): Dual-Filter & Hysteresis Mechanism (The Whipsaw Killer)
Reduced trade friction by introducing structural macroeconomic filters (`02_data_and_factor.ipynb`).
- Introduced a 50-day Simple Moving Average (SMA50) as a strict macro trend filter (only allowing longs above the SMA50).
- Pioneered a **±2% Momentum Buffer Zone**: A long signal is only triggered when the 20-day momentum exceeds +2%, and a sell is only triggered when it drops below -2%. This emergency buffer completely neutralizes redundant trading in choppy markets.

### 📍 Phase 3 (V2): Risk Parity Optimizer (Dynamic Sizing Model)
To address the massive drawdowns of binary positioning, we developed a continuous sizing model based on volatility metrics (`03_data_and_factor.ipynb`). We designed a composite synthetic signal merging two custom multipliers:
1. **Risk Parity Multiplier:** Calculates real-time 20-day annualized volatility. It targets a strict 16% volatility cap—scaling down exposure dynamically as market panic/volatility spikes.
2. **Confidence Multiplier (Momentum Strength):** Linearly maps the raw momentum (0~2.5%) to a continuous base weight (0~100%), preventing abrupt portfolio allocations.
3. **Synthetic Integration:** `Final Position = Valid Trend Boolean * Confidence Weight * Risk Weight`. 

---

## 📊 Core Optimization Achievements

Extensive backtesting was conducted over a 2-year rolling window, extracting standard quantitative performance metrics:

- 🚀 **Extreme Return Optimization (V1 Strategy)**: By implementing the dual-filter (SMA50 + 2% Buffer), the V1 optimizer successfully filtered out inefficient scattered signals. It skyrocketed the annualized return to **42.21%** with an astonishing Sharpe Ratio of **3.90**, completely crushing the SPY Buy & Hold benchmark.
- 🛡️ **Outstanding Drawdown Control (V2 Strategy)**: Under the strict Risk Parity and Confidence framework, the V2 model significantly suppressed portfolio volatility. Even in turbulent market environments, it kept the max drawdown strictly capped at **-5.38%**, demonstrating true institutional-grade robustness and capital preservation.

---

## 📂 Project File Structure

```text
.
├── 01_data_and_factor.ipynb     # V0: Data fetching, MAD outlier filter & baseline momentum backtest
├── 02_data_and_factor.ipynb     # V1: SMA50 trend filter & ±2% Buffer Zone strategy optimization
├── 03_data_and_factor.ipynb     # V2: Institutional dynamic risk-parity & confidence sizing model
└── spy_momentum_2y.csv          # Local cache: Extracted baseline momentum and factor data
