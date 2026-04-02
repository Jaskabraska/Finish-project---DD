# DD Cafe -- Coffee Shop Demand Forecasting & Inventory Optimisation

A data-driven approach to demand forecasting and inventory management for a multi-store coffee shop chain, built as a course project for Data-Driven Models.

## Data

- **Source**: `Coffee Shop Sales.xlsx` -- 149,116 transaction rows, Jan-Jun 2023
- **Stores**: Astoria, Hell's Kitchen, Lower Manhattan
- **Key columns**: `product_type`, `product_detail`, `transaction_qty`, `transaction_date`, `store_location`, `unit_price`
- **Derived data**: `Coffee Shop Sales - with sized types.xlsx` -- enriched version with size-typed products

## Notebook structure

The main analysis lives in `coffee_shop_inventory (1).ipynb`:

| Section | Title | Status |
|---------|-------|--------|
| 1-8 | EDA, time series analysis, SARIMAX | Complete |
| 9 | Feature engineering & data preparation | Planned |
| 10 | Baseline model (weighted average) | Planned |
| 11 | Model development (SMA, Holt-Winters, LR, Random Forest) | Planned |
| 12 | Risk & inventory strategy (std dev experiment) | Planned |
| 13 | Evaluation & comparison | Planned |

## Planned sections (9-13)

### Section 9 -- Feature engineering
Create a `product_type_sized` column by parsing size abbreviations (Sm/Rg/Lg) from `product_detail` and aggregate to daily demand per product type per store.

### Section 10 -- Baseline model
Recency-weighted average forecast using exponentially decaying weights with ~80% weight on the most recent 14 days.

### Section 11 -- Model development
Train-test split (last 42 days as test). Models per store x product type:
- Simple moving average (7-day, 14-day)
- Exponential smoothing (Holt-Winters, period=7)
- Linear regression (day of week, lags, rolling means)
- Random Forest (same feature set)

### Section 12 -- Risk & inventory strategy
Simulate inventory decisions at different risk levels (z = 0, 0.5, 1.0, 1.5) using the 40% loss constraint on unsold stock.

### Section 13 -- Evaluation & comparison
Compare models on forecast accuracy (MAE, RMSE, MAPE) and business impact (profit, waste, stockouts) with visualisations.

## Libraries

`pandas`, `numpy`, `scikit-learn`, `statsmodels`, `matplotlib`, `seaborn`
