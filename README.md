# Markowitz Portfolio Optimization with ML-Driven Expected Returns

Constructing a diversified 11-asset portfolio that maximizes the Sharpe ratio, using machine-learning-based return forecasts as inputs to Markowitz optimization, validated against the S&P 500.

## Overview

The objective of this project is to build a portfolio across five asset classes - equities, bond ETFs, REIT ETFs, commodities, and cryptocurrencies, and show that diversification in the portfolio reduces overall risk. The key tweak is that rather than using historical means as the expected returns, the project uses machine learning models tailored to each asset class to generate forward-looking return forecasts.

The pipeline has three stages:

1. **Forward-looking expected returns** are generated per asset using machine learning, rather than a naive historical mean.
2. **Portfolio weights** are optimized to maximize the Sharpe ratio under realistic constraints, using an exact quadratic programming solver, with a Monte Carlo simulation as an independent cross-check.
3. **Out-of-sample backtesting** measures realized performance on unseen 2025 data against an equal-weight benchmark and the S&P 500.

## Key Results (Out-of-Sample, Jan–Oct 2025)

| Strategy | Ann. Return | Ann. Volatility | Sharpe | Max Drawdown |
|---|---|---|---|---|
| **Max Sharpe** | 19.25% | 10.87% | **1.40** | -5.6% |
| **Min Volatility** | 11.74% | 5.38% | **1.43** | -1.7% |
| Equal Weight (1/N) | 16.60% | 16.30% | 0.77 | -11.2% |
| S&P 500 (SPY) | 14.58% | 21.58% | 0.49 | -17.2% |

- The **Maximum Sharpe portfolio beat the S&P 500 on every metric**. Higher returns, roughly half the volatility, nearly 3x the Sharpe ratio, and a far shallower drawdown.
- The optimized portfolio **outperformed the naive equal-weight benchmark**, confirming the optimization added value beyond simple diversification.
- The model's **forecast was well-calibrated** as the Max Sharpe portfolio's expected annualized return (18.17%) closely matched its realized return (19.25%) on data the models never saw.

## Methodology

### 1. Expected Returns via Machine Learning

Assets are modeled by category according to their volatility and behavior:

| Category | Assets | Model | Rationale |
|---|---|---|---|
| Cryptocurrencies | BTC, ETH | Random Forest Regressor | Captures nonlinear, high-volatility patterns |
| Equities / REITs / Commodities | SPY, QQQ, IWM, VNQ, GLD, USO | Linear Regression | Captures relatively stable, linear trends |
| Bonds | LQD, IEF, HYG | Historical Mean | Low, consistent returns; ML doesn't add anything |

Each model uses the past 20 daily returns as features to predict a **5-day smoothed forward return** (averaging the next 5 days denoises the the very-hard-to-predict single-day return). Data is split chronologically (no shuffling) into 80% train / 20% test to prevent look-ahead leakage. The model's directional signal is then used to tilt a stable historical-mean anchor, producing forward-looking expected returns.

### 2. Portfolio Optimization

The optimal weights are found by **exact quadratic programming** (SLSQP), maximizing the Sharpe ratio subject to:

- **Long-only:** no short selling (weights ≥ 0)
- **Concentration cap:** no asset exceeds 30% of the portfolio (enforces diversification)
- **Fully invested:** weights sum to 1

A **Monte Carlo simulation** of 10,000 random portfolios (Dirichlet-sampled under the same 30% cap) maps the feasible region. The best portfolio found by the simulation is plotted, along with the Capital Market Line through its tangency point, and is then validated against the exact solver — confirming the random search and the exact optimum converge.

### 3. Backtesting

Models are trained on 2018–2025 data and the resulting portfolio is backtested on **out-of-sample** data (Jan–Oct 2025), reporting annualized return, volatility, Sharpe ratio, and maximum drawdown against an equal-weight portfolio and the S&P 500.

## Tech Stack

Python · NumPy · pandas · scikit-learn · SciPy · yfinance · Matplotlib

## Repository

```
├── MPT.ipynb        # full analysis notebook
└── README.md
```

Run the notebook top to bottom; it downloads all data via `yfinance`.

## License

This project is licensed under the MIT License — see the LICENSE file for details.

