# Data Quality Notes

This document explains the data quality issues encountered during analysis  
of `inventory_management.db` and the decisions made to handle each one.  
All issues stem from the synthetic nature of the simulation and would not  
occur in a production SAP environment.

---

## Issue 1 — Negative Lead Times (47.8% of raw delivery records)

**What was observed:**  
477 of 998 raw delivery records produced negative lead times when joining  
`PurchaseOrderDocuments` to `GoodsReceiptsAndIssues` — meaning the goods  
receipt date preceded the purchase order creation date, which is physically  
impossible in a real procurement system.

**Root cause:**  
The simulator generates purchase orders and goods receipt events  
independently and links them via `purchase_document_number` as a  
post-hoc assignment. This occasionally results in a receipt event  
being linked to a PO that was created after the movement was posted.  
In a real SAP system, a goods receipt cannot be posted without a  
pre-existing purchase order — the system enforces this at entry time.

**How it was handled:**  
All records where `receipt_date < purchase_order_date` were removed  
before any lead time analysis. This left 521 clean records across  
100 vendors and 624 purchase orders. The removal rate and root cause  
are documented transparently in the notebook (Section 0, Q1).

**Impact on analysis:**  
The 521 retained records are sufficient for meaningful analysis.  
There is no evidence of systematic bias — negative records appear  
distributed across vendors rather than concentrated in a subset,  
suggesting random assignment rather than a structural flaw.

---

## Issue 2 — Incomplete Purchase Requisition Coverage (54.6%)

**What was observed:**  
Only 546 of 1,000 purchase orders had a matching record in  
`PurchaseRequisitions` with a valid `latest_possible_goods_receipt` date.  
The remaining 454 POs could not be classified as on-time or late.

**Root cause:**  
The simulator generates purchase orders and purchase requisitions as  
partially independent processes. In real SAP, every PO is mandatorily  
preceded by a PR — the system enforces a PR → PO document flow.  
The simulation does not enforce this constraint, resulting in POs  
that exist without a corresponding requisition.

**How it was handled:**  
On-time delivery analysis (Q2) and financial impact analysis (Q4)  
were scoped to the 546 matched POs only. All findings are presented  
with this coverage limitation clearly stated. The 546 matched POs  
span 84 unique vendors, providing adequate coverage for vendor-level  
reliability scoring.

**Impact on analysis:**  
The on-time rate (41.6%) and financial exposure ($20.99M) are  
calculated on a representative sample rather than the full population.  
The true figures could differ, though the directional conclusions  
are unlikely to change given the consistent pattern across vendors.

---

## Issue 3 — No Opening Stock Balance

**What was observed:**  
`MaterialDocuments` records movements from 2025-03-16 onwards with no  
opening stock snapshot. Several materials showed negative running stock  
from their very first recorded movement — implying they were drawing  
down inventory that existed before the simulation's recording window.

**Root cause:**  
The simulator initialises material movements without recording the  
stock position at simulation start. In a real SAP environment, a  
period-opening stock balance would be captured and available for  
historical reconstruction.

**How it was handled:**  
Two mitigations were applied in Q3:

1. **Materials whose first movement was a Goods Issue were excluded**  
   (262 of 490 materials) — these are most likely drawing on  
   pre-simulation opening stock rather than representing genuine stockouts.

2. **First stockout dates were examined** — retained materials showed  
   first stockouts occurring 2–5 months into the observation window,  
   confirming the filter removed the most obvious opening-balance artefacts.

The stockout rate (40.4%) and tier comparison findings are treated as  
directionally indicative rather than precise measurements.

**Impact on analysis:**  
The 47-point spread within the Unreliable vendor tier and the  
counterintuitive At Risk vs Unreliable stockout rate finding (Q3)  
may be partially influenced by residual opening-balance gaps.  
The chi-squared test result (p = 0.1463) already reflects the  
limited statistical power available after filtering.

---

## Issue 4 — Vendor ID Type Inconsistency

**What was observed:**  
`vendor_id` values were stored as integers in some tables and strings  
in others, causing silent join failures when comparing across dataframes  
built from different queries.

**Root cause:**  
SQLite does not enforce column types — values are stored as the type  
inferred at insertion time, which varied across tables in the simulation.

**How it was handled:**  
Explicit `.astype(str)` or `.astype(int)` conversions were applied  
before every cross-dataframe merge involving vendor IDs. This is  
documented in the relevant notebook cells.

---

## Summary Table

| Issue | Records Affected | Action Taken | Analysis Impact |
|---|---|---|---|
| Negative lead times | 477 records (47.8%) | Removed | Q1, Q2, Q4, Q5 scoped to 521 clean records |
| Missing PR records | 454 POs (45.4%) | Scoped analysis to matched POs | On-time and financial metrics are sample-based |
| No opening stock balance | 262 materials (53.5%) | Excluded first-move-Issue materials | Stockout rate directionally valid, not precise |
| Vendor ID type mismatch | All cross-table joins | Explicit type casting applied | No data loss — prevented silent join failures |

---

*All issues are artefacts of synthetic data generation and are expected  
in simulated ERP environments. They do not reflect the quality of  
analysis performed — handling them transparently is itself an  
analytical skill demonstrated by this project.*