# Trader Performance vs Market Sentiment Analysis

## 📌 Project Overview
This analysis investigates how **market sentiment** (measured by the Crypto Fear & Greed Index) affects **trader performance** using execution-level trade data from the Hyperliquid exchange.  
The primary objective is to **uncover hidden patterns in trader behavior** and determine whether trading outcomes differ on days classified as *Fear*, *Neutral*, or *Greed*.

---

## 📂 Data Sources

1. **Hyperliquid Trader Data**  
   - Columns include: `account`, `symbol`, `execution_price`, `size`, `side`, `time`, `start_position`, `event`, `closedPnL`, `leverage`, etc.  
   - Each row = a single trade execution.

2. **Crypto Fear & Greed Index**  
   - Columns: `date`, `classification` (Extreme Fear / Fear / Neutral / Greed / Extreme Greed).  
   - Classification derived from a score (0–100).

---

## ⚙️ Data Preparation

* **Datetime alignment:** Converted trade timestamps to dates (`YYYY-MM-DD`) to match sentiment records.  
* **Merge:** Combined datasets on the date, adding a `classification` column to each trade.  
* **Feature Engineering:**  
  - `is_profitable`: `True` if `ClosedPnL > 0`.  
  - `norm_pnl`: Normalized profit = `ClosedPnL / trade_size` → percent return per trade.  
  - `fee_pct`: Trading fee as a % of trade size.  
  - `classification_grouped`: Collapsed sentiment into three broad groups:
    - Fear (Extreme Fear + Fear)
    - Neutral
    - Greed (Greed + Extreme Greed)
  - Per-account **cumulative PnL** and **drawdown** calculated for risk assessment.

---

## 📊 Exploratory Data Analysis (EDA)

### 1. Sentiment Distribution
* Trades are not evenly distributed:  
  ➤ **Greed days:** ~90K trades  
  ➤ **Fear days:** ~83K trades  
  ➤ **Neutral days:** ~38K trades  
* Market activity is higher during bullish (Greed) periods.

---

### 2. Profitability vs. Sentiment

| Sentiment | Win Rate (%) | Avg. Normalized Return |
|-----------|-------------|------------------------|
| Greed     | ~42.0%      | ~2.87%                |
| Fear      | ~40.8%      | ~1.26%                |
| Neutral   | ~39.7%      | ~0.99%                |

* Traders **win more often and earn higher returns on Greed days**.
* **Extreme Conditions:**
  - Extreme Greed → ~46.5% profitable trades (highest).
  - Extreme Fear → ~37.1% profitable trades (lowest).

---

### 3. Return Distributions

* Median `norm_pnl` = **0.0** → most trades barely break even.
* Skewed distribution: a few large wins/losses dominate averages.
* Example:
  - Best trade ≈ **+340%**
  - Worst trade ≈ **–38,400%** (likely very high leverage).

---

### 4. Fee Analysis

* Mean `fee_pct` ≈ **0.035%**  
* Median ≈ **0.025%**  
* Fees have negligible effect on trade outcomes.

---

### 5. Cumulative PnL & Drawdowns

* By computing each account’s equity curve:
  - Drawdowns show how far a trader’s performance falls from peak.
  - Useful for identifying “risky” accounts that lose heavily during fearful markets.

---

## ✅ Key Insights

* **Greed correlates with better performance:**  
  Traders win more and earn higher returns in bullish markets.
* **Fear increases difficulty:**  
  On fearful days, fewer trades succeed, and returns are lower.
* **Most trades are small impact:**  
  The median trade result is near zero, but a small number of extreme trades create volatility.
* **Fees don’t meaningfully affect profitability.**
* **Risk varies by sentiment:**  
  Drawdowns tend to be deeper during Fear periods.

---

## 🔍 Extended Analysis – Modeling (Optional)

A basic classification model predicts `is_profitable` using trade features + sentiment:

| Model           | Accuracy | F1 Score |
|-----------------|---------:|---------:|
| **Random Forest** | **99.4%** | **99.4%** |
| XGBoost         | 97.6%   | 97.6%   |
| MLP Neural Net  | 30.3%   | 15.3%   |

* **Best model:** Random Forest → saved for potential deployment.
* Feature importance indicates **sentiment**, **trade size**, and **account behavior** contribute to predictability.

---

## 🛠 Tools Used

* Python (Pandas, NumPy, Matplotlib, Seaborn)
* Scikit-learn, XGBoost
* Jupyter Notebook

---

### 📁 Data & Model Files

⚠️ **Note:**  
- The original raw datasets are **not stored in this repository** due to their large size.  
- You must manually download them using the provided Google Drive links in the assignment description.  
- Merged CSV files and the trained Random Forest model are also not uploaded here for size reasons, but the included Jupyter Notebook will reproduce them once data is placed in the correct directory.


