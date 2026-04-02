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
| 16 | Fixed-capacity optimisation: integer allocation by product type under `sum(Q_p)=C` | Complete |

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

## Fixed-capacity optimisation (Section 16)

Section 16 adds a dedicated allocation optimiser for production planning under a fixed capacity budget.

- decision variable: `Q_p` (integer units per `product_type_sized`)
- hard capacity constraint: `sum(Q_p) = C` for each store
- objective: maximise expected profit from sold units minus production cost
- scope: one random day from the final 6-week test set, run separately for each store
- comparison axes:
  - forecasting model (`baseline_pred`, `ma7_pred`, `ma14_pred`, `ets_pred`, `lr_pred`, `rf_pred`)
  - risk loading with `D_tilde = D_hat + k * sigma` for multiple `k` values
- outputs:
  - optimal product mix (`Q_p`)
  - expected profit
  - expected waste and stockout
  - best strategy per store and baseline-vs-advanced comparison

## Economics assumptions

- prepared quantity is based on forecast (`ceil(prediction)`)
- unsold prepared items are discarded (waste)
- unit cost is derived from margin proxy: `unit_cost = avg_unit_price * cogs_pct`
- profit formula: `sold * price - prepared * unit_cost`

## Libraries

`pandas`, `numpy`, `scikit-learn`, `statsmodels`, `matplotlib`, `seaborn`
