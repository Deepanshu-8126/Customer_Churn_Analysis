# Customer Churn Analysis

End-to-end Exploratory Data Analysis (EDA) project identifying key drivers of customer churn, quantifying revenue impact, and delivering actionable retention recommendations — supported by statistical validation and an interactive Power BI dashboard.

---

## 1. Business Problem

The company currently has an overall churn rate of **33.14%** — nearly 1 in every 3 customers has churned. Since acquiring a new customer typically costs 5–7x more than retaining an existing one, this represents a significant and direct revenue risk rather than just an operational metric.

Initial exploratory analysis revealed that **Month-to-month contract customers churn at 46.56%**, compared to **16.75%** (One year) and **16.88%** (Two year) — nearly **3x higher** than long-term contracts. This relationship was statistically validated using a Chi-square test (χ² = 9889.98, p < 0.0001), confirming it is not due to random variation.

**Goal of this project:** Use EDA and statistical validation to identify the true drivers of churn, quantify their business/revenue impact, segment customers by risk level, and provide data-backed recommendations to reduce churn.

---

## 2. Dataset Description

| Column | Description |
|---|---|
| CustomerID | Unique customer identifier |
| Age | Customer age |
| Gender | Male / Female / Other |
| Tenure | Number of months as a customer |
| MonthlyCharges | Monthly billing amount |
| TotalCharges | Total amount billed to date |
| Contract | Month-to-month / One year / Two year |
| PaymentMethod | Bank transfer / Mailed check / Electronic check / Credit card |
| Churn | Yes / No |
| age_bracket | Adult / Middle_age / Senior |
| tenure_bracket | New / Silver / Gold / Diamond |

**Dataset size:** 100,000 rows × 11 columns
**Source:** [Kaggle — Customer Churn Analysis](https://www.kaggle.com/datasets/ashkhan00/customer-churn-analysis)

---

## 3. Project Workflow

```
1. Business Problem Understanding
2. Data Understanding (structure, quality, column meaning)
3. Data Cleaning & Preprocessing
4. Univariate EDA
5. Bivariate / Multivariate EDA (against Churn)
6. Statistical Validation (Chi-square tests)
7. Business Insights & Recommendations
8. Power BI Dashboard
9. Documentation (this README)
```

### Data Cleaning Summary
- Fixed incorrect CSV parsing (semicolon-separated file)
- Converted `TotalCharges` from string to numeric; filled missing values with 0 (new customers with no billing history yet)
- Standardized category columns (`Gender`, `Contract`, `PaymentMethod`, `Churn`) by trimming whitespace
- Created `Churn_Flag` (binary encoding of Churn) for numerical analysis
- Verified no negative/invalid values in Age, Tenure, MonthlyCharges
- Cross-validated `age_bracket` and `tenure_bracket` against their source columns
- Confirmed zero duplicate rows and zero duplicate CustomerIDs

---

## 4. Key Insights

**1. Contract type is the strongest churn driver.**
Month-to-month customers churn at 46.56% vs. 16.75–16.88% for annual/biennial contracts — a ~3x difference, statistically confirmed (Chi-square, p < 0.0001).

**2. Payment method has no significant effect on churn.**
Chi-square test on PaymentMethod returned p = 0.5666, meaning any visual variation in churn rate across payment types is not statistically meaningful. This is not a factor worth prioritizing.

**3. Revenue at risk is substantial and quantifiable.**
Churned customers represent **₹31,27,312 in monthly recurring revenue — 39.10% of total revenue.** This reframes churn from a customer-count problem into a direct revenue leakage problem.

**4. Revenue loss is heavily concentrated in one segment.**
Month-to-month contract customers alone account for **₹23,07,170** of total revenue at risk — roughly **74%** of all revenue loss — despite being addressable through a single lever (contract structure).

**5. Tenure is inversely related to churn.**
Customers with shorter tenure show consistently higher churn rates, indicating the early customer lifecycle is the highest-risk period.

**6. Risk segmentation reveals a compact, high-priority target group.**
Using a weighted rule-based scoring model (Contract type, Tenure, MonthlyCharges), customers were segmented as:
- Low Risk: 38,247 customers (38.2%)
- Medium Risk: 53,335 customers (53.3%)
- High Risk: 8,418 customers (8.4%)

The High Risk group — though smallest — combines multiple compounding risk factors and represents the highest-priority segment for intervention.

---

## 5. Recommendations

1. **Incentivize long-term contracts** — Offer discounts or loyalty benefits to migrate Month-to-month customers toward One/Two-year contracts, given the 3x churn difference and 74% revenue concentration in this segment.

2. **Prioritize the High Risk segment** — Direct retention campaigns (personalized offers, proactive outreach) toward the 8,418 High Risk customers identified via the risk scoring model for maximum ROI.

3. **Deprioritize payment-method-based interventions** — Statistically insignificant (p = 0.5666); retention budget should not be allocated here.

4. **Strengthen early-tenure engagement** — Introduce structured onboarding and engagement touchpoints within the first 12 months, where churn risk is highest.

5. **Report churn as a revenue metric, not just a count** — Shift internal reporting to emphasize revenue-at-risk (₹31.27L/month) to ensure leadership treats churn with appropriate urgency.

---

## 6. Dashboard(working)

An interactive Power BI dashboard was built to track churn rate, revenue at risk, and risk segments in real time.

*(Add dashboard screenshot here: `![Dashboard](dashboard_screenshot.png)`)*

---

## 7. Tools Used

- **Python** — pandas, numpy (data cleaning & analysis)
- **Matplotlib / Seaborn** — data visualization
- **SciPy** — statistical hypothesis testing (Chi-square)
- **Power BI** — interactive dashboard(working)
- **Jupyter Notebook** — analysis environment

---

## 8. Repository Structure

```
├── data/
│   └── Cus_chrn_table.csv
├── notebooks/
│   └── customer_churn.ipynb
├── cleaned_churn_data.csv
├── final_churn_with_insights.csv
├── dashboard/
│   └── churn_dashboard.pbix
├── README.md
```

## 9. How to Run

```bash
git clone <repo-url>
cd customer-churn-analysis
pip install pandas numpy matplotlib seaborn scipy
jupyter notebook notebooks/customer_churn.ipynb
```

---

## Author

*(Deepanshu / LinkedIn / Portfolio link here)*