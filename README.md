# Inventory & Demand Forecasting — Supply Chain Analytics

End-to-end pipeline that forecasts weekly product demand and computes inventory optimization metrics (safety stock, reorder point, EOQ) for top SKUs, visualized in an interactive Power BI dashboard.

## Problem Statement
Retailers/supply chain teams need to know how much stock to hold and when to reorder, without over-stocking (holding cost) or under-stocking (stockout risk). This project forecasts short-term demand per product and translates that forecast directly into actionable inventory decisions.

## Dataset
[DataCo Smart Supply Chain for Big Data Analysis](https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis) — ~180K order records across products, regions, and shipping modes (2015-2017).

## Approach
1. **Data cleaning** — handled missing values, deduplication, date parsing
2. **SQL layer** — structured cleaned data into a queryable schema (SQLite), aggregate queries for demand by category/region/month
3. **ABC classification** — ranked products by revenue contribution to prioritize which SKUs to forecast (Pareto analysis)
4. **Demand forecasting** — Facebook Prophet, weekly granularity, per-product models with time-based train/test split
5. **Inventory optimization** — computed safety stock, reorder point (95% service level), and EOQ per product from forecasted demand and its variability
6. **Dashboard** — Power BI, with a live slicer to switch between products and view demand trend, forecast confidence interval, ABC revenue breakdown, and inventory metrics side by side

## Results
- Forecasted weekly demand for 5 top SKUs (mixed A/B class)
- **Mean WAPE: 23%** across selected products (chose WAPE over MAPE — MAPE is distorted by low-demand weeks where the denominator is small; WAPE weights by total volume and is the more reliable accuracy metric for this kind of intermittent weekly demand)
- Computed per-product reorder points and safety stock at a 95% service level, and EOQ under assumed ordering/holding cost inputs (stated explicitly in the notebook, since actual cost data wasn't available in the dataset)

## Tools
Python (pandas, Prophet, numpy), SQLite, Power BI

## Repo Structure
- `notebook.ipynb` — full pipeline: cleaning → SQL → EDA → forecasting → inventory metrics
- `outputs/` — exported CSVs used to build the dashboard
- `dashboard_screenshot.png` — Power BI dashboard preview

## Notes / Limitations
- Lead time, ordering cost, and holding cost are assumed values (stated in the notebook) since the dataset doesn't include actual procurement cost data — in a production setting these would come from real vendor contracts
- Forecast accuracy varies by product; WAPE ranged 18-29% across the selected SKUs
Built end-to-end demand forecasting pipeline on 180K+ order records; achieved 23% mean WAPE across top SKUs and computed reorder points/safety stock at 95% service level, translated into an interactive Power BI dashboard
