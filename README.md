# 🛒 Customer Purchase Propensity — Data Cleaning & Feature Engineering Pipeline

---

## 📌 Project Overview

This project builds a **complete data preprocessing and feature engineering pipeline** for an e-commerce company. The goal is to clean, transform, and engineer meaningful features from raw, multi-source data so that a Machine Learning model can later predict **whether a customer will make a purchase** (binary classification: `purchased = 1` or `0`).

> **No model is trained** — the focus is entirely on the data pipeline, as instructed.

---

## 🧩 Data Sources

| Source | File | Description |
|--------|------|-------------|
| CSV | `customers.csv` | 8 base customer records (demographics) |
| JSON | `transactions.json` | 8 transaction records |
| SQL | `products.sql` | 6 product rows, loaded via SQLite |
| API | `https://dummyjson.com/users` | 1,000 user records (schema replicated synthetically in sandbox) |

> **Note on API:** The dummyjson.com endpoint was blocked in the sandbox environment. The full API schema (name, age, gender, city, income, education, satisfaction, signup_date, etc.) was faithfully replicated to generate 1,000 realistic Indian e-commerce users. The notebook documents this and is immediately switchable to the live API by replacing the data generation block.

---

## 📁 Repository Structure

```
📦 customer-purchase-propensity/
├── 📓 DataPreprocessing.ipynb      ← Main Jupyter Notebook (all 10 steps)
├── 📊 processed_customer_data.csv  ← Final cleaned & feature-engineered dataset
├── 📄 Summary_Report.docx          ← 1-page summary report
├── 📂 data/
│   ├── customers.csv
│   ├── transactions.json
│   └── products.sql
└── 📖 README.md                    ← This file
```

---

## 🔬 Pipeline Steps (All 10 Covered)

### Step 1 — Project Planning & Problem Framing
- What is Data Analysis & steps in a DS project
- Binary classification framing: predict `purchased` (0 or 1)

### Step 2 — Data Import & Understanding
- Load CSV, JSON, SQLite, and API data
- Merge on `customer_id` and `product_id`
- `.info()`, `.describe()`, `.head()` exploration

### Step 3 — Exploratory Data Analysis (EDA)
- **Univariate:** Histograms, value counts, skewness identification
- **Bivariate:** Box plots, bar charts, scatter plots vs target
- **Multivariate:** Correlation heatmap, pairplot coloured by `purchased`

### Step 4 — Handling Missing Data
| Technique | Applied To |
|-----------|-----------|
| SimpleImputer (median) | age, income, total_spend, num_transactions |
| Most Frequent | gender, city, education, satisfaction, fav_category |
| Missing Indicator | income (flag: `income_missing`) |
| KNN Imputer (k=5) | total_purchases |
| MICE / IterativeImputer | income, total_spend, total_purchases |
| Complete Case Analysis | Reference comparison only |

### Step 5 — Outlier Detection & Handling
- **Z-Score** (threshold = 3): income=11, total_spend=11
- **IQR Method** (1.5×IQR): income=39, total_spend=19
- **Percentile** (1st–99th): income=22, total_spend=22
- **Winsorization** applied to income, total_spend, total_txn_amount

### Step 6 — Date/Time & Mixed Variables
- `signup_date` & `last_purchase_date` → converted to datetime
- Derived: `days_since_signup`, `days_since_last_purchase`, `signup_year`, `signup_month`
- Mixed variable `phone_raw` (e.g. `C0101-PH`) → `customer_id_extracted` via regex

### Step 7 — Encoding Categorical Data
- **Label Encoding** → `gender`
- **One-Hot Encoding** → `city`, `fav_category`
- **Ordinal Encoding** → `education` (5-level scale), `satisfaction` (5-level scale)
- **Numerical Binning** → `income` into Low/Medium/High/Very High quantile groups

### Step 8 — Feature Scaling
- StandardScaler, MinMaxScaler, MaxAbsScaler, RobustScaler, Normalizer — all demonstrated
- **ColumnTransformer** applied (StandardScaler for first 3 cols, RobustScaler for rest)
- **RobustScaler** selected for the final pipeline (best for outlier-heavy data)

### Step 9 — Feature Construction & Transformation
- `purchase_per_day` = `total_purchases / (days_since_signup + 1)`
- `spend_per_purchase` = `total_spend / (total_purchases + ε)`
- **FunctionTransformer** → Log, Square Root, Reciprocal
- **PowerTransformer** → Yeo-Johnson & Box-Cox
- Income binning (equal-width, 5 bins)
- `frequent_buyer` = 1 if `total_purchases > 75th percentile`

### Step 10 — Final Output
- Exported `processed_customer_data.csv` (1,008 rows × 48 columns)
- Summary report generated
- All code documented in notebook

---

## 📊 Final Dataset Stats

| Metric | Value |
|--------|-------|
| Total Records | 1,008 |
| Total Features | 48 |
| Missing Values (post-pipeline) | 0 |
| Target Balance | ~55% purchased, ~45% not |
| New Engineered Features | 15+ |

---

## 🧰 Tech Stack

| Library | Purpose |
|---------|---------|
| `pandas` | Data loading, merging, manipulation |
| `numpy` | Numerical operations |
| `scikit-learn` | Imputers, scalers, encoders, transformers |
| `scipy` | Z-score outlier detection |
| `matplotlib` | Plotting |
| `seaborn` | Statistical visualisations |
| `sqlite3` | SQL product data loading |
| `json` | Transactions loading |

---

## 🚀 How to Run

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/customer-purchase-propensity.git
cd customer-purchase-propensity

# 2. Install dependencies
pip install pandas numpy scikit-learn matplotlib seaborn scipy jupyter

# 3. Copy data files
cp customers.csv transactions.json products.sql ./

# 4. Launch notebook
jupyter notebook DataPreprocessing.ipynb
```

---

## 📋 Deliverables Checklist

- [x] `DataPreprocessing.ipynb` — Python Notebook with all 10 steps
- [x] `processed_customer_data.csv` — Final feature-engineered CSV (1,008 rows)
- [x] `Summary_Report.docx` — Theory + Observations (max 1 page)
- [x] `README.md` — This file

---

## 📬 Contact

**Project by:** [Nandish Patel]  
**Role:** Data Analyst  
**Organisation:** RNW
**Date:** April 2026

---
