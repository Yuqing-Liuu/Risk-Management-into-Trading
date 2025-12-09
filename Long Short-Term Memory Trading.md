## 1. Why LSTM Can Be Used for Trading

Financial time series such as log-returns exhibit several stylized facts:

- Non-stationarity  
- Volatility clustering  
- Regime switching  
- Non-linear temporal dependencies  
- Weak serial dependence mixed with noise  

Traditional linear models (ARIMA, OLS) assume short memory and linear dynamics, which limits their ability to extract predictive structure from asset returns.

LSTMs (Long Short-Term Memory networks) are specifically designed to learn:

1. Long-range temporal dependencies  
2. Non-linear patterns  
3. Hidden regimes  
4. Complex transformations of past returns  

Because markets contain weak but persistent patterns, LSTMs can convert historical return sequences into a probabilistic forecast of the next-period direction.  
This makes LSTMs suitable for **directional trading strategies**, where the goal is not to predict the exact return value but to capture:

- trend continuation  
- mean-reversion signals  
- volatility-regime transitions  


## 2. Mathematical Logic of the LSTM Model

Assume we observe a rolling window of past log-returns:

r_(t-L+1), r_(t-L+2), ..., r_t

Let x_t denote the input at time t.  
The LSTM cell updates its internal states using the following gating mechanism.


### 2.1 LSTM Equations (pure text, no LaTeX)

Forget gate:
f_t = sigmoid( W_f * [h_(t-1), x_t] + b_f )

Input gate:
i_t = sigmoid( W_i * [h_(t-1), x_t] + b_i )

Candidate memory:
g_t = tanh( W_g * [h_(t-1), x_t] + b_g )

Output gate:
o_t = sigmoid( W_o * [h_(t-1), x_t] + b_o )

Cell state update:
c_t = f_t ⊙ c_(t-1) + i_t ⊙ g_t

Hidden state:
h_t = o_t ⊙ tanh(c_t)

Here:
- x_t : input (log-return at time t)
- h_t : hidden representation (extracted temporal features)
- c_t : long-term memory
- ⊙ : element-wise multiplication


### 2.2 Mapping the LSTM Output to Trading Labels

Instead of predicting the raw return r_(t+1), the model predicts one of three classes:

- +1  (Buy)
-  0  (Flat)
- -1  (Sell)

Let y_(t+1) be the true class label.

The LSTM hidden state h_t is passed to a fully-connected layer:

z = W_out * h_t + b_out

Then a softmax function converts it into class probabilities:

p = softmax(z)

Where:
p(+1), p(0), p(-1) sum to 1.


### 2.3 Label Generation (3-class classification)

Let r_(t+1) be the next-day log-return.

A threshold τ > 0 is applied:

If r_(t+1) > τ       → label = +1 (Buy)
If r_(t+1) < -τ      → label = -1 (Sell)
Otherwise            → label = 0  (Flat)

τ can be defined by:
- empirical quantiles (e.g. 33% and 67%)
- volatility scaling (τ = k * std)


## 3. How LSTM Becomes a Trading Strategy

### 3.1 Predictive Output

At time t, the model outputs:

p(+1 | history)
p(0  | history)
p(-1 | history)

The trading signal is:

signal_t = argmax_class p(class)


### 3.2 Position Execution

Positions are opened at t and applied to next-day return r_(t+1):

position_t ∈ { -1, 0, +1 }

PnL_(t+1) = position_t * r_(t+1)


### 3.3 Transaction Costs

Let cost be the proportional trading cost (e.g. 0.1%).

Position change:
Δ_t = | position_t - position_(t-1) |

Then net PnL is:

NetPnL_(t+1) = position_t * r_(t+1) - cost * Δ_t


### 3.4 Performance Evaluation

From the PnL series, we compute:

- cumulative return
- annualized return (CAGR)
- annualized volatility
- Sharpe ratio
- max drawdown
- hit ratio (accuracy of directional prediction)

The strategy is then compared across five asset classes:

- Treasury bond (IEF)
- Commodity (CLF)
- Gold (GCF)
- Currency (EURUSD)
- Technology (XLK)

This allows us to evaluate how the same LSTM architecture performs under different market dynamics and volatility structures.

