# FiveTickers — From 500 Stocks to 5: Clustering & Forecasting

A financial time-series analytics project that analyzes historical prices for 500 equities, discovers patterns in stock movement, selects five representative tickers, and compares ARIMA and LSTM forecasting models.

This project connects pattern discovery, representative stock selection, forecasting, and error-aware evaluation into one practical workflow for financial analytics.

## Objective

The objective of this project was to:

- Analyze historical price movement across 500 stocks
- Discover groups of stocks with similar price behavior
- Select five diverse representative tickers from distinct clusters
- Forecast future price trends using time-series models
- Compare ARIMA and LSTM using RMSE and visual trend alignment

## Project Overview

The workflow starts with a broad universe of 500 equities and narrows the analysis to five representative stocks using hierarchical clustering.

    Scope: 500 equities
    Raw frequency: Minute-level prices
    Resampled frequency: Hourly prices
    Pattern discovery: Correlation matrix + hierarchical clustering
    Selected tickers: 5 representative stocks
    Forecasting models: ARIMA and LSTM
    Evaluation metric: RMSE

The five selected analysis tickers were:

    SP500
    NASDAQ.AAL
    NASDAQ.ADI
    NASDAQ.AKAM
    NASDAQ.DISH

These tickers were chosen to represent distinct clusters, making the deep-dive forecasting analysis more diverse and less redundant.

## Problem Statement

Financial time-series data is noisy, high-dimensional, and difficult to analyze directly across hundreds of securities.

Modeling every stock individually can be inefficient and hard to interpret. This project addressed that challenge by first using clustering to identify similar stock-movement patterns, then selecting representative tickers for deeper forecasting.

This approach supports better exploratory analysis, diversification thinking, and risk-aware decision support.

## Repository Structure

    Fivetickers-Time-series-modeling/
    ├── Stock analysis and forecasting.ipynb
    ├── stock_clusters.csv
    ├── ANALYSIS and FORECASTING of STOCKS.pdf
    └── README.md

## Data and Preprocessing

The project used minute-level historical price data and transformed it into a cleaner time-series format for clustering and forecasting.

### Datetime Conversion

Unix timestamps were converted into datetime objects so the price data could be indexed and analyzed as a proper time series.

### Resampling

Minute-level prices were resampled into hourly average prices.

This reduced short-term noise while retaining useful trend and variability information.

    Minute-level prices → Hourly mean prices

### Missing Data Handling

Columns with missing values were dropped to ensure cleaner clustering and model-training inputs.

### Selected Analysis Tickers

The final five tickers used for deeper analysis were:

- SP500
- NASDAQ.AAL
- NASDAQ.ADI
- NASDAQ.AKAM
- NASDAQ.DISH

These were selected from distinct clusters to represent different price-movement patterns.

## Pattern Discovery: From 500 Stocks to 5

### Correlation Matrix

A pairwise correlation matrix was computed across the hourly stock series to measure similarity in stock movement.

This helped identify which stocks behaved similarly over time.

### Hierarchical Clustering

Hierarchical clustering was applied using the correlation structure to group stocks with similar price dynamics.

A dendrogram was used to visualize relationships across the 500-stock universe.

### Cluster Selection

Five clusters were selected for practical visibility and diversity.

Stocks within the same cluster likely share common movement drivers, such as:

- Macro market factors
- Sector-level trends
- Shared volatility patterns
- Similar price-response behavior

Selecting one representative ticker from each cluster helped avoid redundant analysis and created a more diverse forecasting set.

## Forecasting Setup

The project compared two forecasting approaches:

- LSTM for neural sequence modeling
- ARIMA for statistical rolling forecast modeling

Both models used an 80/20 temporal split, where the first 80% of observations were used for training and the final 20% were used for testing. The time order was preserved to avoid look-ahead bias.

## LSTM Forecasting

The LSTM model was implemented as a univariate sequence model for each selected ticker.

### LSTM Setup

- Scaling: Min-Max normalization to `[0, 1]`
- Temporal split: First 80% training, final 20% testing
- Windowing: 12 hourly steps used to predict the 13th hour
- Input shape: `(samples, time_steps=12, features=1)`
- Evaluation: Forecasts compared against the true test series

### LSTM Purpose

LSTM was used to test whether a deep-learning sequence model could learn temporal dependencies from recent hourly price windows.

## ARIMA Forecasting

ARIMA was implemented using a rolling or walk-forward forecasting strategy.

### ARIMA Setup

- Temporal split: First 80% training, final 20% testing
- Rolling forecast loop:
  - Append the actual price at time `t` to the training history
  - Refit ARIMA on the expanded history
  - Forecast the next step, `t+1`
  - Repeat across the full test horizon

### ARIMA Purpose

ARIMA was used as a statistical time-series baseline with one-step-ahead rolling predictions.

The rolling setup allowed the model to continuously update with the newest observed price, producing tighter short-horizon forecasts.

## Evaluation Metric: RMSE

RMSE was used to compare model error magnitude.

### Why RMSE?

- **Magnitude-aware:** Penalizes large errors more heavily, which matters when large forecast misses can be costly.
- **Comparable across series:** Provides an error value in the same scale as the target variable.
- **Interpretable:** Since RMSE is in price units, it is easier to explain to stakeholders.
- **Useful for model comparison:** Helps compare ARIMA and LSTM forecast accuracy across the selected tickers.

## Key Findings

- ARIMA achieved lower RMSE than LSTM across all five selected stocks.
- Both models captured directional trends and the general shape of price movement.
- ARIMA’s one-step rolling strategy produced tighter alignment with actual prices.
- LSTM provided a useful deep-learning baseline but did not outperform ARIMA in this hourly forecasting setup.
- Lower RMSE suggests fewer large forecast misses, which can help reduce costly trading or risk-management errors.
- Trend capture can still be useful for signal timing, market monitoring, and risk overlays.

## Deliverables

### Exploratory Assets

- Dendrogram of 500 stocks
- Correlation heatmap
- Cluster-based stock grouping

### Selection Plots

- Hourly price charts for the five selected tickers
- Visual comparison of representative stock behavior

### Modeling Artifacts

LSTM artifacts:

- Scaled time-series windows
- Train/test split setup
- Forecast plots
- Actual vs predicted comparisons

ARIMA artifacts:

- Rolling forecasts
- One-step-ahead prediction loop
- Residual diagnostics
- Forecast plots

### Evaluation Summary

- RMSE table per ticker and model
- Visual comparisons of ARIMA vs LSTM forecasts
- Model interpretation based on forecast accuracy and trend fit

## Business and Analytics Interpretation

This workflow can support exploratory financial analytics by helping analysts:

- Identify groups of stocks with similar behavior
- Avoid redundant analysis across highly correlated securities
- Select representative tickers for deeper modeling
- Compare statistical and deep-learning forecasting approaches
- Evaluate forecast models using both error magnitude and trend alignment
- Support diversification and risk-management thinking through cluster-based grouping

## Tech Stack

**Language:** Python  
**Environment:** Jupyter Notebook  
**Libraries:** pandas, NumPy, scikit-learn, SciPy, statsmodels, TensorFlow/Keras, Matplotlib, Seaborn  
**Methods:** Time-series preprocessing, correlation analysis, hierarchical clustering, ARIMA forecasting, LSTM forecasting, RMSE evaluation

## How to Run

Clone the repository:

    git clone <your-repo-link>
    cd Fivetickers-Time-series-modeling

Install dependencies:

    pip install pandas numpy scikit-learn scipy statsmodels tensorflow matplotlib seaborn jupyter

Launch Jupyter Notebook:

    jupyter notebook

Open and run:

    Stock analysis and forecasting.ipynb

## What I Focused On

- Transforming minute-level stock data into hourly time series
- Discovering stock-movement patterns across 500 equities
- Using hierarchical clustering to select representative tickers
- Building ARIMA and LSTM forecasting workflows
- Preserving temporal order during train/test splitting
- Comparing models using RMSE and visual trend alignment
- Connecting model results to business and risk-management interpretation

## Highlights

- Analyzed time-series data across 500 equities
- Resampled minute-level price data into hourly series for cleaner modeling
- Built a correlation-based clustering workflow to reduce 500 stocks to 5 representative tickers
- Applied hierarchical clustering and dendrogram analysis for pattern discovery
- Compared ARIMA rolling forecasts with LSTM sequence modeling
- Used RMSE and visual forecast plots to evaluate model performance
- Found that ARIMA produced lower RMSE than LSTM across all five selected stocks
- Demonstrated financial analytics, time-series modeling, unsupervised learning, and deep-learning experimentation

## Disclaimer

This project is for educational and analytical purposes only. It is not financial advice and should not be used as an investment recommendation or trading strategy.

## Future Improvements

- Add final RMSE comparison table directly to the README
- Add forecast plots for each selected ticker
- Add dendrogram and correlation heatmap images
- Add walk-forward validation details
- Add additional models such as SARIMA, Prophet, or XGBoost
- Add financial evaluation metrics beyond RMSE
- Add a short explanation of how each representative ticker was selected from its cluster
- Add `requirements.txt` for easier reproducibility

## Final Summary

From 500 stocks, hierarchical clustering identified five representative tickers for detailed modeling.

In hourly time-series forecasts, ARIMA delivered consistently lower RMSE than LSTM while both models preserved trend direction and general movement shape. The workflow connects pattern mining, representative selection, forecasting, and error-aware evaluation into a practical financial analytics pipeline.
