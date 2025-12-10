# Risk-Management-into-Trading

The purpose of this thesis is to examine how risk‐control mechanisms affect the performance, stability, and economic viability of LSTM-based trading strategies, and how these controlled strategies can be further integrated into a portfolio-level asset allocation framework.

While LSTM models can capture nonlinear temporal dependencies in financial time series, their raw predictions often lead to unstable returns, excessive drawdowns, and poor robustness. Therefore, this thesis first evaluates whether applying systematic risk controls — including probability thresholds, volatility scaling, position filtering, maximum drawdown limits, rolling-window retraining, and turnover constraints — can materially improve the risk-adjusted performance of LSTM strategies.

After establishing the impact of risk controls at the single-asset level, the thesis extends the analysis to a multi-asset context. The goal is to investigate whether LSTM strategies (with risk controls) provide diversification benefits, improve portfolio efficiency, or alter optimal asset allocation weights compared with traditional benchmarks such as equal-weight, volatility-weighted, or Markowitz mean–variance portfolios.

Overall, this thesis aims to determine:
1) whether LSTM trading strategies become viable only after risk-management overlays are applied,   
2) how controlled LSTM strategies can be incorporated into portfolio allocation to further enhance risk-adjusted returns.
















