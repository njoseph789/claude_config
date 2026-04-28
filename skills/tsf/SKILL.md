---
name: tsf
description: An end-to-end framework for exploratory data analysis, preprocessing, modeling, validation, and forecasting of time series datasets using classical and state-space models.
---

# Skill: Time Series Forecasting and Analysis

## Description
An end-to-end framework for exploratory data analysis, preprocessing, modeling, validation, and forecasting of time series data. Apply this methodology sequentially whenever requested to analyze time series datasets or build forecasting models.

## Phase 1: Exploratory Data Analysis (EDA)
1. **Initial Data Inspection:**
   - Load and examine data structure (shape, dtypes, missing values).
   - Ensure the datetime index is properly formatted.
   - Check for and handle data quality issues (duplicates, gaps).
2. **Visualization Requirements:**
   - **CRITICAL VISUALIZATION RULE:** For all primary data visualizations, plots, and trend lines, use the hex code `#800000` as the primary color.
   - Plot the raw time series to understand scale, trends, and patterns.
   - Create multiple views: full series, zoomed sections, year-over-year.
   - Plot seasonal decomposition (trend, seasonal, residual components).
3. **Anomaly Detection:**
   - Detect and document outliers.
   - Justify treatment (keep, remove, or impute) and visualize before/after if transformed.
4. **Statistical Characterization:**
   - Compute summary statistics.
   - Plot seasonality patterns (by month/quarter/day of week).
   - Calculate and interpret ACF/PACF plots.
   - Assess for regime-switching behavior (useful for evaluating HMM viability).

## Phase 2: Stationarity & Preprocessing
1. **Testing:**
   - Run Augmented Dickey-Fuller (ADF) test.
   - Run KPSS test to confirm ADF results.
   - Visually inspect rolling mean and standard deviation.
2. **Transformation (If Non-Stationary):**
   - Apply 1st or seasonal differencing.
   - Apply log transformation if variance is increasing.
   - Re-test stationarity.
   - **Crucial:** Track all transformations to reverse them during final forecasting.

## Phase 3: Train/Test Split & Model Selection
1. **Data Splitting:**
   - Strictly use temporal splits (e.g., 80/20 or last X periods). Never use random splits.
   - Document the split point clearly to prevent data leakage.
2. **Model Selection Criteria:**
   - **ARIMA/SARIMAX:** For univariate series with clear seasonal/autocorrelation structures. Specify orders (p, d, q) via ACF/PACF and seasonal parameters (P, D, Q, s).
   - **State-Space Models:** When dealing with unobservable state variables, measurement noise, or structural decomposition needs. Define state and observation equations.
   - **Kalman Filter:** For linear Gaussian state-space models requiring real-time filtering, handling missing observations, or optimal state estimation with uncertainty.
   - **Hidden Markov Models (HMM):** For distinct regime-switching behavior (e.g., economic cycles, system states). Specify states, emissions, and evaluate continuous vs. discrete applications.
3. **Model Fitting:**
   - Fit strictly on training data.
   - Record and output all model parameters and selection rationales.

## Phase 4: Model Validation (CRITICAL)
1. **In-Sample Diagnostics:**
   - Plot actual vs. fitted values. *Warning: If predictions are a flat line, investigate over-differencing or fitting on constants.*
2. **Residual Analysis:**
   - Plot residuals over time (must resemble white noise).
   - Check ACF of residuals and run Box-Ljung test ($p > 0.05$).
   - Check residual distribution via histogram and Q-Q plots.
3. **Model-Specific Checks:**
   - *Kalman Filter:* Check innovation sequence, verify one-step-ahead errors, examine Kalman gain.
   - *HMM:* Plot decoded state sequence (Viterbi), verify state interpretability, examine transition matrices.
   - *State-Space:* Check smoothed vs. filtered estimates, verify identifiability.
4. **Out-of-Sample Validation:**
   - Generate test set predictions.
   - Calculate MAE, RMSE, MAPE.
   - Evaluate model degradation on the test set to detect overfitting.

## Phase 5: Forecasting & Interpretation
1. **Forecasting Generation:**
   - Generate future forecasts beyond the test set.
   - Include and visualize confidence/prediction intervals.
2. **CRITICAL FORECASTING PLOT RULE:** - **Never isolate the forecast.** Always plot the newly generated forecasts against the continuous historical data.
   - The historical data must remain visually integrated with the forecast to maintain visual tracking of existing numbers and trends over time.
3. **Model Comparison:**
   - Compare multiple models using AIC, BIC, out-of-sample RMSE, and log-likelihood.
4. **Interpretation & Delivery:**
   - Interpret coefficients, hidden states, and transition dynamics based on the model type.
   - Perform sanity checks: Do forecasts make domain sense? Are bounds reasonable?
   - Deliver clear justifications for model choices, evidence of met assumptions, and an honest discussion of uncertainty/limitations.

## Common Mistakes to Avoid (Checklist)
- [ ] Skipping stationarity tests or not checking residuals for white noise.
- [ ] Randomly splitting train/test sets or leaking test data.
- [ ] Accepting flat-line predictions.
- [ ] Over-differencing ($d > 2$).
- [ ] Forgetting to reverse transformations.
- [ ] Omitting historical data in final forecasting plots.
- [ ] Omitting confidence intervals.
- [ ] Poorly specifying matrices (Kalman) or choosing incorrect state counts (HMM).