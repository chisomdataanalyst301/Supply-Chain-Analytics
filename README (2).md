# 📦 Supply Chain Operations Analytics Dashboard

> End-to-end supply chain analytics project — from synthetic data generation and cleaning in Python, to optimization modeling, to an interactive Power BI dashboard.

**Author:** Chisom Precious

---

## 🎯 Project Overview

This project simulates a multi-warehouse supply chain operation across Nigeria (Lagos, Abuja, Port Harcourt, Kano) involving 8 product SKUs, 5 suppliers (local and international), and 2,000 orders spanning 2023–2024.

The goal was to build a complete analytics pipeline:

1. **Generate** a realistic synthetic supply chain dataset
2. **Clean** the data and handle common data quality issues
3. **Engineer features** for deeper analysis (demand trends, supplier reliability, inventory turnover)
4. **Apply optimization models** — EOQ, Safety Stock/Reorder Point, and Supplier Scoring
5. **Visualize** everything in an interactive Power BI dashboard
6. **Translate findings into actionable business recommendations**

---

## 🗂️ Repository Structure

```
supply-chain-analytics/
│
├── data/
│   ├── supply_chain_data.csv          # Raw generated dataset (2,000 rows, 24 cols)
│   └── supply_chain_cleaned.xlsx      # Cleaned + feature-engineered dataset (34 cols)
│
├── notebooks/
│   └── Supply_Chain_Analysis.ipynb    # Step-by-step Jupyter notebook
│
├── scripts/
│   └── supply_chain_analysis.py       # Full pipeline as a standalone script
│
├── dashboard/
│   ├── supply_chain_dashboard.html    # Interactive HTML preview dashboard
│   └── Supply_Chain_Dashboard.pbix    # Power BI dashboard file
│
├── images/
│   └── dashboard_screenshot.png       # Final dashboard screenshot
│
└── README.md
```

---

## 📊 Dataset Description

The dataset (`supply_chain_data.csv`) contains **2,000 synthetic orders** with the following fields:

| Column | Type | Description |
|---|---|---|
| `order_id` | ID | Unique order identifier |
| `order_date` | Date | Date order was placed |
| `sku` / `product_name` / `category` | Product | Product identity and category (Food, Healthcare, Stationery, Construction) |
| `warehouse_id` / `warehouse_city` | Logistics | Receiving warehouse |
| `supplier_id` / `supplier_name` / `supplier_country` | Supplier | Supplier identity and origin |
| `order_quantity` | Demand | Units ordered |
| `unit_cost_ngn` / `total_order_cost` / `shipping_cost` / `total_cost` | Cost | Cost breakdown in NGN |
| `promised_lead_days` / `actual_lead_days` / `delay_days` / `delivery_delayed` | Logistics | Delivery performance |
| `inventory_before` / `inventory_after` / `reorder_point` / `stockout_flag` | Inventory | Stock levels and stockout indicator |
| `supplier_reliability_score` | Supplier | Base reliability rating (0–1) |

---

## 🧹 Data Cleaning

Implemented in `scripts/supply_chain_analysis.py`, the cleaning step handles:

- Duplicate order IDs
- Non-positive values in cost/quantity fields
- Unrealistic lead times (clipped to 1–180 days)
- Outliers in order quantity (IQR method, 3× bounds)
- Inconsistent SKU formatting (standardized to uppercase)

---

## ⚙️ Feature Engineering

| Feature | Description |
|---|---|
| `rolling_30d_demand` / `rolling_90d_demand` | Rolling average demand per SKU — smooths short-term noise |
| `lead_time_std` | Standard deviation of supplier lead times — measures delivery consistency |
| `composite_sup_score` | Weighted score combining reliability, on-time rate, and delay severity |
| `inv_turnover_proxy` | Order quantity ÷ inventory before — proxy for stock movement speed |
| `total_cost_per_unit` | Total cost ÷ quantity — normalized cost comparison |
| `is_peak_season` | Flag for May–July (identified demand peak months) |

---

## 📈 Optimization Models

### 1. Economic Order Quantity (EOQ)
```
EOQ = √(2 × Annual Demand × Ordering Cost / Holding Cost per Unit)
```
Assumptions: Ordering cost = ₦5,000/order, Holding cost = 20% of unit cost annually.

**Result:** Identifies optimal order size and reorder frequency per SKU. E.g., SKU-B002 (Paracetamol) → order 601 units every ~35 days.

### 2. Safety Stock & Reorder Point (95% Service Level)
```
Safety Stock = z × √(avg_lead_time × std_demand² + avg_demand² × std_lead_time²)
Reorder Point = (avg_demand × avg_lead_time) + Safety Stock
```
z = 1.65 for 95% service level.

**Result:** Per-SKU safety stock buffers and reorder triggers accounting for both demand and lead-time variability.

### 3. Supplier Composite Scoring
Weighted scoring model combining:
- On-time delivery rate (35%)
- Average delay severity (25%)
- Lead time consistency (20%)
- Unit cost competitiveness (20%)

**Result:** Ranked supplier scorecard — identifies top performers (LocalMart Wholesale) vs. underperformers (MedTech Distributors).

---

## 🖥️ Dashboard

Built in Power BI, the dashboard includes:

- **KPI Cards:** Total Spend, Total Orders, Delivery Delay Rate, Stockout Rate
- **Monthly Demand Trend** (line chart) — reveals May–July seasonal peak
- **Spend by Category** (donut chart)
- **Supplier Performance** (bar chart)
- **Supplier Scorecard Table** with conditional formatting
- **Stockout Rate by SKU** (bar chart)
- Interactive slicers: Year, Category, Supplier

![Dashboard Screenshot](images/dashboard_screenshot.png)

---

## 🔑 Key Findings & Recommendations

| Finding | Recommendation |
|---|---|
| **56% of deliveries are delayed** | Renegotiate SLAs with MedTech Distributors (61% delay rate); introduce penalty clauses |
| **Demand peaks 40–50% above baseline in May–July** | Begin procurement in February–March to absorb lead times before peak |
| **LocalMart Wholesale scores 0.76 vs MedTech's 0.63** | Shift more volume to local, faster, more reliable suppliers |
| **Healthcare category shipping = 12.3% of spend** (vs 1.5% for Food) | Consolidate orders to EOQ-recommended batch sizes to reduce shipping frequency |
| **SKU-A001 (Rice) has highest stockout rate** | Raise reorder point to recommended 466 units (safety stock: 165 units) |

---

## 🛠️ Tools Used

- **Python** (pandas, numpy) — data generation, cleaning, feature engineering, optimization models
- **Jupyter Notebook** — step-by-step interactive analysis
- **Power BI** — interactive dashboard and visualization
- **Excel** — intermediate cleaned data format

---

## 🚀 How to Run

1. Clone this repository
2. Open `notebooks/Supply_Chain_Analysis.ipynb` in Jupyter
3. Run cells sequentially (Shift + Enter) — generates data, cleans, engineers features, runs models
4. Open `dashboard/Supply_Chain_Dashboard.pbix` in Power BI Desktop to explore the dashboard
5. (Optional) Open `dashboard/supply_chain_dashboard.html` in any browser for a quick visual preview

---

## 📬 Contact

**Chisom Precious**
Feel free to connect for feedback or collaboration.
