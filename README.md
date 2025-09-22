# FiveTickers — From 500 Stocks to 5: Clustering & Forecasting *(LSTM vs ARIMA)*

**Objective:** Analyze historical prices for **500 stocks**, discover **patterns in price movements**, select **five representative stocks** for deep dive, and **forecast** future trends with time-series models.

---

## 🔎 Overview

- **Scope:** 500 equities (minute-level prices → resampled hourly)
- **Focus:** Pattern discovery via **hierarchical clustering** → select **5 diverse tickers**  
- **Models:** **ARIMA** (rolling forecast) vs **LSTM** (sequence modeling)
- **Metric:** **RMSE** for error magnitude; visual trend match

---

## 📦 Data & Preprocessing

- **Datetime conversion:** Converted *Unix timestamps* to **datetime** objects.
- **Resampling:** Aggregated **minute → hourly** (mean price per hour) to reduce noise while retaining trend and variability.
- **Missing data:** Dropped columns with **NaN** values.
- **Five analysis tickers:** `SP500`, `NASDAQ.AAL`, `NASDAQ.ADI`, `NASDAQ.AKAM`, `NASDAQ.DISH`  
  *Chosen to represent distinct clusters.*

---

## 🧭 Pattern Discovery (500 → 5)

- **Correlation matrix:** Computed **pairwise correlations** of hourly returns/prices across all 500 stocks.
- **Hierarchical clustering:** Built a **dendrogram** from the correlation matrix to group stocks by **similar price dynamics**.
- **Cluster selection:** Chose **5 clusters** (practical visibility, diversity).  
  Stocks within a cluster share likely **drivers** (macro factors, sector trends).  
  This supports **diversification** and **risk management** by avoiding redundancy.

---

## 🔮 Forecasting Setup

### 📈 LSTM (Univariate)

- **Scaling:** **Min–Max** normalization to `[0, 1]`.
- **Temporal split:** First **80%** for **training**, final **20%** for **testing** *(order preserved)*.
- **Windowing:** **Lookback = 12** hourly steps → predict the **13th** hour.
- **Input shape:** `(samples, time_steps=12, features=1)`.
- **Evaluation:** Predict on test horizon; compare with true series and **plot** forecasts.

### 📉 ARIMA (Rolling / Walk-Forward)

- **Temporal split:** First **80%** train, **20%** test *(order preserved)*.
- **Rolling forecast loop:**
  1. Append the **actual** price at time *t* to the training set.
  2. **Refit** ARIMA on the expanded history.
  3. **Forecast** the next step *(t+1)*.
  4. Repeat across the full test horizon.
- **Evaluation:** One-step-ahead rolling predictions; **plot** vs actuals.

---

## 📏 Metric & Rationale — **RMSE**

- **Magnitude-aware:** Penalizes **large errors** more heavily—critical when mispricing can be costly.
- **Comparable across series:** Naturally scaled to the **units** of the target; supports comparison across differently scaled tickers.
- **Interpretability:** Same units as price; easy to communicate model accuracy to stakeholders.

---

## ✅ Key Findings

- **ARIMA** achieved **lower RMSE** than **LSTM** **across all five stocks**, indicating closer point forecasts.
- **Both** models captured **directional trends** (up/down) and the **general shape** of movements;  
  however, ARIMA’s one-step rolling strategy produced **tighter alignment** with actuals.
- **Business implication:** Lower RMSE suggests **fewer large forecast misses**, which can help reduce costly trading errors;  
  trend capture is useful for **signal timing** and **risk overlays**.

---

## 📊 Deliverables

- **Exploratory assets:** Dendrogram of 500 stocks, correlation heatmap.
- **Selection plots:** Hourly price charts for the **five** chosen tickers.
- **Modeling artifacts:**  
  - LSTM: scaled windows, train/test split diagrams, forecast plots.  
  - ARIMA: rolling forecasts, residual diagnostics, forecast plots.
- **Evaluation summary:** RMSE table per ticker & model + visual comparisons.

---

## 📜 Summary

From **500 stocks**, hierarchical clustering identified **5 representative** tickers for detailed modeling.  
In **hourly** time-series forecasts, **ARIMA** delivered **consistently lower RMSE** than **LSTM** while both preserved **trend direction**.  
The workflow connects **pattern mining** → **representative selection** → **forecasting** → **error-aware evaluation** suitable for practical decision support.
