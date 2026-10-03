SPY Quantitative Momentum Trading Strategy

This repository contains a Python-based quantitative trading and backtesting framework focused on the S&P 500 ETF (SPY). It demonstrates the step-by-step evolution of a momentum-based trading model, starting from a basic baseline and advancing to a hedge-fund-level strategy with dynamic risk parity and trend filtering.

Project Structure

The project is divided into three Jupyter Notebooks, each representing a progressively sophisticated version of the trading strategy:

1. 01_data_and_factor.ipynb - Baseline Momentum Strategy

Data Fetching: Automatically downloads the last 2 years of daily SPY data using yfinance (bypassing local CSVs for the freshest data).

Factor Calculation: Computes a 20-day momentum factor.

Data Cleaning: Implements a Median Absolute Deviation (MAD) filter to winsorize extreme outliers.

Signal Generation: A simple binary strategy—buy and hold when the 20-day momentum is > 0, otherwise stay flat.

Robustness: Strictly implements T+1 execution (shift(1)) to avoid look-ahead bias.

2. 02_data_and_factor.ipynb - Upgraded Trend & Buffer Strategy

Regime Filter: Integrates a 50-day Simple Moving Average (SMA50). The strategy only takes long positions when the SPY closes above its SMA50, avoiding bear market traps.

Buffer Zone: Introduces a ±2% momentum buffer to reduce trading friction and whipsawing in choppy markets. Buy signals trigger above +2%, and sell signals trigger below -2%, maintaining the previous day's position in between.

3. 03_data_and_factor.ipynb - Advanced Risk Parity & Dynamic Sizing

Continuous Sizing: Moves away from binary (0 or 1) signals to dynamic position sizing.

Momentum Strength (Conviction): Maps the momentum factor (0% to 2.5%) linearly into a 0% to 100% base position weight.

Risk Parity (Volatility Targeting): Calculates 20-day annualized volatility. Scales positions based on a target annualized volatility of 16% (capped at 1.0x leverage).

Final Synthesis: Multiplies the conviction weight by the risk-parity weight during confirmed uptrends, resulting in a smooth, risk-adjusted equity curve.

Generated Artifacts

spy_momentum_2y.csv: An auto-generated dataset containing the raw historical prices, calculated momentum factors, and MAD-filtered signals outputted by the initial scripts.

Key Performance Metrics

Each notebook automatically backtests the strategy against a standard "SPY Buy & Hold" benchmark and calculates three core institutional metrics:

Annualized Return: Compound annual growth rate based on 252 trading days.

Sharpe Ratio: Risk-adjusted return assuming a 2% risk-free rate.

Maximum Drawdown: The largest peak-to-trough drop in the strategy's equity curve.

Interactive matplotlib charts are generated at the end of each notebook to visualize the Cumulative Return of the Strategy vs. the Benchmark.

Requirements

To run the notebooks locally, you will need Python 3.x and the following packages installed:

pip install pandas numpy matplotlib yfinance


Usage

Clone the repository and open the Jupyter Notebooks. Run the cells sequentially to fetch live data, calculate the factors, and view the backtest results and performance charts.

git clone https://github.com/yourusername/spy-momentum-quant.git
cd spy-momentum-quant
jupyter notebook


Disclaimer

This project is for educational and research purposes only. It does not constitute financial or investment advice. Live market trading involves significant risk.
