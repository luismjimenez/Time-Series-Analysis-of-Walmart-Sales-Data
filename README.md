# Walmart Weekly Sales Forecasting - Time Series Analysis

## Executive Summary:
This analysis provides a comprehensive time-series evaluation of Walmart’s weekly sales data across 45 stores to identify key revenue drivers and optimize forecasting accuracy. The investigation reveals that while external factors, such as temperature, fuel prices, and unemployment rates, yield surprisingly low predictive power ($R^2 \approx 2.5\%$), sales are strongly driven by seasonal cycles and holiday events, which generate an average revenue increase of 7.84%.
To address these fluctuations, several models were tested: a baseline Moving Average, ARIMA, an optimized SARIMA model, Prophet, and an advanced XGBoost model. 
The XGBoost model proved most effective at capturing trend and seasonality, though all models struggled to fully account for extreme holiday peaks. These findings suggest that future forecasting efforts should prioritize internal promotional calendars and store-specific seasonal trends over macroeconomic indicators to improve the precision of inventory and labor planning.

## Business Problem:
Retailers like Walmart face significant challenges in inventory management and labor allocation due to the high volatility of weekly sales across diverse geographic locations. While it is often assumed that macroeconomic indicators, such as fuel prices, unemployment rates, and temperature, drive consumer behavior, this analysis demonstrates that these external factors have minimal predictive power, accounting for only approximately 2.5% of the variance in sales.

The core challenge lies in accurately forecasting extreme demand spikes driven by seasonal cycles and holiday events, which typically result in a revenue increase of 7.84%. 
Traditional forecasting models often underperform during these peak periods, leading to potential stockouts or overstaffing. 
This project seeks to identify the most effective time-series model to capture these quarterly seasonalities and holiday peaks, enabling data-driven decisions that prioritize internal promotional calendars over unreliable external economic markers.

## Methodology:

This project involved a comprehensive time series analysis of Walmart's weekly sales data to forecast future sales.

1.  **Data Loading and Preprocessing**: Initial data was loaded and preprocessed to convert `Date` columns to datetime objects and handle any missing values. The data was then aggregated to create a unified weekly sales time series across all stores.

2.  **Exploratory Data Analysis (EDA)**: Extensive EDA was performed to understand sales distributions, store-wise performance, and the impact of external factors (Temperature, Fuel Price, CPI, Unemployment) and holidays on weekly sales. Time series decomposition and stationarity tests (ADF test and ACF/PACF plots) were conducted to identify underlying trends and seasonality.

3.  **Feature Engineering**: For advanced models, new time-series features were engineered, including lagged sales, rolling mean and standard deviation, and Fourier series terms to capture seasonality.

4.  **Model Selection and Training**: Several forecasting models were implemented and evaluated:
    *   **Baseline Moving Average (MA)**: A simple 4-week moving average served as a benchmark.
    *   **ARIMA (Autoregressive Integrated Moving Average)**: A traditional time series model was applied to capture short-term dependencies.
    *   **SARIMA (Seasonal ARIMA)**: Optimized using a grid search, this model accounted for seasonality.
    *   **Prophet**: Facebook's forecasting tool was used for its robustness with trends and multiple seasonality.
    *   **XGBoost (Extreme Gradient Boosting)**: A powerful machine learning model trained on engineered features to capture complex non-linear relationships.

5.  **Model Evaluation**: All models were evaluated on a dedicated test set (last 13 weeks of data) using key metrics such as Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), and Mean Absolute Percentage Error (MAPE). Performance was compared against the baseline, and the best-performing model was identified.

## Skills:

*   **Programming Languages**: Python
*   **Libraries**: Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn, Statsmodels, Prophet, XGBoost
*   **Methodologies**: Time Series Analysis, Exploratory Data Analysis (EDA), Feature Engineering, Model Training & Evaluation, Statistical Modeling (ARIMA, SARIMA), Machine Learning (XGBoost)
*   **Tools**: Jupyter Notebook/Google Colab

## Results:

1. **Weekly sales show strong seasonality**, with major spikes around Thanksgiving and Christmas.
2. **Holiday weeks boost sales by ~7–8%**, making them the most influential external factor.
3. Economic variables like temperature, CPI, fuel price, and unemployment show **weak correlation** and explain **<3%** of variance.
4. A few stores generate **disproportionately high revenue**, while high-sales stores also show **higher volatility**.
5. The aggregated time series is **stationary** but highly seasonal, confirmed by decomposition and ADF tests.
6. **XGBoost delivers the best forecast accuracy (MAPE ~1.76%)**, outperforming Prophet, ARIMA, SARIMA, and the Moving Average baseline.
7. **SARIMA captures seasonality better than ARIMA**, but both models struggle with extreme holiday spikes.
8. Simple averages (MA(4)) provide a reasonable baseline but **fail to capture seasonal patterns** and spikes.
9. Forecast accuracy can be improved by **adding custom holiday effects** and using **store-level hierarchical models**.
10. Future work should explore hyperparameter tuning for XGBoost, integrate additional external factors, consider ensemble techniques, investigate deep learning models such as LSTMs or Transformers, and build a **production-ready forecasting pipeline**.

**Model Performance:** The more advanced XGBoost model outperformed the other models, successfully capturing quarterly seasonal patterns that simpler models missed.

![Model Performance](Walmart_Sales_Forecast_Model_Performance.png)
*Figure 1: XGBoost Forecast vs. other models*

![Macroeconomic Variance Impact](walmart_sales_variance_impact.png)

**Macroeconomic Impact:** Factors such as fuel prices and unemployment rates accounted for only 2.5% of sales variance, indicating they are not primary drivers of consumer demand in this dataset.

**Holiday Surge:** Specific holiday events drive an average revenue lift of 7.84%, with the largest spikes occurring during Thanksgiving and Christmas.

**Forecasting Limits:** While time-series models capture general seasonality, they consistently underestimate the "peak-of-peak" demand during major holidays.

## Business Recommendations:

**Shift Strategic Focus:** Transition resources from monitoring external macroeconomic indicators toward optimizing internal promotional and seasonal event calendars.

**Apply Holiday Multipliers:** Implement a manual "Holiday Overlay" (a percentage-based multiplier) to model Q4 forecasts and prevent inventory stockouts.

**Store-Specific Buffering:** Prioritize safety stock for high-volatility locations (e.g., Store 20), which exhibit larger-than-average absolute dollar spikes during peak periods.

**Quarterly Planning Cycles:** Utilize forecasts for mid-term (13-week) labor and inventory planning to align with established quarterly seasonal cycles.

## Next Steps:
- **Integrate Store-Specific Multipliers:** Expand the "Holiday Overlay" logic to include the top 5 most volatile stores (Stores 20, 4, 14, 13, and 2), applying customized multipliers to
  account for their higher-than-average seasonal spikes.
- **Feature Engineering for Promotional Data:** Incorporate internal Walmart promotional calendars (Markdowns) into the model to see if it improves accuracy during extreme holiday windows.
- **Deployment of Automated Alerts:** Develop a monitoring script that flags significant deviations between the forecast and actual sales in real-time, allowing for rapid inventory
  adjustments.
- **Long-Term Demand Planning:** Use the model to run "what-if" scenarios regarding future labor requirements based on predicted seasonal volume surges across different store tiers.
- **Periodic Model Retraining:** Establish a quarterly retraining schedule to ensure the parameters adapt to evolving consumer trends and potential shifts in seasonal timing.
