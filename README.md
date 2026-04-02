# DD Cafe — Coffee shop demand forecasting and inventory optimisation

A data-driven approach to demand forecasting and inventory decisions for a multi-store coffee shop chain, built as a course project for Data-Driven Models.

## Data

- **Source**: `Coffee Shop Sales (1)(Transactions).csv` (149,116 transaction rows, Jan–Jun 2023)
- **Stores**: Astoria, Hell's Kitchen, Lower Manhattan
- **Key columns**: `transaction_date`, `transaction_time`, `store_location`, `product_category`, `product_type`, `product_detail`, `transaction_qty`, `unit_price`
- **Pre-enriched fields**:
  - `product_type_sized` — product type with size suffix (sm / rg / lg), 44 unique values
  - `COGS_product_type` — product-specific cost-of-goods-sold percentage (20 %, 30 %, or 40 %)

## Notebook files

- `coffee_shop_inventory (1).ipynb` — editable notebook (source)
- `coffee_shop_inventory (1).executed (1).ipynb` — fully executed output snapshot with all four models

## Current notebook structure

| Section | Title | Description |
|---------|-------|-------------|
| 1–8 | EDA, feature engineering, SARIMAX | Exploratory analysis, calendar features, aggregation, weekly SARIMAX forecasting |
| 9 | Feature engineering & daily demand prep | Load CSV, aggregate to daily demand per store × product\_type\_sized |
| 10 | Baseline model (recency-weighted average) | Exponentially decaying weights (λ tuned for 14-day window), 6-week test split |
| 11 | Pilot model (Linear Regression) | Global LR across all stores with calendar, lag, and rolling features |
| 12 | Classical per-series model (ETS / SARIMA) | Holt-Winters ETS per series, conditional SARIMA upgrade for strong/stable series |
| 13 | Global ML model (XGBoost) | XGBoost regressor trained on all store–product series with rich feature set |
| 14 | Risk & inventory strategy (per store) | z-score safety-stock simulation for all 4 models, per store |
| 15 | Evaluation & comparison | Forecast accuracy + business-impact metrics per store, weekly profit trends, profit diff vs baseline |

## Four-model comparison

All models predict daily units sold for every (store, product\_type\_sized) combination over the 6-week test window (42 days). Results and inventory optimisation are handled **separately per store**.

| Model | Type | Scope |
|-------|------|-------|
| **baseline** | Recency-weighted average | Per series |
| **pilot** | Linear Regression | Global (all stores combined) |
| **classical** | ETS / SARIMA | Per series (conditional SARIMA for strong signal) |
| **global** | XGBoost | Global (all stores combined, rich features) |

## Inventory simulation

For each model, the notebook simulates stocking decisions at four z-score risk levels (0.0, 0.5, 1.0, 1.5):

- `stock = ceil(prediction + z × σ)` where σ is per-series residual standard deviation
- `sold = min(stock, actual)`
- `profit = sold × price − stock × unit_cost`
- `waste_cost = unsold × unit_cost`
- `stockout = max(0, actual − stock)`

Results are shown per store: profit pivot by model and z-score, best z-score per model, weekly profit trends, and overall 6-week summary with profit difference vs baseline.

## Economics assumptions

- Prepared quantity is based on forecast (`ceil(prediction)`)
- Unsold prepared items are discarded (waste)
- Unit cost: `avg_unit_price × cogs_pct` (product-specific, 20–40 %)
- Profit: `sold × price − prepared × unit_cost`

## Libraries

`pandas`, `numpy`, `scikit-learn`, `statsmodels`, `xgboost`, `matplotlib`, `seaborn`
