# ASOS Inventory & Phantom Revenue Analysis

**Tools:** Python · pandas · matplotlib · seaborn  
**Dataset:** ASOS Product Data (scraped/compiled) · ~150+ brands · pricing & stockout data  
**Notebook:** [ASOS Inventory & Phantom Revenue Analysis.ipynb](ASOS%20Inventory%20%26%20Phantom%20Revenue%20Analysis.ipynb)

---

## Project Overview

This project analyses pricing strategy and inventory availability across fashion brands sold on ASOS, with a focus on identifying phantom revenue opportunities — lost sales caused by stockouts.

In e-commerce, stockouts represent hidden revenue leakage: customers arrive with intent to purchase but leave empty-handed due to unavailable inventory. This analysis explores how price positioning interacts with stockout rates, revealing inefficiencies in inventory planning and brand strategy.

The workflow combines exploratory data analysis with strategic segmentation to uncover patterns across brands, highlight outliers, and translate insights into actionable recommendations for pricing and inventory optimisation.

---

## Key Findings

| # | Finding |
|---|---------|
| 1 | Mid-priced brands (£40–£60) show the highest stockout pressure (~25–30%) — indicating strong demand but insufficient inventory |
| 2 | Low-priced brands (<£40) have highly volatile stockout rates (5%–60%) — suggesting inconsistent demand forecasting |
| 3 | Premium brands (£80+) maintain low stockout rates (<20%) — likely due to conservative inventory strategies |
| 4 | Brands like Topshop, Mango, and Pull&Bear combine high stockouts with mid-range pricing — clear missed revenue opportunities |
| 5 | Naked stands out as a premium-priced brand with elevated stockouts (~41%) — potential understocking despite higher margins |
| 6 | No strong linear correlation between price and stockouts — performance is driven more by brand positioning and demand predictability |

---

## Workflow

| Step | Description |
|---|---|
| 1 | Data Cleaning – Price Column |
| 2 | Extract Brand from Product Description |
| 3 | Standardize Brand Names |
| 4 | Stockout Analysis – Phantom Revenue |
| 5 | Brand‑Level Aggregation |
| 6 | Visualisation – Brand Strategy Quadrant |
| 7 | Key Insights & Business Recommendations |

---

## Visuals

### Price vs Stockout Rate — Brand Strategy Map
![Brand Strategy Map](Brand%20Strategy%20Analysis.png)
This chart segments brands into four strategic quadrants:

- **Top-left:** Low price, high stockout → Operational inefficiency
- **Top-right:** High price, high stockout → Missed premium revenue
- **Bottom-left:** Low price, low stockout → Stable volume players
- **Bottom-right:** High price, low stockout → Conservative premium strategy
---

## Analysis Approach

The core of the analysis aggregates product-level data into brand-level metrics:

- **Average Price (£)** → proxy for brand positioning
- **Average Stockout Rate** → proxy for unmet demand

These metrics are plotted to create a strategy map, enabling quick identification of:

- Understocked high-demand brands
- Over-supplied low-demand brands
- Balanced performers

Reference lines at:

- **£40** (price threshold)
- **40%** stockout rate

divide the space into actionable segments.

---

## Business Recommendations

| Recommendation | Rationale |
|---|---|
| **Increase inventory for mid-priced high-demand brands** | Brands in the £40–£60 range with high stockouts represent the largest immediate revenue opportunity — demand is proven, supply is insufficient. |
| **Improve demand forecasting for low-price brands** | High volatility suggests poor forecasting models — implement data-driven replenishment using historical sales patterns. |
| **Test controlled stock increases for premium brands like Naked** | Elevated stockouts at high price points indicate untapped willingness to pay. |
| **Introduce dynamic inventory allocation** | Shift stock toward high-performing brands in real time instead of static allocation. |
| **Quantify phantom revenue** | Estimate lost sales from stockouts to prioritise inventory investment decisions. |

---

## Dataset

Download the dataset from the following drive link:

https://drive.google.com/drive/folders/1W569j3au1iYVtWm5WeWer2gBGU2Xwzui

Place the dataset in the same folder as the ipynb file.

---

*This is Project 1 in my data analyst portfolio.*  

*→ See also: [Supplier Performance Analysis](../Supplier_Performance_Analysis/)*  
*→ See also: [Customer Churn Prediction](../Customer_Churn_Prediction/)*
*→ See also: [Supply Chain Operations Dashboard](../Supply_Chain_Operations_Dashboard/)*