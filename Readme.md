# Customer Churn Analysis

An end-to-end Exploratory Data Analysis (EDA) project to analyze customer churn patterns, quantify revenue loss, segment customers based on churn risk, and provide actionable business recommendations — backed by statistical validation (Chi-square test) and an interactive Power BI dashboard.

---

## 1. Business Problem

The business is experiencing an overall customer churn rate of **33.14%** (approx. 1 in every 3 customers leaves). Acquiring a new customer is significantly more expensive than retaining an existing one, making churn reduction a critical priority.

**Key Objectives:**
- Identify the primary factors driving customer churn.
- Validate whether observed patterns are statistically significant or due to chance.
- Calculate the actual revenue at risk (Monthly Recurring Revenue lost).
- Segment customers by risk levels (Low, Medium, High).
- Provide practical recommendations to improve customer retention.

---

## 2. Dataset Description

The dataset contains **100,000 customer records** across 11 features.

| Column | Description |
|---|---|
| `CustomerID` | Unique identifier for each customer |
| `Age` | Age of the customer (18 to 70) |
| `Gender` | Male / Female / Other |
| `Tenure` | Duration of customer relationship in months (1 to 60) |
| `MonthlyCharges` | Current monthly bill amount (in ₹) |
| `TotalCharges` | Cumulative amount billed till date |
| `Contract` | Contract duration (`Month-to-month`, `One year`, `Two year`) |
| `PaymentMethod` | Payment channel (`Bank transfer`, `Credit card`, `Electronic check`, `Mailed check`) |
| `Churn` | Churn status (`Yes` / `No`) |
| `age_bracket` | Categorized age (`Adult`, `Middle_age`, `Senior`) |
| `tenure_bracket` | Customer lifecycle stage (`New`, `Silver`, `Gold`, `Diamond`) |

**Source:** [Kaggle — Customer Churn Dataset](https://www.kaggle.com/datasets/ashkhan00/customer-churn-analysis)

---

## 3. Project Workflow & Data Cleaning

```
Raw Data (data/raw) ──> Data Cleaning ──> EDA & Statistical Tests ──> Risk Segmentation ──> Power BI Dashboard ──> Recommendations
```

### Data Cleaning Steps Performed:
- **Parsed File Properly:** Resolved semicolon (`;`) delimiter and quotes in the raw CSV.
- **Type Conversion & Null Handling:** Converted `TotalCharges` to numeric and imputed missing values with `0` (for new customers with 0 tenure).
- **Standardized Text:** Cleaned and stripped extra whitespaces across categorical columns (`Gender`, `Contract`, `PaymentMethod`, `Churn`).
- **Feature Encoding:** Created `Churn_Flag` (`1` for Yes, `0` for No) for numeric and correlation calculations.
- **Data Validation:** Verified zero duplicate records, verified consistency of `age_bracket` and `tenure_bracket` with underlying values, and checked that numeric fields had no negative values.

---

## 4. Key Insights & Findings

### 1. Contract Type is the Primary Driver of Churn
- **Month-to-month contracts:** **46.56% churn rate**
- **One year contracts:** **16.75% churn rate**
- **Two year contracts:** **16.88% churn rate**
- Customers on month-to-month contracts are **~3x more likely to churn** compared to annual contract holders.

### 2. Statistical Validation (Chi-Square Test)
To verify if differences across categories were statistically significant, Chi-square tests of independence were conducted ($\alpha = 0.05$):

| Factor | Test | Result | Conclusion |
|---|---|---|---|
| **Contract** | Chi-square ($\chi^2 = 9889.98$) | $p < 0.0001$ | **Highly Significant:** Contract type directly impacts churn. |
| **Payment Method** | Chi-square | $p = 0.5666$ | **Not Significant:** Churn is uniform (~33%) across all payment methods. No retention budget should be wasted here. |
| **Gender** | Chi-square | $p > 0.05$ | **Not Significant:** Gender has no measurable influence on churn. |

### 3. Tenure vs. Churn
- Churn is highest among newer customers (`Tenure < 12 months`).
- As tenure increases (`Gold` and `Diamond` tiers), churn rates drop substantially.

### 4. Monthly Charges
- Customers who churn tend to have slightly higher median monthly charges compared to retained customers.

---

## 5. Revenue at Risk Analysis

Looking at churn strictly as customer count understates the business impact. The monthly revenue loss was calculated directly:

- **Total Monthly Revenue:** ₹79,98,450
- **Monthly Revenue at Risk (Churned):** **₹31,27,312 (39.10% of total revenue)**

### Loss Breakdown by Contract Type:

| Contract Type | Churned Customers | Monthly Revenue Lost | % of Total Lost Revenue |
|---|:---:|:---:|:---:|
| **Month-to-month** | 23,280 | **₹23,07,170** | **73.77%** |
| **One year** | 4,188 | ₹4,11,850 | 13.17% |
| **Two year** | 4,220 | ₹4,08,292 | 13.06% |
| **Total** | **33,140** | **₹31,27,312** | **100%** |

> **Key Takeaway:** Month-to-month customers account for **~74% of all revenue loss**.

---

## 6. Customer Risk Scoring & Segmentation

To help the retention team prioritize outreach, a simple rule-based risk score was built combining the top churn factors:

- **+2 points** if Contract is `Month-to-month`
- **+2 points** if Tenure is `< 12 months`
- **+1 point** if MonthlyCharges is above the median

```python
df['Risk_Score'] = (
    (df['Contract'] == 'Month-to-month').astype(int) * 2 +
    (df['Tenure'] < 12).astype(int) * 2 +
    (df['MonthlyCharges'] > df['MonthlyCharges'].median()).astype(int) * 1
)
df['Risk_Level'] = pd.cut(df['Risk_Score'], bins=[-1, 1, 3, 5], labels=['Low', 'Medium', 'High'])
```

### Risk Level Distribution:

| Risk Level | Score | Customer Count | % of Total | Action |
|---|:---:|:---:|:---:|---|
| **Low** | 0 – 1 | 38,247 | 38.2% | Standard loyalty programs & regular engagement |
| **Medium** | 2 – 3 | 53,335 | 53.3% | Periodic check-ins, feature adoption nudges |
| **High** | 4 – 5 | **8,418** | **8.4%** | **Immediate proactive intervention & contract upgrade offers** |

---

## 7. Business Recommendations

| # | Recommendation | Target Group | Expected Outcome |
|---|---|---|---|
| **1** | **Incentivize Annual Contracts** | Month-to-month customers | Offer discounts (e.g., 10–15% off) for moving to 1-year plans to tackle the 74% revenue leakage. |
| **2** | **Focus Outreach on High-Risk Segment** | 8,418 High-Risk customers | Direct customer success resources to this specific cohort for maximum retention ROI. |
| **3** | **Strengthen Early Onboarding** | New customers (< 12m tenure) | Introduce guided onboarding and early milestone check-ins to reduce first-year drop-offs. |
| **4** | **Avoid Payment Method Campaigns** | All customers | Chi-square test confirmed $p = 0.5666$. Avoid spending marketing budget on payment channel offers. |
| **5** | **Track Revenue at Risk in Executive KPIs** | Leadership reporting | Report churn in terms of lost monthly revenue (₹31.27 Lakhs) rather than just percentage to maintain visibility on revenue impact. |

---

## 8. Power BI Dashboard

An interactive dashboard (`dashboard/Customer_Churn_Analysis.pbix`) was created to visualize:
- Overall churn rate and monthly revenue at risk.
- Churn breakdown across contract types, tenure brackets, and age groups.
- Risk level distribution with interactive filters for operations and retention teams.

---

## 9. Repository Structure

```
Customer_Churn-analysis/
├── data/
│   ├── raw/
│   │   └── Cus_chrn_table.csv              # Original dataset (100,000 rows)
│   └── processed/
│       ├── cleaned_churn_data.csv          # Cleaned dataset
│       └── final_churn_with_insights.csv   # Dataset with Risk_Score & Risk_Level
├── notebooks/
│   └── customer_churn.ipynb                # Complete EDA, cleaning & risk scoring notebook
├── dashboard/
│   └── Customer_Churn_Analysis.pbix        # Power BI report
├── .gitignore
├── requirements.txt
└── README.md
```

---

## 10. Tools & Libraries Used

- **Python** (Pandas, NumPy) — Data cleaning and feature engineering
- **Matplotlib & Seaborn** — Data visualization and EDA
- **SciPy** (`scipy.stats.chi2_contingency`) — Hypothesis testing (Chi-square test)
- **Power BI** — Interactive KPI dashboard
- **Jupyter Notebook** — Analysis environment

---

## 11. How to Run

1. **Clone the repo:**
   ```bash
   git clone https://github.com/Deepanshu-8126/Customer_Churn_Analysis.git
   cd Customer_Churn_Analysis
   ```
2. **Install requirements:**
   ```bash
   pip install -r requirements.txt
   ```
3. **Open the notebook:**
   ```bash
   jupyter notebook notebooks/customer_churn.ipynb
   ```
4. **Open the Power BI file:**
   - Open `dashboard/Customer_Churn_Analysis.pbix` in Power BI Desktop.

---

## Author

**Deepanshu Kapri**  
- GitHub: [@Deepanshu-8126](https://github.com/Deepanshu-8126)
