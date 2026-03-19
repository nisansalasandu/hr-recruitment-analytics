# Predictive Workforce Planning Using HR Recruitment Analytics

> A data science project that forecasts future hiring needs by analysing historical HR recruitment data, identifying patterns in hiring activity, attrition behaviour, and departmental trends.

---

## Project Overview

| Item | Detail |
|---|---|
| **Dataset** | HRDataset_v14.csv — [Kaggle Human Resources Data Set](https://www.kaggle.com/datasets/rhuebner/human-resources-data-set) |
| **Records** | 311 employees, 36 original columns |
| **Departments** | Production, IT/IS, Sales, Software Engineering, Admin Offices, Executive Office |
| **Hire Year Range** | 2006 – 2018 |
| **Tools** | Python, Jupyter Notebook, pandas, scikit-learn, matplotlib, seaborn |

---

## Repository Structure

```
hr-recruitment-analytics/
│
├── data/
│   ├── raw/
│   │   └── HRDataset_v14.csv              # Original dataset
│   └── processed/
│       └── HRDataset_Cleaned.csv          # Cleaned output from Notebook 01
│
├── notebooks/
│   ├── 01_data_cleaning.ipynb             # Step 1 — Data cleaning & preparation
│   ├── 02_exploratory_analysis.ipynb      # Step 2 — EDA & visualisations
│   ├── 03_forecasting_model.ipynb         # Step 3 — Regression + Attrition models
│   └── 04_visualizations.ipynb                    # Step 4 — Final report notebook with visualizations
│
├── outputs/
│   ├── figures/                           # All saved chart images
│      ├── 01_hiring_trend.png
│      ├── 02_dept_overview.png
│      ├── 03_seasonal_patterns.png
│      ├── 04_termination_reasons.png
│      ├── 05_recruitment_sources.png
│      ├── 06_model_accuracy.png
│      └── 07_forecast_summary.png
│   
│
├── Workforce_Planning_Report.pdf
└── README.md
```

---

## How to Run

### 1. Clone the repository
```bash
git clone https://github.com/nisansalasandu/hr-recruitment-analytics.git
cd hr-recruitment-analytics
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run notebooks in order
```bash
jupyter notebook
```
Open and run each notebook in sequence:
1. `01_data_cleaning.ipynb`
2. `02_exploratory_analysis.ipynb`
3. `03_forecasting_model.ipynb`
4. `04_visualizations.ipynb`

---

## Requirements

```
pandas
numpy
matplotlib
seaborn
scikit-learn
jupyter
openpyxl
```

---

## Analysis Pipeline

### Step 1 — Data Cleaning
- Parsed date columns (`DateofHire`, `DateofTermination`) to datetime
- Handled 207 missing `DateofTermination` values (active employees — expected)
- Investigated and filled 8 missing `ManagerID` values (confirmed as Webster Butler, ID=39)
- Standardised 10 text columns (strip whitespace + Title Case)
- Manually fixed acronyms broken by `.title()` — `IT/IS` and `Still Employed`
- Converted identifier columns from int to string type
- Added 5 derived columns: `HireYear`, `HireMonth`, `HireMonthName`, `TenureYears`, `IsActive`
- **Output:** 311 rows × 41 columns, zero unexpected nulls

### Step 2 — Exploratory Data Analysis
Nine visualisations covering:

| Chart | Key Finding |
|---|---|
| Annual Hiring Trend | Peak in 2011 (83 hires), declining since |
| Hires by Department | Production = 67% of all hiring |
| Seasonal Patterns | January, July, September are peak months |
| Department Trend Over Time | IT/IS shows growing trend |
| Attrition by Department | Production 39.7%, Software Engg 36.4% |
| Termination Reasons | "Another Position" top reason (20) |
| Recruitment Sources | Indeed (28%) + LinkedIn (24.4%) = 52%+ |
| Hiring Heatmap | 2011 most intense year across all months |
| Tenure by Department | IT/IS shortest (3.68 yrs), fastest churn |

### Step 3 — Forecasting Model

**Model 1 — Linear Regression**
- Trained on 2011–2016 annual hire counts (6 data points)
- Slope: **-10.17 hires/year** (declining trend)
- R² = 0.674, MAE = 10.97, RMSE = 12.09, MAPE = 27.6%
- LOO-CV Mean MAE = 16.76 hires
- Raw forecast goes negative by 2019 → adjusted using 3-year moving average baseline of **37 hires/year**

**Model 2 — Attrition-Based**
- Formula: `Predicted Hires = Active Headcount × Historical Attrition Rate`
- Total org-wide replacement hires: **~66 per year**

| Department | Active | Attrition Rate | Predicted Hires/Year | Priority |
|---|---|---|---|---|
| Production | 126 | 39.7% | 50.0 | 🔴 High |
| IT/IS | 40 | 20.0% | 8.0 | 🟡 Medium |
| Sales | 26 | 16.1% | 4.2 | 🟢 Low |
| Software Engineering | 7 | 36.4% | 2.5 | 🔴 High |
| Admin Offices | 7 | 22.2% | 1.6 | 🟡 Medium |

---

## Key Findings

1. **Hiring peaked in 2011** (83 hires) and has been declining. The 3-year moving average of **37 hires/year** is the recommended planning baseline.

2. **Production dominates** — 67% of all hires and 39.7% attrition rate means ~50 replacement hires needed annually.

3. **Voluntary exits are preventable** — "Another Position", "Unhappy", and "More Money" account for 43% of terminations. Retention strategies could significantly reduce future hiring costs.

4. **January, July, September** are peak hiring months — start recruitment campaigns 4–6 weeks earlier.

5. **Indeed + LinkedIn = 52%+ of hires** — concentrate job posting budget on these platforms.

6. **IT/IS shows a growing trend** (regression slope +2.0) — plan for 8–18 hires/year.

---

## Visualisations

### Figure 1 — Annual Hiring Trend & Forecast
![Annual Hiring Trend](outputs/figures/01_hiring_trend.png)

### Figure 2 — Department Overview (Hires & Attrition)
![Department Overview](outputs/figures/02_dept_overview.png)

### Figure 3 — Seasonal Hiring Patterns
![Seasonal Patterns](outputs/figures/03_seasonal_patterns.png)

### Figure 4 — Termination Reasons
![Termination Reasons](outputs/figures/04_termination_reasons.png)

### Figure 5 — Recruitment Sources
![Recruitment Sources](outputs/figures/05_recruitment_sources.png)

### Figure 6 — Model Accuracy (R² + LOO-CV)
![Model Accuracy](outputs/figures/06_model_accuracy.png)

### Figure 7 — Combined Forecast Summary
![Forecast Summary](outputs/figures/07_forecast_summary.png)

---

## Model Accuracy Summary

| Metric | Overall LR | Production | IT/IS | Sales | Software Engg |
|---|---|---|---|---|---|
| R² | 0.674 | 0.826 | 0.280 | 0.180 | 0.812 |
| MAE | 10.97 | 6.80 | 4.33 | 2.19 | 0.56 |
| RMSE | 12.09 | 8.45 | 5.48 | 3.02 | 0.63 |
| LOO-CV MAE | 16.76 | — | — | — | — |

> **Note:** LOO-CV R² returns NaN for single-sample test sets (known sklearn limitation with 6 data points). MAE is used for cross-validation assessment.

---

## Recommendations

| Priority | Area | Recommendation |
|---|---|---|
| 🔴 High | Production | Build continuous pipeline for ~50 hires/year |
| 🔴 High | Software Engineering | Focus on retention — 36.4% attrition in small team |
| 🟡 Medium | IT/IS | Plan for 8–18 hires/year, growing trend |
| 🟡 Medium | Seasonal | Launch campaigns in Dec, Jun, Aug for Jan/Jul/Sep peaks |
| 🟢 Low | Sales | Most stable department — maintain current practices |
| All | Retention | Address "Unhappy" and "More Money" through engagement & comp reviews |
| All | Sourcing | Maintain focus on Indeed and LinkedIn (52%+ of all hires) |

---

## Dataset Credit

Huebner, R. & Patalano, C. Human Resources Data Set. Kaggle.  
https://www.kaggle.com/datasets/rhuebner/human-resources-data-set

---

## License

This project is for educational purposes as part of a Data Science internship.
