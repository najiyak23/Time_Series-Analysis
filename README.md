
# AAPL Stock Returns: Time Series Analysis and ARIMA Forecasting

Explore the complete analysis, including data preparation, stationarity testing, ARIMA model selection, backtesting, forecasting, and residual diagnostics.

📓 [View the ARIMA AAPL Stock Forecasting Notebook](https://github.com/najiyak23/Time_Series-Analysis/blob/main/ARIMA_AAPL_Stock_Forecasting.ipynb)


## Project Overview

This project analyses the weekly stock prices and log returns of Apple Inc. (AAPL) from June 2009 to June 2019 using time series analysis techniques in Python.

The objective is to investigate the statistical properties of weekly stock returns, assess stationarity, select an appropriate ARIMA model, and evaluate its forecasting performance using chronological backtesting. The project also examines residual diagnostics and model-based prediction intervals to understand the limitations of forecasting financial returns.

Rather than focusing only on producing forecasts, this analysis evaluates whether the selected model provides meaningful predictive improvement over simple baseline methods.

## Objectives

- Explore historical weekly stock prices and log returns.
- Test the stationarity of weekly log returns using the Augmented Dickey–Fuller (ADF) test.
- Select an ARIMA model using information criteria, including AIC and BIC.
- Evaluate out-of-sample forecasting performance using fixed-origin and rolling one-step-ahead backtesting.
- Compare ARIMA forecasts against historical-mean and zero-return baselines.
- Examine residual autocorrelation, volatility clustering, and distributional characteristics.
- Generate ten-week price forecasts with model-based prediction intervals.

## Dataset

The dataset contains weekly AAPL stock prices, using the closing price from the last trading day of each week.

| Attribute | Details |
|---|---|
| Stock | Apple Inc. (AAPL) |
| Period | June 2009 – June 2019 |
| Total observations | 522 weekly observations |
| Training observations | 417 |
| Test observations | 105 |
| Main variable | Weekly closing price |
| Derived variable | Weekly log returns |

The data was divided chronologically to ensure that future observations were not used during model selection or training.

## Methodology

### 1. Data Preparation and Exploratory Analysis

The weekly price series was prepared for time series analysis. Log returns were calculated to study relative price changes rather than modelling the price level directly.


Log returns were calculated using the following formula:

`r_t = ln(P_t / P_(t-1))`

where:
- `r_t` is the log return for week `t`.
- `P_t` is the closing price for week `t`.
- `P_(t-1)` is the closing price for the previous week.


Exploratory analysis was used to examine price movements, return fluctuations, and potential patterns over time.

### 2. Stationarity Testing

The Augmented Dickey–Fuller (ADF) test was applied to weekly log returns.

The test produced a statistic of approximately **-24.72**, with a p-value close to zero, providing strong evidence against the null hypothesis of a unit root.

Based on this result, the return series was modelled without differencing, using \(d = 0\).

### 3. ARIMA Model Selection

ARIMA models were evaluated using information criteria, with model selection performed on the training data only.

The Bayesian Information Criterion (BIC) selected:

**ARIMA(0,0,0) with a constant**

This model represents a constant expected return plus random errors, without autoregressive or moving-average terms.

The estimated mean weekly log return was approximately **0.0048**, or 0.48% in log-return terms. The estimated error variance was approximately **0.0014**.

Although AIC slightly favoured a more complex model, BIC favoured the simpler specification.

### 4. Out-of-Sample Backtesting

Two backtesting approaches were used:

- **Fixed-origin backtesting:** Model parameters were estimated using the training data and held fixed while forecasting the test period.
- **Rolling one-step-ahead backtesting:** Forecasts were generated sequentially using information available before each forecast. The model was re-estimated as new observations became available.

The ARIMA model was compared against two baseline forecasts:

- Historical-mean return
- Zero-return forecast

Forecast accuracy was evaluated using Mean Squared Error (MSE) and Mean Absolute Error (MAE). Directional accuracy, out-of-sample \(R^2\), and the Diebold–Mariano test were also considered.

### 5. Forecasting and Prediction Intervals

A ten-week price forecast was generated from the end of the training period. Simulated forecast paths were used to construct model-based 95% prediction intervals.

The analysis examined the difference between the point forecast and actual prices, as well as how forecast uncertainty increased with the forecast horizon.

### 6. Residual Diagnostics

Residuals from the selected model were examined using:

- Residual time series plots
- Residual histograms
- Autocorrelation function (ACF) plots
- Ljung–Box tests for residual autocorrelation
- Ljung–Box tests on squared residuals
- ARCH-LM test for volatility clustering
- Excess kurtosis and the Jarque–Bera normality test

These diagnostics were used to assess whether important patterns remained unexplained by the fitted model.

## Key Findings

### Model Selection

BIC selected ARIMA(0,0,0), indicating that the simpler constant-mean model was preferred under the BIC criterion among the candidate models evaluated.

### Forecasting Performance

The selected ARIMA model did not demonstrate meaningful improvement over the baseline methods.

| Evaluation Metric | Result |
|---|---:|
| Fixed-origin ARIMA MSE | 0.001411 |
| Fixed-origin ARIMA MAE | 0.0277 |
| Rolling ARIMA MSE | 0.00141 |
| Rolling ARIMA MAE | 0.02772 |
| Zero-return baseline MSE | 0.00140 |
| Out-of-sample \(R^2\) vs. historical mean | 0.0000 |
| Diebold–Mariano test p-value | 0.336 |
| Directional accuracy | 57.1% |

The Diebold–Mariano test did not provide statistically significant evidence of a difference in forecast accuracy. The ARIMA model performed essentially identically to the historical-mean baseline, while the zero-return baseline achieved a marginally lower MSE.

The directional accuracy of 57.1% did not establish predictive skill because the model consistently forecast a positive return, and approximately 57.1% of the test-period returns were positive.

### Residual Diagnostics

The Ljung–Box tests did not find statistically significant residual autocorrelation at the tested lags of 10 and 20.

Similarly, the tests on squared residuals and the ARCH-LM test did not provide statistically significant evidence of volatility clustering at the tested lags.

The excess kurtosis was approximately 0.56, while the Jarque–Bera test was statistically significant at the 5% level (\(p = 0.04\)). This suggests that the residual distribution should not automatically be assumed to be normal.

Failure to reject the diagnostic null hypotheses does not prove that all model assumptions hold.

### Ten-Week Forecast

The model generated a steadily increasing point forecast, reflecting its estimated positive mean weekly log return.

The forecast increased from approximately **156.20** in the first test week to **163.15** in the tenth test week. Actual prices remained below the point forecast during these ten weeks, with the tenth-week actual price at approximately **157.48**.

All ten actual prices fell within their corresponding simulated 95% prediction intervals. However, the intervals widened substantially over the forecast horizon, illustrating the accumulation of uncertainty.

## Limitations

- The analysis focuses on a single stock and one historical period.
- The univariate model does not incorporate external predictors such as trading volume, market indices, or macroeconomic indicators.
- Weekly aggregation may conceal short-term patterns and volatility dynamics visible in daily data.
- The test period contains 105 observations, limiting the precision of forecast-performance comparisons.
- Transaction costs, slippage, and trading-strategy profitability were not evaluated.
- Model-based prediction intervals depend on the fitted model and its error assumptions.
- Historical forecasting performance does not guarantee future performance.

## Future Work

Potential extensions include:

1. Comparing ARIMA(1,0,0) and ARIMA(0,0,1) using the same out-of-sample evaluation procedure.
2. Investigating volatility dynamics with GARCH models using daily returns.
3. Exploring seasonal models if a plausible seasonal structure is identified.
4. Evaluating machine learning approaches such as XGBoost or LSTM using chronological validation and appropriate baselines.
5. Investigating whether additional predictors improve forecasting performance beyond the historical-mean baseline.

## Tools and Technologies

- **Python** — data analysis and modelling
- **Pandas and NumPy** — data preparation and numerical calculations
- **Matplotlib** — time series plots and visualisation
- **Statsmodels** — ADF testing, ARIMA modelling, and residual diagnostics
- **SciPy** — statistical testing and distributional analysis
- **Scikit-learn** — evaluation metrics, where applicable

## Conclusion

This project demonstrates a structured time series analysis workflow, including exploratory analysis, stationarity testing, information-criterion-based model selection, chronological backtesting, forecasting, and residual diagnostics.

The selected ARIMA(0,0,0) model provides a useful baseline for weekly AAPL returns but does not demonstrate predictive improvement over simple benchmarks. The results highlight the importance of evaluating forecasting models out of sample rather than relying solely on model fit or visually plausible forecasts.

The central takeaway is that a statistically valid modelling workflow can produce an important result even when the selected model does not establish meaningful predictive skill.
