# Strategy Summary – LSTM-Based Trading

<img width="1274" height="378" alt="image" src="https://github.com/user-attachments/assets/801e34b3-a21d-4233-9dc9-6d582ef08da5" />



---


## 2. Labeling (Buy / Hold / Sell)

With epsilon proportional to rolling volatility:

$$ y_t =
\begin{cases}
+1 & \text{if } r_{t+H} > \epsilon \\
-1 & \text{if } r_{t+H} < -\epsilon \\
0 & \text{otherwise}
\end{cases} $$

---

## 3. Rolling Window LSTM

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

## 4. Trading Rule

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

## 5. Buy-and-Hold Benchmark

$$ BuyHold = \frac{P_{end}}{P_{start}} - 1 $$

---

<img width="2035" height="1174" alt="image" src="https://github.com/user-attachments/assets/564d1e7a-4ce5-4263-b1f0-b5f64399c820" />

<img width="2407" height="975" alt="image" src="https://github.com/user-attachments/assets/15c4bd84-8edd-4703-8857-764d10c31411" />

<img width="2446" height="802" alt="image" src="https://github.com/user-attachments/assets/f4b30f8c-cb17-4769-b119-1873ffa0cca7" />

<img width="1389" height="290" alt="image" src="https://github.com/user-attachments/assets/f8b3bea0-c5c1-4b68-8e0e-7b959870d230" />












