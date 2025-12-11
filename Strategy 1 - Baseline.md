# Strategy Summary – LSTM-Based Trading

<img width="1325" height="375" alt="image" src="https://github.com/user-attachments/assets/d90d8cd0-4073-4121-9053-10c067a84dce" />


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


<img width="2121" height="1196" alt="image" src="https://github.com/user-attachments/assets/e03fe7a5-17ad-4e00-b7ac-ec780beb77b7" />



<img width="1200" height="386" alt="image" src="https://github.com/user-attachments/assets/5c46f052-59d0-4e60-b1ae-c12f7eb1c120" />


<img width="2022" height="1183" alt="image" src="https://github.com/user-attachments/assets/e015a2c4-bb02-4170-b609-fc69ecb1579c" />


<img width="1398" height="1148" alt="image" src="https://github.com/user-attachments/assets/828ba393-6c1b-476a-a603-e8e9a87c61f7" />








