# 📉 End-to-End Customer Churn Analysis

A full-stack data analytics project for a **Telecom company** — covering ETL in SQL Server, an interactive Power BI dashboard, and a Machine Learning model to predict future churners.

---

## 🗺️ Project Architecture

```
Raw CSV Data
    │
    ▼
STEP 1 ── SQL Server (ETL)
            │  Staging table → Null handling → Prod table → Views
            ▼
STEP 2 ── Power BI (Transform)
            │  Age groups, Tenure groups, Churn Status column
            ▼
STEP 3 ── Power BI (DAX Measures)
            │  Total Customers, Churn Rate, New Joiners
            ▼
STEP 4 ── Power BI Dashboard (Historical Analysis)
            │  Summary page + Churn Reason tooltip
            ▼
STEP 5 ── Python / Jupyter (ML – Random Forest)
            │  Train on historical data → Predict new churners
            ▼
STEP 6 ── Power BI Dashboard (Predicted Churners Page)
```

---

## 🎯 Project Goals

- Visualize customer data across **Demographic, Geographic, Account & Services** dimensions
- Study churner profiles and identify areas for **marketing campaigns**
- Predict **future churners** using Machine Learning

**Key Metrics tracked:**
- Total Customers
- Total Churn & Churn Rate
- New Joiners

---

## 📁 Repository Structure

```
churn-analysis/
│
├── churn_analysis_enhanced.pbix    # Power BI dashboard (Steps 2–4 & 6)
├── churn_analysis.ipynb            # Python ML model (Step 5)
├── sql/
│   └── etl_queries.sql             # All SQL queries (Step 1)
├── data/
│   └── prediction_data.xlsx        # Exported SQL views for ML model
├── screenshots/
│   ├── summary_page.png
│   ├── churn_reason_page.png
│   └── prediction_page.png
└── README.md
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| **SQL Server + SSMS** | ETL — data loading, cleaning, staging to prod |
| **Power BI Desktop** | Dashboard, DAX measures, Power Query transforms |
| **Python (Jupyter)** | Random Forest churn prediction model |
| **pandas, scikit-learn** | Data preprocessing & ML |
| **matplotlib, seaborn** | Feature importance visualization |

---

## STEP 1 — ETL in SQL Server

- Imported raw CSV into a **staging table** (`stg_Churn`) via Import Wizard
- Explored data: checked distinct values and null counts across all columns
- Cleaned nulls using `ISNULL()` and loaded into production table (`prod_Churn`)
- Created two views for downstream use:
  - `vw_ChurnData` — historical churned/stayed customers (for ML training)
  - `vw_JoinData` — newly joined customers (for ML prediction)

---

## STEP 2 & 3 — Power BI Transform & DAX

**Power Query transformations:**
- Added `Churn Status` column (1 = Churned, 0 = Stayed)
- Created mapping tables for **Age Groups** and **Tenure Groups**
- Unpivoted services columns into a `prod_Services` table

**DAX Measures:**
```
Total Customers = COUNT(prod_Churn[Customer_ID])
Total Churn     = SUM(prod_Churn[Churn Status])
Churn Rate      = [Total Churn] / [Total Customers]
New Joiners     = CALCULATE(COUNT(...), Customer_Status = "Joined")
```

---

## STEP 4 — Power BI Dashboard (Historical)

**Summary Page**
- KPI cards: Total Customers, New Joiners, Total Churn, Churn Rate
- Demographics: Gender & Age Group churn breakdown
- Account info: Payment Method, Contract type, Tenure Group
- Geographic: Top 5 States by Churn Rate
- Churn category distribution with drill-through tooltip

---

## STEP 5 — Machine Learning (Random Forest)

### Install dependencies
```bash
pip install pandas numpy matplotlib seaborn scikit-learn joblib openpyxl
```

### How the model works
1. Loads `vw_ChurnData` from Excel (exported from SQL Server)
2. Drops non-predictive columns (`Customer_ID`, `Churn_Category`, `Churn_Reason`)
3. Label-encodes 19 categorical features
4. Trains a **Random Forest (100 trees)** on 80% of data
5. Evaluates with Confusion Matrix + Classification Report
6. Plots **Feature Importances** chart
7. Runs predictions on `vw_JoinData` (new customers)
8. Exports predicted churners to `Predictions.csv`

---

## STEP 6 — Power BI Dashboard (Predicted Churners)

A dedicated **Churn Prediction Page** built on `Predictions.csv`:
- Count of predicted churners (DAX measure)
- Breakdowns by Gender, Age Group, Marital Status
- Account info: Payment Method, Contract, Tenure Group
- Geographic: State-level churn count
- Customer-level grid with Monthly Charge, Revenue, Refunds, Referrals

---

## 🚀 How to Run This Project

### Prerequisites
- SQL Server + [SSMS](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms)
- [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free)
- [Anaconda](https://docs.anaconda.com/anaconda/install) (includes Jupyter + Python)

### Steps
1. Run `sql/etl_queries.sql` in SSMS to create the database and views
2. Export `vw_ChurnData` and `vw_JoinData` from SQL Server to `prediction_data.xlsx`
3. Run `churn_analysis.ipynb` in Jupyter Notebook to generate `Predictions.csv`
4. Open `churn_analysis_enhanced.pbix` in Power BI Desktop and refresh data sources
## 👤 Author
Swasti Parashar |@swastiparashar522861-max| www.linkedin.com/in/swasti-parashar-178a4a3b8 

## 📜 License

This project is open source and available under the [MIT License](LICENSE).
