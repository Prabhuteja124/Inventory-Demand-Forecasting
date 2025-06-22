# Spare Parts Inventory Demand Forecasting

## Overview

This project involves forecasting the demand for spare parts in service centers to improve inventory planning and reduce stockouts or overstocking. We used Facebook Prophet, a time series forecasting model, to predict demand on both daily and weekly levels.

## Objectives

* Clean and preprocess service center spare part data
* Categorize parts into daily, weekly, low-priority, and ignored
* Forecast demand for daily and weekly parts using Prophet
* Recommend a minimum stock level for low-priority and ignored parts
* Visualize and save forecast results

## Dataset Description

The dataset contains records of spare part usage with fields like:

* `invoice_date`: Date of usage
* `Job Card Date`, `Vehicle No.`, `Vehicle Model`, `Current KM Reading`
* `Spare_parts`: Name of the spare part
* `demand_count`: Quantity used

## Step-by-Step Workflow

### 1. Data Preparation

* Parsed dates and removed missing or erroneous values
* Aggregated daily demand for each part

### 2. Part Classification

* Based on usage frequency:

  * **Daily Parts**: Frequently used, forecasted on a daily basis
  * **Weekly Parts**: Moderately used, forecasted on a weekly basis
  * **Low Priority**: Rarely used, assigned a buffer stock
  * **Ignore**: No forecasting needed, optionally buffered

### 3. Time Series Preprocessing

* Created continuous time series using `resample(...).asfreq()`
* Handled missing dates and interpolated missing values
* Applied log-transform conditionally to stabilize variance
* Clipped outliers before forecasting

### 4. Forecasting with Prophet

#### Daily Parts Forecast

* Used `Prophet` with daily frequency
* Forecasted for the next **30 days**
* Saved plots and forecast CSV: `prophet_30_day_demand_forecast.csv`

#### Weekly Parts Forecast

* Used `Prophet` with weekly frequency
* Added custom monthly and yearly seasonality
* Forecasted for the next **12 weeks**
* Saved plots and forecast CSV: `prophet_12_week_demand_forecast.csv`

### 5. Forecast Evaluation

* Metrics used:

  * **MAE (Mean Absolute Error)**
  * **RMSE (Root Mean Squared Error)**
* Visual comparison with training/test/forecast data

<!-- ### 6. Final Output

* Combined both daily and weekly forecasts into a single DataFrame
* Rounded off the forecasted values
* Output example:

```
date       forecast_demand  Spare_parts     forecast_type
2019-01-08               4      3m oil            Daily
2019-01-08               2  Clutch cable         Daily
2019-01-08               8  Chain overhaul       Weekly
```

* Saved final output: `combined_forecast.csv` -->

### 6. Handling Low Priority and Ignore Parts

* No Prophet forecasting performed
* Recommended fixed minimum stock buffer (e.g., 2-3 units)

## Directories and Files

* `prophet_daily_forecast_plots/`: Daily forecast plots
* `prophet_weekly_forecast_plots/`: Weekly forecast plots
* `prophet_30_day_demand_forecast.csv`: Daily part predictions
* `prophet_12_week_demand_forecast.csv`: Weekly part predictions
<!-- * `combined_forecast.csv`: Final rounded predictions -->

## Tools Used

* Python
* Pandas, NumPy
* Facebook Prophet
* Matplotlib, Seaborn
* scikit-learn

## Future Improvements

* Include external regressors (e.g., holidays, events)
* Compare Prophet with LSTM, XGBoost, and SARIMA
* Deploy as a web dashboard or API for real-time forecasting

## Conclusion

Prophet proved effective for both daily and weekly demand forecasting. With appropriate preprocessing and tuning, it handled seasonality and trends well. For less frequently used parts, a buffer stock strategy was adopted instead of forecasting.
