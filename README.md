# markowitz_portfolio_optimization
The objective of this project is to construct a diversified portfolio comprising 11 assets and demonstrate how diversification reduces overall portfolio volatility. The analysis further aims to determine the optimal asset weights that yield the minimum volatility portfolio and the maximum Sharpe ratio portfolio. 
The portfolio contains 11 assets from multiple classes to better capture and diversify the movements of the market. The assets include stocks, bonds-ETF, REIT-ETF, commoddities and cryptos. 

The assets are divided into three categories based on their volatility and behavior. Cryptocurrencies, being highly volatile and exhibiting nonlinear patterns, are modeled using a Random Forest Regressor to better capture complex relationships. Equities, REITs, and commodities are relatively more stable and follow linear trends, so a Linear Regression model is used for their return prediction. Bonds display low volatility and consistent returns; therefore, their expected returns are estimated using the simple average of historical returns rather than a machine learning model.

Features for both the Linear Regression and Random Forest Regressor models are created using time series data to predict the expected returns and covariance based on 7 years worth of historical data, where each feature set consists of the past 20 daily returns to predict the next (nth) return. A lag of 20 is chosen to capture short-term momentum and mean-reversion behavior in asset returns. The daily return is converted to annual return by compounding over 252 trading days.

After estimating the expected returns and covariance matrix for all assets, a Monte Carlo simulation is conducted to generate a large number of random portfolios. This allows plotting of the Expected Return vs. Standard Deviation curve, representing the Markowitz Efficient Frontier.
Two distinct optimal portfolios are identified from the simulation: the Minimum Variance Portfolio and the Maximum Sharpe Ratio Portfolio :

1) The Minimum Variance Portfolio achieves an expected annual return of 8.02% with a standard deviation of 5.35%, resulting in a Sharpe ratio of 0.74.

2) The Maximum Sharpe Ratio Portfolio, on the other hand, delivers a higher expected annual return of 21.37% with a standard deviation of 15.29%, yielding a Sharpe ratio of 1.13.

where the risk free rate is taken to be 4.02%. These results highlight the trade-off between risk and return in portfolio optimization. While the minimum variance portfolio prioritizes stability and lower volatility, the maximum Sharpe ratio portfolio optimizes the risk-adjusted return, making it more suitable for investors with higher risk tolerance.

After calculating the two optimal portfolios, backtesting was conducted over the subsequent nine-month period using historical market data to evaluate their real-world performance. The Minimum Volatility Portfolio achieved an annualized return of 20.47%, while the Maximum Sharpe Ratio Portfolio achieved an annualized return of 27.17%. These results validate the robustness of the portfolio construction approach, demonstrating that both the optimized portfolios outperformed the average market performance, with the Maximum Sharpe Ratio Portfolio providing superior risk-adjusted returns in the test period.
