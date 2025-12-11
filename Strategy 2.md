# Strategy 2 — Risk-Control + Dynamic Leverage Overlay 

Strategy 2 (S2) is a risk-managed enhancement applied on Strategy 1

While S1 generates directional signals (long/short/flat), S2 adjusts position size using:

- Volatility targeting  
- Dynamic leverage scaling  
- Drawdown-based risk reduction  
- Minimum exposure floor  


---

Trading signals often suffer from:

- Large swings in volatility  
- Deep drawdowns  
- Inconsistent exposure  
- Excessive risk concentration  

S2 follows institutional overlay logic (CTA, risk-parity, vol targeting) by controlling exposure instead of altering the directional model.

---

##  Volatility Targeting Component

The objective is to scale exposure so that annualized volatility stays around the target:

$$ \sigma_{\text{target}} = 30\% $$

Rolling realized volatility (window \( L = 60 \)):

$$ \sigma_t = \text{Std}(r_{t-L:t}) \times \sqrt{252} $$

Raw leverage:

$$ k^{raw}_t = \frac{\sigma_{\text{target}}}{\sigma_t} $$

Bounded leverage:

$$ k^{raw}_t = \min(\max(k^{raw}_t, k_{\min}), k_{\max}) $$

with  
- \( k_{\max} = 10 \)  
- \( k_{\min} = 0.10 \)

---

## Drawdown-Based Leverage Adjustment

Let equity curve be \(E_t\) and running peak:

$$ P_t = \max_{i \le t} E_i $$

Drawdown:

$$ DD_t = \frac{E_t}{P_t} - 1 $$

Two DD triggers:

- Soft drawdown:  \(-15\%\)  
- Hard drawdown:  \(-30\%\)

Adjusted leverage:

\[
k_t =
\begin{cases}
0.2 \cdot k^{raw}_t, & DD_t \le -30\% \\
0.5 \cdot k^{raw}_t, & -30\% < DD_t \le -15\% \\
k^{raw}_t, & \text{otherwise}
\end{cases}
\]

This mimics real institutional risk controls.

---

## Final Position and PnL Construction

S2 uses S1’s directional signal:

$$ s^{S1}_t \in \{-1, 0, +1\} $$

Scaled exposure:

$$ \text{Position}^{S2}_t = k_t \cdot s^{S1}_t $$

Daily log return:

$$ r^{S2}_t = k_t \cdot s^{S1}_t \cdot r_t $$

Equity curve:

$$ E_t = \exp\left(\sum_{i=1}^t r^{S2}_i\right) $$

---


<img width="783" height="414" alt="image" src="https://github.com/user-attachments/assets/606d091c-07ce-48fe-ae88-77bb6852f516" />


<img width="2453" height="998" alt="image" src="https://github.com/user-attachments/assets/3f6926b1-dcbe-4cf4-bf1e-228e4d21877e" />

<img width="2639" height="1182" alt="image" src="https://github.com/user-attachments/assets/38b35f90-f5c8-4069-a304-f6c1f5bdc52d" />


<img width="1190" height="240" alt="image" src="https://github.com/user-attachments/assets/8ecdb922-4c0c-4831-b47d-b9cebb714a4c" />

<img width="1190" height="240" alt="image" src="https://github.com/user-attachments/assets/dea3be9b-b95a-41b3-a402-ead14c3db0d7" />

<img width="1190" height="240" alt="image" src="https://github.com/user-attachments/assets/dea1c7c8-a11c-472d-a7fa-970cd43dbbbd" />

<img width="1190" height="240" alt="image" src="https://github.com/user-attachments/assets/08b33b28-1480-4ee0-924e-facad4867862" />

<img width="1190" height="240" alt="image" src="https://github.com/user-attachments/assets/c6b9caf9-f681-40e5-a40d-eea2b1933eb3" />


<img width="1409" height="458" alt="image" src="https://github.com/user-attachments/assets/f706e9e2-12b3-4948-89d4-a85019cb2fce" />

<img width="1388" height="792" alt="image" src="https://github.com/user-attachments/assets/b740aba6-a7e6-4751-ab2b-98690e8cf234" />




