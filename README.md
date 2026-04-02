# DD Cafe - Coffee Shop demand forecasting and inventory optimisation

A data-driven approach to demand forecasting and inventory decisions for a multi-store coffee shop chain, built as a course project for Data-Driven Models.

## Data

- **Source**: `Coffee Shop Sales (1)(Transactions).csv` (149,116 transaction rows, Jan-Jun 2023)
- **Stores**: Astoria, Hell's Kitchen, Lower Manhattan
- **Key columns**: `transaction_date`, `transaction_time`, `store_location`, `product_category`, `product_type`, `product_detail`, `transaction_qty`, `unit_price`
- **Pre-enriched fields in source**:
  - `product_type_sized`
  - `COGS_product_type`

## Notebook files

- `coffee_shop_inventory (1).ipynb` - editable notebook
- `coffee_shop_inventory (1).executed.ipynb` - fully executed output snapshot

## Current notebook structure

| Section | Title | Status |
|---------|-------|--------|
| 1-8 | EDA, feature engineering, aggregation, SARIMAX forecasting | Complete |
| 9 | Feature engineering and daily demand prep for sized product types | Complete |
| 10 | Baseline recency-weighted forecasting | Complete |
| 11 | Multi-model development and test predictions | Complete |
| 12 | Risk and inventory simulation with waste/cost logic | Complete |
| 13 | Evaluation and model comparison | Complete |
| 14 | Pilot mode: one store, one model, full 6-week profit comparison vs baseline | Complete |

## Pilot mode (Section 14)

The latest pilot section is a simplified decision view:

- one selected location (`TARGET_STORE`)
- one model (linear regression)
- all `product_type_sized` values for that location
- full 6-week test period
- weekly and overall comparison against baseline on:
  - profit
  - waste cost
  - prediction totals
  - actual totals
  - `profit_diff` (`model_profit - baseline_profit`)

## Economics assumptions

- prepared quantity is based on forecast (`ceil(prediction)`)
- unsold prepared items are discarded (waste)
- unit cost is derived from margin proxy: `unit_cost = avg_unit_price * cogs_pct`
- profit formula: `sold * price - prepared * unit_cost`

## Libraries

`pandas`, `numpy`, `scikit-learn`, `statsmodels`, `matplotlib`, `seaborn`
