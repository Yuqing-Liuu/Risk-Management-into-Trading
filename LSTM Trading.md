## 1. Why LSTM Can Be Used for Trading

Financial time series such as log-returns exhibit several stylized facts:

- Non-stationarity and structural breaks  
- Volatility clustering  
- Regime switching  
- Non-linear temporal dependencies  
- Weak serial dependence mixed with strong noise  

Long Short-Term Memory (LSTM) networks are specifically designed to:

1. Capture long-range temporal dependencies via the memory cell  
2. Model non-linear interactions between past returns  
3. Adapt to different volatility and trend regimes through the gating mechanism  
4. Compress raw sequences into a low-dimensional hidden state that summarizes the recent market history  

Because financial markets may contain weak but persistent patterns (trend, mean-reversion, regime shifts), LSTMs can transform a window of past log-returns into a directional forecast for the next period.  
This forecast can then be converted into trading actions (buy / sell / flat), turning the LSTM into a systematic trading strategy.


## 2. LSTM Cell: Mathematical Formulation

Consider a window of past log-returns


$$
\{r_{t-L+1}, r_{t-L+2}, \dots, r_t\},
$$


where $\(L\)$ is the lookback length. At each step $\(k\)$ in the window we feed an input vector $\(x_k\)$ to the LSTM.

For an LSTM cell, the updates at time step $\(k\)$ are:


## 2.1 LSTM Equations

Forget gate:

$$
f_k = \sigma\big( W_f [h_{k-1}, x_k] + b_f \big)
$$

Input gate:

$$
i_k = \sigma\big( W_i [h_{k-1}, x_k] + b_i \big)
$$

Candidate memory:

$$
\tilde{c}_k = \tanh\big( W_c [h_{k-1}, x_k] + b_c \big)
$$

Output gate:

$$
o_k = \sigma\big( W_o [h_{k-1}, x_k] + b_o \big)
$$

Cell state update:

$$
c_k = f_k \odot c_{k-1} + i_k \odot \tilde{c}_k
$$

Hidden state:

$$
h_k = o_k \odot \tanh(c_k)
$$



## 3. From LSTM Output to Trading Signal (3-Class Classification)

Instead of directly predicting the next log-return $\(r_{t+1}\)$, we formulate a 3-class classification problem:

- $y_{t+1} = +1$ : Buy (long)
  
- $y_{t+1} = 0$ : Flat (no position)
  
- $y_{t+1} = -1$ : Sell (short)

### 3.1 Label Construction

Let $\(r_{t+1}\)$ be the next-day log-return.  
We define two thresholds $\(\tau_{\text{low}}$ < 0 < $\tau_{\text{high}}\)$.  
A simple symmetric choice uses a single $\(\tau > 0\)$ such that:

$$
y_{t+1} =
\begin{cases}
+1, & \text{if } r_{t+1} > \tau, \\
0, & \text{if } -\tau \le r_{t+1} \le \tau, \\
-1, & \text{if } r_{t+1} < -\tau.
\end{cases}
$$

Typical ways to choose $\(\tau\)$:

- **Quantile-based**: $\(\tau\)$ chosen so that roughly 1/3 of observations are Buy, 1/3 Sell, 1/3 Flat.  
- **Volatility-based**: $\(\tau = k \cdot \sigma_r\)$, where $\(\sigma_r\)$ is the standard deviation of daily log-returns and $\(k > 0\)$.

This transforms numerical returns into discrete trading decisions, which is more directly aligned with a real trading system.


### 3.2 Classification Head on Top of LSTM

After the LSTM processes the lookback window and produces $\(h_t\)$, we map it to class scores:

$$
z_t = W_{\text{out}} h_t + b_{\text{out}} \in \mathbb{R}^3,
$$

where the three components correspond to the logits for $\(-1, 0, +1\)$.

We apply the softmax function to obtain class probabilities:

$$
p_t^{(k)} = \frac{\exp(z_t^{(k)})}
{\sum_{j \in \{-1,0,+1\}} \exp(z_t^{(j)})},
\quad k \in \{-1,0,+1\}.
$$

The predicted label is:

$$
\hat{y}_{t+1} = \arg\max_{k \in \{-1,0,+1\}} p_t^{(k)}.
$$

Training is done by minimizing the cross-entropy loss between the predicted probabilities $\(p_t^{(k)}\)$ and the true labels $\(y_{t+1}\)$.


## 4. Turning LSTM Predictions into a Trading Strategy

### 4.1 Trading Signal and Position

We interpret the predicted class $\(\hat{y}_{t+1}\)$ as a trading signal:

- **Long:** $\hat{y}_{t+1} = +1$
  
- **Flat:** $\hat{y}_{t+1} = 0$
  
- **Short:** $\hat{y}_{t+1} = -1$



Let $\(s_t \in \{-1, 0, +1\}\)$ denote the position held during $\([t, t+1]\)$.  
At the decision time $\(t\)$ we set:

$$
s_t = \hat{y}_{t+1}.
$$

The realized next-day PnL (ignoring costs) is:

$$
\text{PnL}_{t+1} = s_t \cdot r_{t+1}.
$$


### 4.2 Transaction Costs

Transaction costs are **not included** in this study.  


### 4.3 Performance Metrics

From the time series of $\text{NetPnL}_{t}$ we construct:

- Cumulative return  
- Annualized return 
- Annualized volatility  
- Sharpe ratio  
- Maximum drawdown  
- Hit ratio (percentage of days with correctly predicted direction)


