# Strategy Summary – LSTM-Based Trading

<img width="1287" height="384" alt="image" src="https://github.com/user-attachments/assets/969c1360-bef5-4d76-b1fb-4798070f34ab" />


Overall Assessment

- The strategy demonstrates profitability potential in certain assets (CLF, GCF, EURUSD).
  
- However, risk control is insufficient, as seen in high volatility and extreme drawdowns (especially CLF).
  
- Predictive accuracy (Hit Ratio ~45–51%) is only slightly above random chance.
  
- Low turnover suggests limited trading frequency, which helps reduce transaction costs.
  
**Conclusion**: Strategy 1 highlights the predictive capacity of LSTM models but suffers from poor risk-adjusted performance. This makes the case for introducing Strategy 2 with risk management mechanisms to achieve more stable and practical trading outcomes.



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


<img width="2064" height="1183" alt="image" src="https://github.com/user-attachments/assets/d155bf4c-139b-48c4-82d1-1a0d4cbdabfe" />


<img width="1195" height="391" alt="image" src="https://github.com/user-attachments/assets/9e36952d-a553-4380-8f6b-276a2c7996e0" />

<img width="2004" height="1163" alt="image" src="https://github.com/user-attachments/assets/d4dd3270-c272-4181-a719-a45d5257989d" />

<img width="1398" height="1148" alt="image" src="https://github.com/user-attachments/assets/a478141c-0895-4b6a-b732-b9268ac7f90d" />







