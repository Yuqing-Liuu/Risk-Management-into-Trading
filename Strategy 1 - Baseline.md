# Strategy Summary – LSTM-Based Trading

Compare LSTM-based trading signals against a Buy-and-Hold benchmark.

---

## 2. Data & Returns
Training period: 2014–2022  
Testing period: 2022–2024  

We compute log returns as:

$$ r_t = \log(P_t) - \log(P_{t-1}) $$

Future return for horizon \(H\):

$$ r_{t+H} = \log(P_{t+H}) - \log(P_t) $$

---

## 3. Labeling (Buy / Hold / Sell)

- Buy (+1) if future return > epsilon  
- Sell (-1) if future return < -epsilon  
- Hold (0) otherwise  

With epsilon proportional to rolling volatility:

$$ y_t =
\begin{cases}
+1 & \text{if } r_{t+H} > \epsilon \\
-1 & \text{if } r_{t+H} < -\epsilon \\
0 & \text{otherwise}
\end{cases} $$

---

## 4. Rolling Window LSTM

To address non-stationarity, we adopt a **rolling / walk-forward LSTM**:

- Train window: 1000 days  
- Retrain every 60 days  
- Forecast horizon: 1 day  

Process:

1. Train LSTM on most recent 1000 days
2. Predict next 60 days
3. Slide window forward
4. Repeat until end of test set


---

## 5. Trading Rule

Let the predicted directional signal be:

$$ \hat{y}_t \in \{-1, 0, +1\} $$

Position:

$$ s_t =
\begin{cases}
+1 & \text{if } \hat{y}_t = +1 \\
-1 & \text{if } \hat{y}_t = -1 \\
0 & \text{if } \hat{y}_t = 0
\end{cases} $$

Strategy PnL:

$$ PnL_t = s_t \cdot r_t $$

Total return:

$$ TotalReturn = \left( \prod_t (1 + PnL_t) \right) - 1 $$

(No transaction costs included.)

---

## 6. Buy-and-Hold Benchmark

$$ BuyHold = \frac{P_{end}}{P_{start}} - 1 $$

---

<img width="2938" height="266" alt="image" src="https://github.com/user-attachments/assets/cdc92e4e-6fc4-450c-8912-9297ad557cb8" />
<img width="2110" height="1161" alt="image" src="https://github.com/user-attachments/assets/c42b9091-ac1b-452e-9cce-48c81a5e17b3" />

<img width="3014" height="1017" alt="image" src="https://github.com/user-attachments/assets/95a9c10b-83bc-4e88-a672-52be2494bd1d" />
<img width="3007" height="1033" alt="image" src="https://github.com/user-attachments/assets/e7afc0fc-f5e4-4d21-90a0-4d28dcb71cc1" />
<img width="2996" height="483" alt="image" src="https://github.com/user-attachments/assets/ce7e6a14-3baa-4da2-bf99-4b449e7f53b5" />






